---
name: finance:scenario
description: What-if scenario modeling
argument-hint: "<company-name> <scenario-description>"
---
<objective>
Model what-if scenarios. What happens if we hire 3 people? Revenue drops 20%?

Company and scenario: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // SCENARIO >>`
2. Load: FORECAST.md, CASHFLOW.md, BUDGET.md, METRICS.md
3. Define scenario variables
4. Model impact on: cash flow, runway, burn rate, P&L
5. Compare to base case side by side
6. Recommend action
</process>
