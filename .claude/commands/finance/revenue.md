---
name: finance:revenue
description: Revenue tracking and analysis
argument-hint: "<company-name> [--period month|quarter|year]"
---
<objective>
Revenue analysis. MRR/ARR tracking, growth, trends.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // REVENUE >>`
2. Load: REVENUE.md, METRICS.md, LEARNINGS.md
3. Pull from Stripe/QuickBooks if connected
4. MRR waterfall: starting + new + expansion - churn - contraction = ending
5. Revenue by stream, growth trends, ARPU, LTV
6. Update REVENUE.md and METRICS.md
</process>
