---
name: finance:today
description: Daily financial briefing — what needs attention today
argument-hint: "<workspace-name>"
---
<objective>
Daily financial operations briefing. Cash position, deadlines, and action items.

Workspace: $ARGUMENTS
</objective>

<execution_context>
@./.claude/financeos/references/defaults.md
</execution_context>

<process>
1. Display: `<< FINANCE:OS // TODAY >>`
2. Load workspace context — CASHFLOW.md, BUDGET.md, EXPENSES.md, REVENUE.md, TAX-CALENDAR.md, METRICS.md

## Cash position
3. Show current cash position:
   - Bank balance(s)
   - Accounts receivable (outstanding invoices, overdue flagged)
   - Accounts payable (bills due this week)
   - Net cash position

## Runway
4. Show runway status:
   - Current burn rate (monthly)
   - Runway remaining (months)
   - Flag if below safety threshold (6 months)
   - Critical alert if below 3 months

## Deadlines today/this week
5. Show financial deadlines:
   - Tax filing deadlines
   - Payroll dates
   - Invoice due dates (receivable)
   - Bill payment dates (payable)
   - Reconciliation deadlines
   - Report submission dates

## Budget pulse
6. Show budget vs actuals for current period:
   - Revenue: actual vs forecast
   - Expenses: actual vs budget (flag if >10% variance)
   - Top 3 over-budget categories

## Alerts
7. Flag urgent items:
   - Overdue invoices (>30 days)
   - Overdue bills
   - Budget overages
   - Unusual transactions
   - Low balance warnings

## Actions
8. Suggest top 3 actions for today
9. Display quick action shortcuts: /finance:cashflow, /finance:invoices, /finance:reconcile, /finance:expenses
</process>
