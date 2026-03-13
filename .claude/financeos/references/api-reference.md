# FINANCE:OS — Dangerous API Endpoints Reference

**HARD GATE — NEVER call any endpoint in this file without explicit user confirmation showing the exact blast radius and dollar amount at risk.**

These are financial APIs where mistakes cost real money. Refunds cannot be un-refunded. Transfers cannot be recalled. Deleted records break audit trails. Every call in this document has the potential to cause irreversible financial damage.

Last updated: 2026-03-13

---

## Universal Rules (Apply to ALL Financial Platforms)

1. **Never call a money-moving endpoint without showing the user the exact amount, currency, and destination**
2. **Never call a DELETE endpoint on any financial record** — prefer voiding, archiving, or marking inactive
3. **Never batch-process destructive financial operations** — always one-at-a-time with confirmation
4. **Log every destructive API call** in `logs/finance-decisions.md` with full context, amount, and timestamp
5. **Always verify idempotency keys** on money-moving endpoints to prevent duplicate charges/transfers
6. **Never store API keys with write access in code** — use environment variables or secrets manager
7. **Test mode first** — every platform below has a sandbox/test mode. Use it before touching live data
8. **Reconcile after every destructive operation** — verify the ledger matches expected state
9. **When in doubt, the safe alternative is always "do nothing and ask"**
10. **Respect rate limits with wide margins** — never exceed 50% of documented limits on financial APIs

---

## 1. Stripe API (api.stripe.com)

**Base URL:** `https://api.stripe.com/v1`
**Auth:** Bearer token (`Authorization: Bearer sk_live_...`) or Basic auth with secret key as username
**Test mode:** Use `sk_test_...` keys — completely separate from live data
**Rate limit:** 100 read requests/sec, 100 write requests/sec in live mode. 25 read/sec in test mode. Returns `429` when exceeded.
**API version:** Set via `Stripe-Version` header. Breaking changes are versioned.

### 1.1 Customer Deletion

| Detail | Value |
|---|---|
| **Endpoint** | `DELETE /v1/customers/{customer_id}` |
| **Irreversible** | YES — permanently deletes the customer object |
| **Blast radius** | CRITICAL — cascading destruction |

**What gets destroyed:**
- The customer object itself (permanently deleted, returns `deleted: true`)
- All active subscriptions on that customer are **immediately cancelled** (not at period end — immediate)
- All saved payment methods (cards, bank accounts) are detached and deleted
- All discount/coupon associations are removed
- Tax IDs attached to the customer are deleted

**What is retained:**
- Past charges, invoices, and payment intents remain in Stripe (with customer reference now showing `deleted`)
- Refunds on past payments are still possible even after customer deletion
- Events and logs referencing the customer are retained

**Safe alternative:** Do NOT delete customers. Instead:
- Remove payment methods: `DELETE /v1/customers/{id}/sources/{source_id}`
- Cancel subscriptions individually: `POST /v1/subscriptions/{sub_id}` with `cancel_at_period_end: true`
- Update metadata to mark as `"status": "archived"`

**Fee implications:** None directly, but if active subscriptions are cancelled mid-cycle, you lose that recurring revenue immediately with no proration credit back to you.

---

### 1.2 Refunds

| Detail | Value |
|---|---|
| **Endpoint** | `POST /v1/refunds` |
| **Irreversible** | YES — once processed, a refund cannot be reversed |
| **Blast radius** | HIGH — real money leaves your account |

**Parameters:**
- `charge` or `payment_intent` (required) — the original charge/PI to refund
- `amount` (optional) — partial refund in cents. Omitting refunds the full amount
- `reason` (optional) — `duplicate`, `fraudulent`, `requested_by_customer`

**Critical details:**
- **Stripe keeps the original processing fee.** A $100 charge with 2.9% + $0.30 fee: if you refund $100, you lose the $100 AND the $3.20 processing fee. Total cost: $103.20.
- Refund window: up to 180 days after the original charge (after that, use a manual payout or credit)
- Partial refunds are allowed — multiple partial refunds up to the original charge amount
- Refunds take 5-10 business days to appear on the customer's statement
- If the customer's card has expired or been cancelled, the refund goes through the card network anyway (the issuing bank handles it)
- **Cannot refund a refund** — there is no "undo refund" endpoint

**Cancelling a pending refund:**
- `POST /v1/refunds/{refund_id}/cancel` — only works if refund status is `pending` (very short window, mainly for bank transfers)
- Once status is `succeeded`, it is permanent

**Safe alternative:**
- Issue store credit or account balance instead: `POST /v1/customers/{id}/balance_transactions` with a negative amount
- This keeps the money in your Stripe account and gives the customer a credit

---

### 1.3 Subscription Cancellation

