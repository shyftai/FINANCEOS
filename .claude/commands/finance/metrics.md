---
name: finance:metrics
description: Key financial metrics dashboard
argument-hint: "<company-name>"
---
<objective>
All key financial metrics in one view.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // METRICS >>`
2. Load: METRICS.md, REVENUE.md, EXPENSES.md, CASHFLOW.md
3. Display: burn rate, runway, MRR/ARR, growth, gross margin, net margin, CAC, LTV, LTV:CAC, quick ratio, rule of 40
4. Trend arrows and vs targets
5. Update METRICS.md
</process>
