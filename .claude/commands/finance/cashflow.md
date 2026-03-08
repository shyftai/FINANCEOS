---
name: finance:cashflow
description: Cash flow analysis and projections
argument-hint: "<company-name> [--period 13w|6m|12m]"
---
<objective>
Analyze cash flow and project forward.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // CASH FLOW >>`
2. Load: CASHFLOW.md, REVENUE.md, EXPENSES.md, ACCOUNTS.md
3. Current position: all account balances
4. Inflows: recurring revenue, expected payments, one-time income
5. Outflows: recurring expenses, upcoming bills, payroll
6. 13-week projection with weekly granularity
7. Flag weeks where cash goes below safety threshold
8. Update CASHFLOW.md
</process>