| Detail | Value |
|---|---|
| **Endpoint** | `DELETE /v1/subscriptions/{subscription_id}` (immediate cancel) |
| **Also** | `POST /v1/subscriptions/{subscription_id}` with `cancel_at_period_end: true` (graceful cancel) |
| **Irreversible** | The DELETE is irreversible. The `cancel_at_period_end` can be reversed before period end. |
| **Blast radius** | HIGH — stops recurring revenue |

**Immediate cancellation (`DELETE`):**
- Subscription status moves to `cancelled` immediately
- No further invoices are generated
- If `prorate` is true (default), a prorated credit is generated — but NOT automatically refunded
- Customer loses access immediately (if your system checks subscription status)
- **Cannot be reactivated** — must create a new subscription

**Graceful cancellation (`cancel_at_period_end: true`):**
- Subscription remains `active` until the current billing period ends
- Customer retains access until period end
- Can be reversed by setting `cancel_at_period_end: false` before the period ends
- Generates a `customer.subscription.updated` webhook event

**Safe alternative:** Always use `cancel_at_period_end: true` unless the customer explicitly demands immediate cancellation. You can reverse it if they change their mind.

**Fee implications:** No cancellation fee from Stripe, but you lose the recurring revenue. If using Stripe Billing with metered usage, final invoice is generated on cancellation.

---

### 1.4 Payouts

| Detail | Value |
|---|---|
| **Endpoint** | `POST /v1/payouts` (create manual payout) |
| **Also** | `POST /v1/payouts/{payout_id}/cancel` (cancel pending payout) |
| **Irreversible** | YES once the payout leaves `pending` status |
| **Blast radius** | CRITICAL — moves real money to your bank account |

**Create payout:**
- `amount` (required) — in cents
- `currency` (required)
- `destination` (optional) — specific bank account. Defaults to the default external account
- `method` — `standard` (1-2 days) or `instant` (minutes, but costs 1% fee, min $0.50, capped at $5.00)

**Cancel payout:**
- Only works while status is `pending` (typically a few-hour window)
- Once status is `in_transit` or `paid`, cannot be cancelled
- A failed payout (bank rejection) returns funds to your Stripe balance

**Reversing a completed payout:** Not possible through the API. You must handle this through your bank.

**Fee implications:**
- Standard payouts: free
- Instant payouts: 1% of payout amount (min $0.50, max $5.00)
- Cross-border payouts: additional 0.25%-1% depending on destination country

---

### 1.5 Webhook Endpoint Deletion

| Detail | Value |
|---|---|
| **Endpoint** | `DELETE /v1/webhook_endpoints/{webhook_endpoint_id}` |
| **Irreversible** | YES — the endpoint and its signing secret are permanently deleted |
| **Blast radius** | HIGH — you stop receiving events, potentially breaking your entire integration |

**What happens:**
- Stripe immediately stops sending events to this URL
- The webhook signing secret is destroyed — you cannot recover it
- Any events that were queued for delivery are lost
- Your application stops receiving payment confirmations, subscription updates, dispute notifications, etc.

**Safe alternative:**
- Disable instead of delete: `POST /v1/webhook_endpoints/{id}` with `disabled: true`
- This preserves the endpoint configuration and signing secret
- Can be re-enabled later

---

### 1.6 Dispute Handling

| Detail | Value |
|---|---|
| **Endpoint** | `POST /v1/disputes/{dispute_id}/close` |
| **Irreversible** | YES — accepting a dispute means you lose the money permanently |
| **Blast radius** | CRITICAL — you forfeit the charge amount AND the $15 dispute fee |

**What happens when you close/accept a dispute:**
- The disputed amount is permanently deducted from your balance
- A $15.00 dispute fee is charged (non-refundable regardless of outcome)
- Your dispute rate increases (Stripe monitors this — too many disputes can get your account terminated)
- The customer keeps their money

**Safe alternative:** Always submit evidence via `POST /v1/disputes/{dispute_id}` before closing. Even weak evidence is better than accepting the loss.

---

### 1.7 Other Dangerous Stripe Endpoints

| Endpoint | Method | Blast Radius | Notes |
|---|---|---|---|
| `/v1/products/{id}` | `DELETE` | Medium | Deletes product; prices linked to it become orphaned |
| `/v1/prices/{id}` | `POST` with `active: false` | Low | Prices cannot be deleted, only deactivated (safe by design) |
| `/v1/coupons/{id}` | `DELETE` | Medium | Coupon deleted; existing discounts using it remain but coupon cannot be reapplied |
| `/v1/invoices/{id}` | `DELETE` | High | Only works on draft invoices. Finalized invoices cannot be deleted — must void instead |
| `/v1/invoices/{id}/void` | `POST` | High | Marks invoice as uncollectible. Irreversible. |
| `/v1/payment_methods/{id}/detach` | `POST` | Medium | Removes payment method from customer. Can disrupt future billing. |
| `/v1/accounts/{id}` | `DELETE` | CRITICAL | Deletes a connected account (Stripe Connect). All data, payouts, and balance are affected. |
| `/v1/subscription_items/{id}` | `DELETE` | High | Removes a line item from a subscription. Prorated by default. |
| `/v1/plans/{id}` | `DELETE` | High | Legacy. Deletes a plan. Existing subscriptions using it continue but plan cannot be reused. |

