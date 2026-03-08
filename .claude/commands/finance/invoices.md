---
name: finance:invoices
description: Invoice management
argument-hint: "<company-name> [--action create|track|overdue]"
---
<objective>
Create, track, and follow up on invoices.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // INVOICES >>`
2. Load: REVENUE.md, ACCOUNTS.md
3. Modes: create (generate invoice), track (status), overdue (follow up)
4. Display: outstanding, overdue, paid this month, total AR
5. Generate follow-up emails for overdue
</process>
