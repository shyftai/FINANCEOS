---
name: finance:runway
description: Runway calculation and scenarios
argument-hint: "<company-name>"
---
<objective>
How long does the money last? Model different scenarios.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // RUNWAY >>`
2. Load: CASHFLOW.md, EXPENSES.md, REVENUE.md, FORECAST.md
3. Calculate: current cash / monthly burn = runway months
4. Three scenarios: current burn, optimized (-20%), growth (+20%)
5. Runway timeline with milestones
6. Optimization suggestions
7. Update METRICS.md
</process>