---

## 2. QuickBooks Online API (quickbooks.api.intuit.com)

**Base URL:** `https://quickbooks.api.intuit.com/v3/company/{realmId}`
**Auth:** OAuth 2.0 Bearer token. Tokens expire every 60 minutes. Refresh tokens expire after 100 days.
**Sandbox:** `https://sandbox-quickbooks.api.intuit.com/v3/company/{realmId}`
**Rate limit:** 500 requests per minute per realm (company). Throttled at 10 concurrent requests. Returns `429` or `503` when exceeded.
**Important:** QuickBooks uses "sparse update" — you can send partial objects. This is dangerous because omitted fields in a non-sparse update will be BLANKED.

### 2.1 Invoice Deletion

| Detail | Value |
|---|---|
| **Endpoint** | `POST /v3/company/{realmId}/invoice?operation=delete` |
| **Irreversible** | YES — the invoice is permanently removed from the ledger |
| **Blast radius** | CRITICAL — breaks accounting records and audit trail |

**What happens:**
- The invoice is permanently deleted from QuickBooks
- The invoice number becomes reusable (can cause confusion)
- If payments were applied to this invoice, those payment links are broken
- Financial reports retroactively change — P&L, balance sheet, AR aging all shift
- **Tax filings that referenced this invoice are now inconsistent**

**Requirements:**
- Must send the full invoice object with `SyncToken` in the request body
- The `SyncToken` must match the current version (optimistic locking)

**Safe alternative:** ALWAYS void instead of delete:
- `POST /v3/company/{realmId}/invoice?operation=void`
- This zeroes out the invoice but keeps it in the ledger for audit trail
- The invoice number remains used (no duplicates)
- Historical reports retain the record

---

### 2.2 Payment Voiding/Deletion

| Detail | Value |
|---|---|
| **Endpoint (void)** | `POST /v3/company/{realmId}/payment?operation=void` |
| **Endpoint (delete)** | `POST /v3/company/{realmId}/payment?operation=delete` |
| **Irreversible** | Both are effectively irreversible |
| **Blast radius** | CRITICAL — affects bank reconciliation and AR |

**Void vs Delete:**
- **Void:** Payment record remains but amount is zeroed. The linked invoice returns to "unpaid" status. Bank reconciliation shows the void. Audit trail is preserved.
- **Delete:** Payment record is permanently removed. The linked invoice returns to "unpaid." Bank reconciliation is broken. Audit trail is destroyed.

**Cascade effects:**
- If the payment was deposited (grouped into a bank deposit), voiding/deleting the payment may affect the deposit record
- Applied credits or discounts on the payment are reversed
- If this payment settled an invoice that was part of a statement, the statement is now inaccurate

**Safe alternative:** Always void, never delete. If a payment was made in error, void it and create a refund receipt to document the correction.

---

### 2.3 Journal Entry Deletion

| Detail | Value |
|---|---|
| **Endpoint** | `POST /v3/company/{realmId}/journalentry?operation=delete` |
| **Irreversible** | YES |
| **Blast radius** | CRITICAL — journal entries are the foundation of double-entry bookkeeping |

**What happens:**
- The journal entry is permanently removed
- Both the debit and credit sides are removed from the general ledger
- Account balances retroactively change
- Trial balance, P&L, and balance sheet all shift
- If this JE was an adjusting entry for period close, the closed period is now wrong

**Safe alternative:** Create a reversing journal entry (same accounts, opposite amounts) dated in the current period. This preserves the audit trail and keeps closed periods intact.

---

### 2.4 Account Deletion

| Detail | Value |
|---|---|
| **Endpoint** | `POST /v3/company/{realmId}/account?operation=update` with `Active: false` |
| **Note** | QuickBooks does not allow true deletion of accounts — only deactivation |
| **Blast radius** | Medium — deactivated accounts are hidden but data is preserved |

**What happens when deactivating:**
- Account is hidden from chart of accounts and dropdown menus
- Historical transactions using this account are preserved
- Reports can still show the account if "include inactive" is selected
- Cannot deactivate an account that has a non-zero balance
- Sub-accounts become orphaned if parent is deactivated

**This is actually safe by design** — QuickBooks prevents true account deletion to protect the ledger.

---

### 2.5 Other Dangerous QuickBooks Endpoints

