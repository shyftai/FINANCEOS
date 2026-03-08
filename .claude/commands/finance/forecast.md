---
name: finance:forecast
description: Revenue and expense forecasting
argument-hint: "<company-name> [--horizon 3m|6m|12m]"
---
<objective>
Financial forecasting with scenario modeling.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // FORECAST >>`
2. Load: FORECAST.md, REVENUE.md, EXPENSES.md, METRICS.md, LEARNINGS.md
3. Revenue forecast: base/best/worst by month
4. Expense forecast: fixed + variable projections
5. Net income by scenario
6. State all assumptions explicitly
7. Update FORECAST.md
</process>
