---
name: finance:vendors
description: Vendor management and optimization
argument-hint: "<company-name> [--action review|add|compare]"
---
<objective>
Manage vendors, review contracts, find savings.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // VENDORS >>`
2. Load: VENDORS.md, EXPENSES.md, LEARNINGS.md
3. Modes: review (audit), add (onboard), compare (alternatives)
4. Flag: expiring contracts, price increase opportunities, consolidation
5. Update VENDORS.md
</process>