| Endpoint | Operation | Blast Radius | Notes |
|---|---|---|---|
| `/v3/company/{realmId}/bill?operation=delete` | DELETE | High | Deletes vendor bill; affects AP aging and 1099 reporting |
| `/v3/company/{realmId}/billpayment?operation=delete` | DELETE | High | Deletes bill payment; vendor balance reverts, bank recon breaks |
| `/v3/company/{realmId}/creditmemo?operation=delete` | DELETE | High | Deletes credit memo; customer credits vanish |
| `/v3/company/{realmId}/deposit?operation=delete` | DELETE | CRITICAL | Deletes bank deposit; all grouped payments become undeposited |
| `/v3/company/{realmId}/purchaseorder?operation=delete` | DELETE | Medium | Removes PO from vendor workflow |
| `/v3/company/{realmId}/vendor?operation=update` (Active: false) | Deactivate | Medium | Hides vendor; 1099 data preserved |
| `/v3/company/{realmId}/estimate?operation=delete` | DELETE | Low | Deletes estimate; no financial impact unless converted to invoice |
| `/v3/company/{realmId}/transfer?operation=delete` | DELETE | High | Deletes bank-to-bank transfer record; reconciliation breaks |

**Critical QuickBooks note:** The `?operation=delete` pattern requires sending the full entity object (with matching `SyncToken`) in the POST body. This is an unusual pattern — it's a POST that acts as a DELETE. Automated systems must be careful not to accidentally trigger this by constructing URLs with query parameters.

---

## 3. Xero API (api.xero.com)

**Base URL:** `https://api.xero.com/api.xro/2.0`
**Auth:** OAuth 2.0. Access tokens expire every 30 minutes. Must include `Xero-tenant-id` header.
**Rate limit:** 60 calls per minute per tenant (per app connection). 5,000 calls per day. Also a concurrent limit of 5 requests. Returns `429` when exceeded.
**Important:** Xero uses a "minute rate limit" that is very aggressive — 1 call per second average. Plan accordingly.

### 3.1 Invoice Voiding

| Detail | Value |
|---|---|
| **Endpoint** | `POST /api.xro/2.0/Invoices/{InvoiceID}` with `Status: "VOIDED"` |
| **Irreversible** | YES — a voided invoice cannot be un-voided |
| **Blast radius** | HIGH — affects revenue recognition and tax reporting |

**What happens:**
- Invoice status changes to `VOIDED`
- The invoice amount is zeroed in all reports
- If payments were allocated to this invoice, they must be removed FIRST (Xero will reject the void otherwise)
- The invoice number is consumed (cannot be reused)
- Credit notes applied to this invoice must be deallocated first

**Requirements:**
- Invoice must be in `AUTHORISED` or `SUBMITTED` status to void
- `PAID` invoices cannot be voided — you must remove the payment allocation first
- `DRAFT` invoices can be deleted entirely (see below)

**Safe alternative:** For draft invoices, deletion is acceptable. For authorized invoices, voiding is the correct approach (it IS the safe alternative vs. trying to delete records from accounting history).

---

### 3.2 Invoice Deletion

| Detail | Value |
|---|---|
| **Endpoint** | `POST /api.xro/2.0/Invoices/{InvoiceID}` with `Status: "DELETED"` |
| **Irreversible** | YES |
| **Blast radius** | Medium (only allowed on DRAFT invoices) |

**What happens:**
- Only works on `DRAFT` status invoices
- The invoice is permanently removed
- Invoice number becomes reusable
- No financial impact since drafts haven't been booked to the ledger

**Xero protects against dangerous deletion** by only allowing deletion of drafts. This is a well-designed safety guardrail.

---

### 3.3 Payment Deletion

| Detail | Value |
|---|---|
| **Endpoint** | `PUT /api.xro/2.0/Payments/{PaymentID}` with `Status: "DELETED"` |
| **Irreversible** | YES |
| **Blast radius** | CRITICAL — affects bank reconciliation and invoice status |

**What happens:**
- The payment record is removed
- The associated invoice returns to `AUTHORISED` (unpaid) status
- Bank reconciliation for this payment is undone
- If part of a batch payment, only this payment within the batch is affected
- Overpayments/prepayments allocated via this payment are deallocated

**Requirements:**
- Cannot delete payments that are part of a locked period
- Cannot delete if the payment has been part of a BAS/GST/VAT return that's been filed

**Safe alternative:** There is no "void" for payments in Xero — deletion is the only reversal mechanism. However, you should:
1. Create a credit note for the invoice
2. Reconcile the bank transaction as a refund
3. This preserves the audit trail better than payment deletion

---

### 3.4 Contact Archival

| Detail | Value |
|---|---|
| **Endpoint** | `POST /api.xro/2.0/Contacts/{ContactID}` with `ContactStatus: "ARCHIVED"` |
| **Irreversible** | No — contacts can be unarchived |
| **Blast radius** | Low-Medium |

