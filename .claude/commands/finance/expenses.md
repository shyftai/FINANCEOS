---
name: finance:expenses
description: Expense tracking and categorization
argument-hint: "<company-name> [--period month|quarter]"
---
<objective>
Track and categorize expenses. Find patterns and savings.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // EXPENSES >>`
2. Load: EXPENSES.md, BUDGET.md, ACCOUNTS.md, LEARNINGS.md
3. Pull from QuickBooks/Xero/Ramp if connected
4. Categorize by chart of accounts
5. Compare to budget, flag variances
6. Identify trends and optimization opportunities
7. Update EXPENSES.md
</process>
