---
name: finance:tax-prep
description: Tax preparation checklist
argument-hint: "<company-name> [--filing type]"
---
<objective>
Prepare for tax filing. Checklist, documents, CPA coordination.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // TAX PREP >>`
2. Load: TAX-CALENDAR.md, ACCOUNTS.md, EXPENSES.md, REVENUE.md
3. Identify upcoming filing
4. Generate checklist of required documents
5. Flag missing items
6. Note: "Organizational support, not tax advice. Consult your CPA."
</process>