**What happens:**
- Contact is hidden from active contact lists
- Cannot create new invoices, bills, or transactions for this contact
- Historical transactions are fully preserved
- Can be unarchived by setting `ContactStatus: "ACTIVE"`

**This is a safe operation** — it's the Xero equivalent of soft-delete.

---

### 3.5 Bank Transaction Removal

| Detail | Value |
|---|---|
| **Endpoint** | `POST /api.xro/2.0/BankTransactions/{BankTransactionID}` with `Status: "DELETED"` |
| **Irreversible** | YES |
| **Blast radius** | HIGH — affects bank reconciliation and financial reporting |

**What happens:**
- The manually created bank transaction (spend/receive money) is removed
- Bank reconciliation is affected if this transaction was reconciled
- P&L and balance sheet shift retroactively
- Only works on transactions with `AUTHORISED` status
- Does NOT work on bank statement lines imported from feeds — those cannot be deleted via API

**Safe alternative:** Xero does not have a "void" for bank transactions. If you need to reverse a manually entered transaction, create an opposite transaction (e.g., if you entered a $500 spend, enter a $500 receive with a reference like "Reversal of [original ref]").

---

### 3.6 Other Dangerous Xero Endpoints

| Endpoint | Method | Blast Radius | Notes |
|---|---|---|---|
| `/api.xro/2.0/CreditNotes/{id}` | POST (Status: DELETED) | High | Only works on DRAFT credit notes |
| `/api.xro/2.0/ManualJournals/{id}` | POST (Status: VOIDED) | CRITICAL | Voids manual journal; affects trial balance |
| `/api.xro/2.0/Overpayments/{id}/Allocations` | DELETE | High | Removes overpayment allocation; invoice returns to unpaid |
| `/api.xro/2.0/Prepayments/{id}/Allocations` | DELETE | High | Removes prepayment allocation |
| `/api.xro/2.0/BatchPayments/{id}` | POST (Status: DELETED) | CRITICAL | Deletes entire batch; all payments in batch are removed |
| `/api.xro/2.0/RepeatingInvoices/{id}` | POST (Status: DELETED) | Medium | Stops future auto-generated invoices |
| `/api.xro/2.0/Items/{id}` | DELETE | Medium | Deletes inventory item; historical line items show "deleted item" |

---

## 4. Mercury API (api.mercury.com)

**Base URL:** `https://api.mercury.com/api/v1`
**Auth:** Bearer token (`Authorization: Bearer <token>`). API tokens are created in Mercury dashboard.
**Rate limit:** Not publicly documented with specific numbers. Mercury recommends "reasonable use" and may throttle without warning.
**Important:** Mercury's API is primarily read-only for account data, with limited write capabilities for transfers. This is a safety-conscious design.

### 4.1 Transfer Initiation

| Detail | Value |
|---|---|
| **Endpoint** | `POST /api/v1/account/{accountId}/transactions` (for ACH/wire) |
| **Irreversible** | Depends on transfer type — wires are effectively irreversible once sent |
| **Blast radius** | CRITICAL — moves real money out of your account |

**Transfer types and reversibility:**
- **ACH transfers:** Can potentially be reversed within 1-2 business days if not yet settled. Contact Mercury support.
- **Domestic wire transfers:** Effectively irreversible once sent. The receiving bank must cooperate to return funds.
- **International wire transfers:** Irreversible. Funds may take 3-5 business days and involve intermediary banks.

**Required fields:**
- `amount` — in dollars (not cents, unlike Stripe)
- `recipientId` — must be a pre-created recipient
- `paymentMethod` — `ach` or `wire` or `domesticWire` or `internationalWire`
- `idempotencyKey` — ALWAYS include this to prevent duplicate transfers

**Approval workflow:** Mercury supports multi-person approval for transfers above configurable thresholds. API-initiated transfers may still require dashboard approval depending on your account settings. Verify this before assuming API transfers auto-execute.

**Safe practices:**
- Always use idempotency keys
- Verify recipient details via `GET /api/v1/recipients/{recipientId}` before sending
- Start with small test transfers to new recipients
- Set up transfer approval thresholds in Mercury dashboard that require manual approval above a dollar amount

---

### 4.2 Account Data Access

| Detail | Value |
|---|---|
| **Endpoint** | `GET /api/v1/accounts` and `GET /api/v1/account/{accountId}` |
| **Irreversible** | N/A (read-only) |
| **Blast radius** | DATA EXPOSURE — reveals full account numbers, routing numbers, balances |

**What is exposed:**
- Full account numbers and routing numbers
- Current balance and available balance
- Account type and status
- All transaction history via `GET /api/v1/account/{accountId}/transactions`

**Risk:** While read-only, this data is extremely sensitive. A leaked API token exposes your complete banking information. Treat Mercury API tokens with the same security as your bank login credentials.

