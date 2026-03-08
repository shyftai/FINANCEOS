---
name: finance:variance
description: Budget vs actual variance analysis
argument-hint: "<company-name> [--period month|quarter]"
---
<objective>
Compare budget to actuals. Explain the variances.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // VARIANCE >>`
2. Load: BUDGET.md, EXPENSES.md, REVENUE.md, LEARNINGS.md
3. Side by side: budget vs actual per line item
4. Variance ($ and %), flag >10%
5. Analyze causes, recommend adjustments
6. Update LEARNINGS.md
</process>
