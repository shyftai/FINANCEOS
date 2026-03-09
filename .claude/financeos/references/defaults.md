# FINANCE:OS — Defaults

## Workspace defaults

| Setting | Default | Override in |
|---------|---------|------------|
| Execution mode | interactive | workspace.config.md |
| Collaboration mode | solo | workspace.config.md |

## Reporting defaults

| Setting | Default | Override in |
|---------|---------|------------|
| Fiscal year | Jan-Dec | workspace.config.md |
| Accounting basis | Cash | workspace.config.md |
| Currency | USD | workspace.config.md |
| Rounding | Nearest dollar (reports), cents (calculations) | Never override |
| Negative format | Parentheses: ($1,234) | Never override |
| Period comparison | vs prior month and vs budget | report |

## Forecast defaults

| Setting | Default |
|---------|---------|
| Cash flow forecast horizon | 13 weeks |
| Revenue forecast horizon | 12 months |
| Scenario count | 3 (base, best, worst) |
| Confidence range | ±20% from base |

## Budget defaults

| Setting | Default |
|---------|---------|
| Budget period | Annual with quarterly breakdown |
| Variance alert threshold | >10% |
| Reforecast frequency | Quarterly |

## Runway defaults

| Setting | Default |
|---------|---------|
| Burn calculation | 3-month rolling average |
| Safety threshold | 6 months runway |
| Critical threshold | 3 months runway |

## Quality defaults

| Gate | Default |
|------|---------|
| Data source required | Yes — never override |
| Reconciliation before reporting | Yes — never override |
| Assumptions stated | Yes — never override |
| CPA review flag for tax | Yes — never override |
