---
name: finance:deadlines
description: Upcoming tax and filing deadlines
argument-hint: "<company-name>"
---
<objective>
Show all upcoming financial deadlines. Never miss a filing.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // DEADLINES >>`
2. Load: TAX-CALENDAR.md
3. Next 90 days, sorted by date
4. 🔴 overdue, 🟡 this week, 🟢 upcoming
5. Preparation status for each
</process>