**Safe practices:**
- Use read-only API tokens when you only need to pull data
- Rotate API tokens regularly
- Never log full account numbers — mask all but last 4 digits
- Restrict API token permissions to minimum required scope

---

### 4.3 Recipient Management

| Detail | Value |
|---|---|
| **Endpoint** | `POST /api/v1/recipients` (create) |
| **Also** | `DELETE /api/v1/recipients/{recipientId}` (delete) |
| **Blast radius** | Medium — deleting a recipient prevents future transfers to them |

**Create recipient risks:**
- A recipient with incorrect bank details will cause transfers to fail (ACH) or be sent to the wrong account (wire)
- Always verify recipient bank details through a separate channel before large transfers
- Wire transfers to incorrect accounts are extremely difficult to recover

**Delete recipient:**
- Removes the recipient from your Mercury account
- Pending transfers to this recipient may fail
- Historical transfers are preserved in transaction history

---

## 5. Wise (TransferWise) API (api.transferwise.com)

**Base URL:** `https://api.transferwise.com/v1` (some endpoints use `/v2` or `/v3`)
**Auth:** API token (`Authorization: Bearer <token>`). Personal tokens and OAuth tokens available.
**Sandbox:** `https://api.sandbox.transferwise.tech` — use this for testing
**Rate limit:** Not explicitly documented per-endpoint. Wise may throttle at ~100 requests/minute. Returns `429` when exceeded.
**Important:** Wise uses a multi-step transfer flow: Quote -> Recipient -> Transfer -> Fund. Each step must complete before the next.

### 5.1 Transfer Creation and Funding

| Detail | Value |
|---|---|
| **Create transfer** | `POST /v1/transfers` |
| **Fund transfer** | `POST /v3/profiles/{profileId}/transfers/{transferId}/payments` |
| **Irreversible** | Transfer creation is reversible (can cancel). Funding is harder to reverse. |
| **Blast radius** | CRITICAL — moves real money across currencies and borders |

**Transfer creation (`POST /v1/transfers`):**
- Requires a valid `quoteId` (from `POST /v3/quotes`) and `targetAccount` (recipient ID)
- Transfer is created in `incoming_payment_waiting` status — money has NOT moved yet
- The quote has an expiry time — if expired, the exchange rate may change
- `customerTransactionId` (UUID) acts as idempotency key — ALWAYS provide one

**Funding (`POST /v3/profiles/{profileId}/transfers/{transferId}/payments`):**
- THIS is the point of no return for balance-funded transfers
- `type: "BALANCE"` — deducts from your Wise multi-currency balance immediately
- Once funded from balance, cancellation may still be possible if Wise hasn't initiated the payout
- Bank transfer funding has a longer window since it depends on incoming funds

**Fee implications:**
- Wise charges a transfer fee (varies by corridor, typically 0.3%-1.5% of amount)
- The fee is deducted from the source amount or added on top, depending on configuration
- Mid-market exchange rate is used (no markup on rate — fee is explicit)
- Cancelling a funded transfer: the fee is refunded to your balance, but exchange rate differences are not compensated

---

### 5.2 Transfer Cancellation

| Detail | Value |
|---|---|
| **Endpoint** | `PUT /v1/transfers/{transferId}/cancel` |
| **Irreversible** | The cancellation itself is irreversible — you cannot un-cancel |
| **Blast radius** | Medium — stops money movement, but funds are returned |

**What happens:**
- Transfer status moves to `cancelled`
- If funded from balance, funds are returned to your Wise balance (not your bank account)
- If funded via bank transfer and funds haven't arrived, the transfer is simply cancelled
- If funds are already in transit to the recipient, cancellation may fail — Wise will return an error
- Exchange rate is not locked on cancellation — if you create a new transfer, you get the current rate

**Cancellation window:**
- Can cancel while status is `incoming_payment_waiting`, `incoming_payment_initiated`, or `processing`
- Cannot cancel once status is `outgoing_payment_sent` — money is already with the recipient's bank
- The window varies by corridor — some routes process in minutes, others in days

---

### 5.3 Recipient Deletion

| Detail | Value |
|---|---|
| **Endpoint** | `DELETE /v1/accounts/{recipientId}` |
| **Irreversible** | YES — the recipient record is permanently deleted |
| **Blast radius** | Medium |

**What happens:**
- Recipient (called "account" in Wise API) is permanently deleted
- Cannot send future transfers to this recipient without re-creating
- Historical transfers referencing this recipient retain the data
- If there are pending transfers to this recipient, they may fail

**Safe alternative:** Wise does not offer a "deactivate" for recipients. Before deleting, verify no pending transfers exist:
- `GET /v1/transfers?profile={profileId}&status=incoming_payment_waiting` and check for references to this recipient

---

### 5.4 Fund Conversion (Multi-Currency Balance)

