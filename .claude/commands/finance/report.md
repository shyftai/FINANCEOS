---
name: finance:report
description: Generate financial reports
argument-hint: "<company-name> [--type monthly|quarterly|annual|board]"
---
<objective>
Generate comprehensive financial report.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // REPORT >>`
2. Load all workspace files
3. Generate: executive summary, P&L, balance sheet, cash flow, KPIs, variance, forecast, recommendations
4. Save to reports/
5. Offer: share via Slack, export
</process>
