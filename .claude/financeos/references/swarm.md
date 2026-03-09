# Swarm Reference — Parallel Financial Operations

FINANCE:OS can spawn parallel agents for batch financial work using Claude's sub-agent capabilities.

## When to swarm

- **Batch invoice processing** — split large invoice batches across agents (25-50 per agent)
- **Multi-account reconciliation** — one agent per bank account, reconcile in parallel
- **Parallel vendor analysis** — analyze multiple vendor contracts simultaneously
- **Bulk transaction categorization** — split uncategorized transactions across agents

## How it works

1. Command detects batch size exceeds threshold (default: 25 items)
2. Split work into equal chunks
3. Spawn one agent per chunk with shared workspace context
4. Each agent processes independently, writes results to a temp section
5. Orchestrator merges results, flags conflicts, presents summary

## Agent context

Each spawned agent receives:
- Workspace ACCOUNTS.md and relevant source-of-truth files
- Its assigned chunk of work
- Output format instructions

## Limits

- Max 6 concurrent agents
- Each agent gets the same quality gates — no shortcuts
- Hard gates (payments, tax filings, reconciliation sign-offs) are never delegated to swarm agents
- Results always merge back for human review before finalization

## Display

Use the swarm display pattern from ui-brand.md for progress and results.
