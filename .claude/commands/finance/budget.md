---
name: finance:budget
description: Create or update budget
argument-hint: "<company-name> [--period annual|quarterly|monthly]"
---
<objective>
Create or update the budget. Set targets by category.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // BUDGET >>`
2. Load: BUDGET.md, REVENUE.md, EXPENSES.md, FORECAST.md, LEARNINGS.md
3. New: guided creation by category with assumptions
4. Update: show current, incorporate actuals, reforecast
5. Display summary with assumptions
6. Update BUDGET.md
</process>
