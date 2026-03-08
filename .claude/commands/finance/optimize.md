---
name: finance:optimize
description: Cost optimization opportunities
argument-hint: "<company-name>"
---
<objective>
Find cost savings. Review every line item for optimization.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // OPTIMIZE >>`
2. Load: EXPENSES.md, VENDORS.md, BUDGET.md, LEARNINGS.md
3. Analyze: duplicates, underused subscriptions, renegotiation opportunities, cheaper alternatives
4. Rank by potential savings
5. Display recommendations with estimated impact
6. Add approved changes to ROADMAP.md
</process>
