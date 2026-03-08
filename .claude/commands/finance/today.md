---
name: finance:today
description: Daily financial briefing
argument-hint: "<company-name>"
---
<objective>
Daily financial briefing. Cash position, what needs attention, upcoming deadlines.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // TODAY >>`
2. Load: CASHFLOW.md, BUDGET.md, METRICS.md, TAX-CALENDAR.md, ROADMAP.md, LEARNINGS.md
3. 🔴 DO NOW: Negative cash flow, overdue invoices, tax deadlines today, reconciliation gaps
4. 🟡 TODAY: Invoices to send, bills to pay, budget variance alerts, vendor renewals
5. 🟢 THIS WEEK: Upcoming deadlines, recurring payments, forecast updates
6. Cash snapshot, runway, key metrics vs last week
7. Role-aware: Founder→runway+cash, CFO→everything, Controller→reconciliation
</process>
