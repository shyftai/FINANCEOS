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
3. Collaboration: Solo / Team
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
5. Company basics: entity type, jurisdiction, fiscal year, accounting basis, currency
6. Chart of accounts setup (defaults + customize)
7. Bank connections
8. Revenue streams, pricing, MRR/ARR
9. Major expenses, recurring costs, vendors
10. Initial budget framework
11. Tax obligations and deadlines
12. Create workspace from _template/, fill answers, display summary
</process>
