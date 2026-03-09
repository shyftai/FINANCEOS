---
name: finance:swarm
description: Run financial operations with parallel agents
argument-hint: "<operation> [workspace-name]"
---
<objective>
Execute financial operations in parallel using multiple agents.

Operation and workspace: $ARGUMENTS
</objective>

<execution_context>
@./.claude/financeos/references/swarm.md
</execution_context>

<process>
1. Display: `<< FINANCE:OS // SWARM >>`
2. Parse operation from $ARGUMENTS
3. Load workspace context

## Available operations

### /finance:swarm reconcile
- Reconcile multiple accounts in parallel
- Each agent handles one account/bank feed
- Compile discrepancies into single report

### /finance:swarm invoices
- Process batch of invoices in parallel
- Each agent categorizes and records one batch
- Compile totals and flag anomalies

### /finance:swarm vendors
- Analyze multiple vendors in parallel
- Each agent reviews one vendor's spend, terms, and alternatives
- Compile vendor comparison report

### /finance:swarm categorize
- Bulk transaction categorization
- Each agent handles one batch of transactions
- Compile categorized results with confidence scores

### /finance:swarm forecast
- Run multiple forecast scenarios in parallel
- Each agent models one scenario (optimistic, base, pessimistic, custom)
- Compile comparison table

4. Split work, launch agents, collect results, present output
5. Log costs in COSTS.md
6. Suggest next action
</process>
