---
name: finance:onboard
description: Create a new financial workspace with guided setup
argument-hint: "<company-name>"
---
<objective>
Set up a new financial workspace. From chart of accounts to budget to first cash flow forecast.

Company name: $ARGUMENTS
</objective>

<process>
1. Display mode header: `<< FINANCE:OS // ONBOARD >>`
2. Role selection: Founder / CFO / Controller / Bookkeeper / Fractional CFO
3. Collaboration: Solo / Team (if team, ask who else touches finances — co-founder, accountant, bookkeeper)
4. Execution mode: Ask: "How should I handle approvals?"
   - **Interactive** (default) — confirms each major decision before proceeding
   - **Auto** — auto-approves and keeps moving. Only stops for tax filings, external payments, reconciliation sign-offs, and compliance.

   Role-based defaults:
   - **Controller** → default interactive (reconciliation and compliance need precision)
   - **CFO** → default interactive (strategic decisions need review)
   - **Bookkeeper** → suggest auto (routine categorization and data entry benefits from speed)
   - **Founder** → suggest auto (they want to move fast)
   - **Fractional CFO** → default interactive (client work needs checkpoints)

   Save to workspace.config.md as `**Execution mode:** auto` or `**Execution mode:** interactive`

## Registration (optional)
5. Ask: "Would you like to register this workspace for updates, tips, and priority support? (just your email and company name)"
   - If yes: collect email and company name, then POST to the registration endpoint:
     ```
     POST https://shyft.ai/api/hooks/register
     {
       "os": "financeos",
       "version": "1.1.0",
       "company": "{company_name}",
       "email": "{email}",
       "workspace": "{workspace_name}",
       "timestamp": "{ISO 8601}"
     }
     ```
     Show: `Registered — you'll get updates at {email}`
   - If no: skip gracefully. Show: `Skipped — you can register anytime with /finance:feedback`
   - This step never blocks onboarding. If the POST fails, ignore silently and continue.

6. Company basics: entity name, entity type (LLC, C-Corp, S-Corp, sole prop, partnership), state/country of incorporation, fiscal year end, primary currency

7. Multi-entity setup:
   - Does the company have multiple entities? (holding company, subsidiaries, international entities)
   - If yes: list each entity with type, jurisdiction, and intercompany relationship
   - Flag: multi-entity requires consolidated reporting and intercompany elimination rules

8. Accounting method selection:
   - **Cash basis** — simpler, records revenue/expenses when cash moves. Best for: early-stage startups, small businesses, companies under $25M revenue (IRS threshold). Pros: easy to understand, matches bank statements. Cons: poor revenue timing accuracy, harder to get investor-grade financials.
   - **Accrual basis** — records revenue when earned, expenses when incurred. Best for: companies with investors, SaaS/subscription businesses, companies planning to raise. Pros: GAAP-compliant, accurate P&L, investor-ready. Cons: more complex, requires accounts receivable/payable tracking.
   - **Modified cash** — hybrid approach. Ask: do you need investor-grade financials? If yes, recommend accrual.
   - Note: switching from cash to accrual later is painful — if in doubt, start with accrual.

9. Tax jurisdiction details:
   - Federal tax obligations (entity type determines filing requirements)
   - State tax registrations: which states do you have nexus in? (employees, offices, significant sales)
   - Sales tax: do you collect sales tax? Which states? (SaaS companies: check economic nexus thresholds)
   - International: any foreign tax obligations? VAT/GST registration?
   - Key deadlines: list filing deadlines based on entity type and fiscal year
   - Tax preparer: do you have a CPA or tax firm? Name and contact.

10. Chart of accounts configuration:
   - Start with standard template based on entity type and industry
   - Revenue accounts: list revenue streams (product sales, subscriptions, services, etc.)
   - COGS accounts: direct costs tied to revenue (hosting, payment processing, etc.)
   - Operating expense categories: payroll, rent, software, marketing, legal, etc.
   - Ask: any custom categories needed for your business? (R&D for tax credits, department-level tracking, project-based costing)
   - Class/department tracking: do you need to track P&L by department, product line, or location?

11. Bank and payment connections:
    - Primary business bank account(s): bank name, account type (checking, savings, credit)
    - Credit cards: business credit cards in use
    - Payment processors: Stripe, PayPal, Square, etc.
    - Payroll provider: Gusto, Rippling, ADP, etc.
    - Note: list connections needed — actual API setup happens in `/finance:connect`

12. Revenue streams and pricing:
    - Current revenue model (subscription, one-time, usage-based, hybrid)
    - Current MRR/ARR if applicable
    - Pricing tiers or rate card
    - Revenue recognition method (important for accrual basis)

13. Major expenses and recurring costs:
    - Top 5 recurring expenses (payroll, rent, software, etc.)
    - Current monthly burn rate (or best estimate)
    - Major upcoming expenses (hiring, equipment, office)
    - Vendor list: key vendors with monthly/annual spend

14. Initial budget framework:
    - Does a budget exist? If yes, import it. If no, build from current actuals.
    - Budget period: monthly, quarterly, annual
    - Budget approach: top-down (revenue target, allocate expenses) or bottom-up (sum known costs)

15. Cash flow baseline:
    - Current cash position
    - Monthly cash burn
    - Runway calculation (cash / monthly burn)
    - Expected inflows: revenue, funding, grants

16. Create workspace from _template/, fill in all gathered information
17. Display summary:

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  FINANCE WORKSPACE: {Company Name}                          ┃
┃  Entity: {type}    Basis: {cash/accrual}    FY: {end}       ┃
┃                                                             ┃
┃  Accounts: {n} accounts configured                          ┃
┃  Banks: {n} connections to set up                           ┃
┃  Revenue: ${MRR}/mo    Burn: ${burn}/mo                     ┃
┃  Runway: {months} months                                    ┃
┃  Tax jurisdictions: {list}                                  ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

18. Suggest next steps:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  >> Next: /finance:connect to link bank accounts
     Also: /finance:budget to build your first budget
     Also: /finance:forecast for cash flow projection
     Also: /finance:health for financial health check
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Group questions logically — do not ask all at once. Ask 2-3 questions per block, build on previous answers.
</process>