| Detail | Value |
|---|---|
| **Endpoint** | `POST /v3/profiles/{profileId}/balance-movements` |
| **Also** | Convert via the quote/transfer flow between your own balances |
| **Irreversible** | YES — currency conversion at a specific rate cannot be undone |
| **Blast radius** | HIGH — you may lose money on unfavorable rates if done incorrectly |

**What happens:**
- Converts funds from one currency to another in your Wise multi-currency account
- Uses the mid-market rate at the time of conversion
- A small conversion fee applies (typically 0.3%-0.6%)
- You can convert back, but the round-trip will cost you two fees and any rate movement

**Safe practices:**
- Always check the current rate before converting: `GET /v3/rates?source=USD&target=EUR`
- Set up rate alerts instead of converting at arbitrary times
- For large conversions, consider using limit orders through the Wise dashboard (not available via API)
- Never auto-convert based on API triggers without human confirmation on the rate and amount

---

### 5.5 Other Dangerous Wise Endpoints

| Endpoint | Method | Blast Radius | Notes |
|---|---|---|---|
| `POST /v1/transfers/{id}/payments` | POST | CRITICAL | Funds a transfer — actual money movement |
| `POST /v3/quotes` | POST | Low | Creates a quote (no money moves), but quote locks a rate temporarily |
| `GET /v1/transfers/{id}` | GET | DATA EXPOSURE | Shows transfer amount, recipient details, status |
| `GET /v4/profiles/{id}/balances` | GET | DATA EXPOSURE | Shows all currency balances |
| `POST /v1/accounts` | POST | Medium | Creates a new recipient — verify bank details carefully |
| `PUT /v1/accounts/{id}` | PUT | Medium | Updates recipient bank details — if wrong, next transfer goes to wrong account |

---

## Authentication Summary

| Platform | Auth Method | Token Expiry | Test/Sandbox Available | Key Rotation |
|---|---|---|---|---|
| **Stripe** | Bearer / Basic (API key) | Never (until revoked) | Yes (`sk_test_` prefix) | Manual via dashboard |
| **QuickBooks** | OAuth 2.0 | 60 min access / 100 day refresh | Yes (sandbox company) | Auto-refresh required |
| **Xero** | OAuth 2.0 | 30 min access / 60 day refresh | Yes (demo company) | Auto-refresh required |
| **Mercury** | Bearer (API token) | Never (until revoked) | No public sandbox | Manual via dashboard |
| **Wise** | Bearer (API token) | Never (until revoked) | Yes (sandbox.transferwise.tech) | Manual via dashboard |

---

## Rate Limit Summary

| Platform | Rate Limit | Throttle Response | Recommended Max Usage |
|---|---|---|---|
| **Stripe** | 100 req/sec (live), 25 req/sec (test) | `429` | 50 req/sec |
| **QuickBooks** | 500 req/min per realm | `429` or `503` | 250 req/min |
| **Xero** | 60 req/min per tenant, 5,000/day | `429` | 30 req/min |
| **Mercury** | Undocumented | Undocumented | 30 req/min (conservative) |
| **Wise** | ~100 req/min (estimated) | `429` | 50 req/min |

---

## DANGER SUMMARY TABLE

Every destructive endpoint across all five platforms, ranked by blast radius.

### CRITICAL (Irreversible Financial Damage)

| Platform | Endpoint | What It Does | Why It's Critical |
|---|---|---|---|
| Stripe | `POST /v1/refunds` | Refunds a charge | Money leaves your account. Stripe keeps the processing fee. Cannot undo. |
| Stripe | `DELETE /v1/customers/{id}` | Deletes customer | Cascading destruction: cancels all subscriptions, deletes payment methods |
| Stripe | `POST /v1/disputes/{id}/close` | Accepts a dispute | You lose the charge amount AND $15 fee. Increases dispute rate. |
| Stripe | `POST /v1/payouts` | Creates a payout | Moves money to bank. Instant payouts cost 1% and are irreversible. |
| Stripe | `DELETE /v1/accounts/{id}` | Deletes connected account | Destroys entire Connect sub-account. All data and balance affected. |
| QuickBooks | `invoice?operation=delete` | Deletes invoice | Permanently removes from ledger. Breaks audit trail and tax filing consistency. |
| QuickBooks | `deposit?operation=delete` | Deletes bank deposit | All grouped payments become undeposited. Bank reconciliation destroyed. |
| QuickBooks | `journalentry?operation=delete` | Deletes journal entry | Removes double-entry records. Trial balance, P&L, balance sheet all shift. |
| Xero | `PUT /Payments/{id}` (DELETED) | Deletes payment | Bank reconciliation undone. Invoice returns to unpaid. No void alternative. |
| Xero | `POST /BatchPayments/{id}` (DELETED) | Deletes batch payment | All payments in the batch are removed simultaneously. |
| Xero | `POST /ManualJournals/{id}` (VOIDED) | Voids manual journal | Affects trial balance and all downstream reports. |
| Mercury | `POST /account/{id}/transactions` | Creates a transfer | Moves real money via ACH or wire. Wires are effectively irreversible. |
| Wise | `POST /v3/.../payments` | Funds a transfer | Deducts from balance immediately. May be irreversible if payout already sent. |
| Wise | `POST /v3/.../balance-movements` | Currency conversion | Converts at current rate. Round-trip costs two fees plus rate movement. |

