---
name: finance:pnl
description: Profit & loss statement
argument-hint: "<company-name> [--period month|quarter|year]"
---
<objective>
Generate P&L statement.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // P&L >>`
2. Load: REVENUE.md, EXPENSES.md, ACCOUNTS.md
3. Standard P&L: revenue, COGS, gross profit, OpEx, operating income, net income
4. Compare to prior period and budget
5. Save to reports/
</process>
