---
name: finance:reconcile
description: Bank reconciliation
argument-hint: "<company-name> [--account account-name]"
---
<objective>
Reconcile bank statements with book records.

Company: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // RECONCILE >>`
2. Load: ACCOUNTS.md, transactions/
3. Compare bank balance to book balance
4. Identify discrepancies, flag unmatched transactions
5. Update reconciliation status
</process>
