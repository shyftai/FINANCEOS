---
name: finance:dashboard
description: Full financial dashboard
argument-hint: "<company-name>"
---
<objective>
Complete financial dashboard. Cash, revenue, expenses, metrics.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // DASHBOARD >>`
2. Load all workspace files, pull from connected tools
3. Display: Cash position, Revenue (MRR/ARR, growth), Expenses (burn, top categories), Budget vs actual, Runway, Key metrics, To-dos
4. Flag anomalies and trends
5. Suggest actions
</process>
