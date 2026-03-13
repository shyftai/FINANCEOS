# FINANCE:OS Chart of Accounts

Standard chart of accounts for SaaS / tech startups. Load before categorizing transactions.

---

## Account Structure

| Range | Category | Type |
|-------|----------|------|
| 1000–1999 | Assets | Balance Sheet |
| 2000–2999 | Liabilities | Balance Sheet |
| 3000–3999 | Equity | Balance Sheet |
| 4000–4999 | Revenue | Income Statement |
| 5000–5999 | Cost of Goods Sold (COGS) | Income Statement |
| 6000–6999 | Operating Expenses | Income Statement |
| 7000–7999 | Other Income/Expense | Income Statement |

---

## Standard Accounts

### Assets (1000–1999)

| Account | Name | Description |
|---------|------|-------------|
| 1000 | Cash — Operating | Primary operating checking account |
| 1010 | Cash — Savings | Reserve / savings account |
| 1020 | Cash — Payroll | Dedicated payroll account |
| 1100 | Accounts Receivable | Outstanding customer invoices |
| 1200 | Prepaid Expenses | Prepaid software, insurance, rent |
| 1300 | Employee Advances | Outstanding advances to employees |
| 1500 | Fixed Assets | Equipment, furniture |
| 1510 | Accumulated Depreciation | Contra-asset for depreciation |

### Liabilities (2000–2999)

| Account | Name | Description |
|---------|------|-------------|
| 2000 | Accounts Payable | Outstanding vendor bills |
| 2100 | Credit Card Payable | Outstanding credit card balances |
| 2200 | Accrued Expenses | Expenses incurred not yet paid |
| 2300 | Deferred Revenue | Prepaid subscriptions not yet earned |
| 2400 | Payroll Liabilities | Payroll taxes, benefits payable |
| 2500 | Sales Tax Payable | Collected sales tax not yet remitted |
| 2600 | Notes Payable — Short Term | Lines of credit, short-term debt |
| 2700 | Notes Payable — Long Term | Venture debt, term loans |

### Equity (3000–3999)

| Account | Name | Description |
|---------|------|-------------|
| 3000 | Common Stock | Par value of issued shares |
| 3100 | Additional Paid-In Capital | Investment above par value |
| 3200 | Retained Earnings | Accumulated profits/losses |
| 3300 | Owner's Draw / Distributions | Distributions to shareholders |

### Revenue (4000–4999)

| Account | Name | Description |
|---------|------|-------------|
| 4000 | Subscription Revenue — MRR | Monthly recurring subscription revenue |
| 4010 | Subscription Revenue — Annual | Annual prepaid subscriptions |
| 4100 | Services Revenue | Implementation, consulting, training |
| 4200 | One-Time Revenue | Setup fees, one-time charges |
| 4300 | Usage / Overage Revenue | Metered or usage-based billing |
| 4900 | Discounts & Refunds | Contra-revenue (credits, refunds) |

### Cost of Goods Sold (5000–5999)

| Account | Name | Description |
|---------|------|-------------|
| 5000 | Hosting & Infrastructure | AWS, GCP, Azure, Vercel |
| 5100 | Third-Party Software (COGS) | APIs, services embedded in product |
| 5200 | Customer Support — Direct | Support staff directly serving customers |
| 5300 | Payment Processing Fees | Stripe fees, payment gateway costs |

### Operating Expenses (6000–6999)

| Account | Name | Description |
|---------|------|-------------|
| 6000 | Payroll — Salaries | Employee salaries |
| 6010 | Payroll — Benefits | Health, dental, 401k, etc. |
| 6020 | Payroll — Taxes | Employer payroll taxes |
| 6100 | Contractors | Independent contractor payments |
| 6200 | Software & Subscriptions | Internal tools (not COGS) |
| 6300 | Marketing & Advertising | Ad spend, content, events |
| 6310 | Marketing — Tools | Marketing SaaS tools |
| 6400 | Sales — Commissions | Sales team commissions |
| 6410 | Sales — Tools | CRM, sales tools |
| 6500 | Office & Admin | Rent, supplies, utilities |
| 6600 | Professional Services | Legal, accounting, consulting |
| 6700 | Travel & Entertainment | Business travel, meals |
| 6800 | Insurance | Business insurance premiums |
| 6900 | Depreciation & Amortization | Asset depreciation |

### Other Income/Expense (7000–7999)

| Account | Name | Description |
|---------|------|-------------|
| 7000 | Interest Income | Bank interest earned |
| 7100 | Interest Expense | Loan interest paid |
| 7200 | Foreign Exchange Gain/Loss | Currency conversion differences |
| 7300 | Other Income | Miscellaneous non-operating income |

---

## Categorization Rules

| Transaction Type | Account | Notes |
|-----------------|---------|-------|
| AWS / GCP bill | 5000 | COGS — hosting |
| Stripe fees | 5300 | COGS — payment processing |
| Slack / Notion | 6200 | OpEx — internal software |
| Google Ads spend | 6300 | OpEx — marketing |
| Apollo / Clay | 6310 | OpEx — marketing tools |
| Gusto payroll | 6000/6010/6020 | Split by component |
| 1099 contractor | 6100 | OpEx — contractors |
| Lawyer retainer | 6600 | OpEx — professional services |

---

## Notes

- This is a starting template — customize per workspace
- Map to your accounting platform (QBO, Xero) during onboarding
- COGS vs OpEx distinction matters for gross margin calculation
- Deferred revenue (2300) is critical for SaaS — recognize as earned
