---
name: finance:burn
description: Burn rate analysis
argument-hint: "<company-name>"
---
<objective>
Analyze burn rate. Gross burn, net burn, trends.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // BURN RATE >>`
2. Load: EXPENSES.md, REVENUE.md, CASHFLOW.md, METRICS.md
3. Gross burn (total expenses), net burn (expenses - revenue)
4. 3-month rolling average, direction
5. Breakdown by category
6. Runway impact
7. Update METRICS.md
</process>