### HIGH (Significant but Partially Recoverable)

| Platform | Endpoint | What It Does | Recovery Difficulty |
|---|---|---|---|
| Stripe | `DELETE /v1/subscriptions/{id}` | Immediate cancellation | Must create new subscription. Customer may churn. |
| Stripe | `POST /v1/invoices/{id}/void` | Voids finalized invoice | Irreversible. Must create credit note or new invoice. |
| Stripe | `DELETE /v1/webhook_endpoints/{id}` | Deletes webhook | Signing secret destroyed. Must reconfigure entire integration. |
| QuickBooks | `payment?operation=delete` | Deletes payment | Bank reconciliation broken. Must re-enter and re-reconcile. |
| QuickBooks | `billpayment?operation=delete` | Deletes bill payment | Vendor balance reverts. AP aging affected. |
| QuickBooks | `transfer?operation=delete` | Deletes bank transfer | Inter-account reconciliation broken. |
| Xero | `POST /Invoices/{id}` (VOIDED) | Voids invoice | Cannot un-void. Must remove payment allocations first. |
| Xero | `POST /BankTransactions/{id}` (DELETED) | Deletes bank transaction | Bank reconciliation affected. No void alternative. |
| Wise | `PUT /transfers/{id}/cancel` | Cancels transfer | Cancellation itself is irreversible. Rate is lost. |

### MEDIUM (Recoverable but Disruptive)

| Platform | Endpoint | What It Does | Recovery |
|---|---|---|---|
| Stripe | `DELETE /v1/products/{id}` | Deletes product | Prices become orphaned. Recreate product. |
| Stripe | `DELETE /v1/coupons/{id}` | Deletes coupon | Existing discounts remain. Cannot reapply same coupon. |
| Stripe | `POST /v1/payment_methods/{id}/detach` | Detaches payment method | Customer must re-enter card. May disrupt billing. |
| QuickBooks | `bill?operation=delete` | Deletes vendor bill | AP aging affected. 1099 reporting impacted. |
| QuickBooks | `creditmemo?operation=delete` | Deletes credit memo | Customer credits vanish. Must recreate. |
| Xero | `POST /Contacts/{id}` (ARCHIVED) | Archives contact | Reversible. Set back to ACTIVE. |
| Xero | `DELETE /Items/{id}` | Deletes inventory item | Historical line items show "deleted item." |
| Mercury | `DELETE /recipients/{id}` | Deletes recipient | Must recreate with full bank details. |
| Wise | `DELETE /accounts/{id}` | Deletes recipient | Must recreate. Verify pending transfers first. |
| Wise | `PUT /accounts/{id}` | Updates recipient | Wrong bank details = money sent to wrong account. |

---

## Pre-Flight Checklist (Before Any Destructive Financial API Call)

```
[ ] 1. Am I in test/sandbox mode? (If not, should I be?)
[ ] 2. Have I confirmed the exact entity ID being affected?
[ ] 3. Have I shown the user the dollar amount at risk?
[ ] 4. Have I checked for cascade effects? (linked invoices, payments, subscriptions)
[ ] 5. Is there a safer alternative? (void vs delete, cancel_at_period_end vs immediate)
[ ] 6. Am I using an idempotency key? (for money-moving operations)
[ ] 7. Have I verified this won't break bank reconciliation?
[ ] 8. Have I logged this in finance-decisions.md?
[ ] 9. Can this be reversed if it's wrong? (If not, get explicit written confirmation)
[ ] 10. Have I verified the API token has minimum required permissions?
```

---

## Key Differences from GTM:OS API Reference

| Concern | GTM:OS APIs | FINANCE:OS APIs |
|---|---|---|
| Primary risk | Burning email reputation, wasting credits | Losing real money, breaking audit trails |
| Recovery | Re-warm domain, buy more credits | May be legally impossible to recover funds |
| Compliance | CAN-SPAM, GDPR | SOX, GAAP, tax law, banking regulations |
| Testing | Safe to test with dummy leads | MUST use sandbox — test data in production breaks books |
| Batch operations | Common and expected | Should be avoided for destructive operations |
| Rate limits | Generous (thousands/min) | Tight (60-500/min) — respect them |
| Audit requirements | Nice to have | Legally required in most jurisdictions |
