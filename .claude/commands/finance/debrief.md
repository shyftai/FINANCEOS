---
name: finance:debrief
description: End-of-period financial review and retrospective
argument-hint: "<workspace-name> [period]"
---
<objective>
Retrospective on a completed fiscal period — what went well, what surprised, what to improve.
</objective>
<process>
1. Display: `<< FINANCE:OS // DEBRIEF >>`
2. Load workspace context — METRICS.md, BUDGET.md, FORECAST.md, CASHFLOW.md
3. Compare actuals vs forecast for the period
4. Identify top 3 wins (under budget, revenue beats, cost savings)
5. Identify top 3 misses (overages, missed forecasts, unexpected expenses)
6. Extract lessons — update LEARNINGS.md
7. Generate recommendations for next period
8. Display debrief summary
9. Suggest: /finance:forecast, /finance:budget
</process>
