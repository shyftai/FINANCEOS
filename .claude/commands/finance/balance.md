---
name: finance:balance
description: Balance sheet
argument-hint: "<company-name> [--date as-of-date]"
---
<objective>
Generate balance sheet.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // BALANCE SHEET >>`
2. Load: ACCOUNTS.md, CASHFLOW.md
3. Assets (current + long-term), liabilities (current + long-term), equity
4. Verify: assets = liabilities + equity
5. Save to reports/
</process>
