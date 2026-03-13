---
name: finance:status
description: Show workspace status — cash, revenue, expenses, health
argument-hint: "<workspace-name>"
---
<objective>
Quick snapshot of financial workspace state — cash position, revenue, expenses, and alerts.

Workspace: $ARGUMENTS
</objective>

<execution_context>
@./.claude/financeos/references/ui-brand.md
@./.claude/financeos/references/defaults.md
</execution_context>

<process>
1. Display: `<< FINANCE:OS // STATUS >>`
2. Load workspace from workspaces/$ARGUMENTS/

| Field | Value |
|-------|-------|
| Workspace | {name} |
| Cash on hand | ${n} |
| Monthly burn | ${n} |
| Runway | {n} months |
| MRR | ${n} |
| AR outstanding | ${n} |
| AP outstanding | ${n} |
| Tools connected | {list} |

3. Flag any warnings (low runway, overdue AR, budget overruns)
4. Show available commands
5. Display Next Up block
</process>
