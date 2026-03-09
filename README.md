# FINANCE:OS — The Financial Operations System

> Turn Claude Code into a full financial operations engine. Track it. Plan it. Report it. Optimize it.

FINANCE:OS is a Claude Code plugin that runs your entire finance workflow — from cash flow tracking to budget management, revenue forecasting, expense optimization, tax preparation, and financial reporting. It connects to your accounting stack via API, enforces accuracy at every step, and never ships a report with unverified numbers.

**Built for:** founders, CFOs, controllers, bookkeepers, and fractional CFOs running financial operations.

---

## Install

### Prerequisites
You need [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed:
```
npm install -g @anthropic-ai/claude-code
```

### Get started
```
git clone https://github.com/shyftai/FINANCEOS.git
cd FINANCEOS
claude
```

That's it. FINANCE:OS boots automatically — shows the banner, scans your connected tools, and asks which workspace to load. No config files to edit, no build step.

### Connect your tools
FINANCE:OS works with your existing tools. Two ways to connect:

**MCP servers (recommended)** — Claude can use these tools directly with full capabilities:
- [QuickBooks](https://quickbooks.intuit.com) — direct accounting data access
- [Stripe](https://stripe.com) — payment and revenue data
- [Xero](https://xero.com) — accounting data (alternative to QuickBooks)
- [Slack](https://slack.com) — team notifications and alerts

Add MCP servers to your Claude Code settings — FINANCE:OS detects them automatically on boot.

**API keys** — add to `.env` for tools that connect via API:
```
cp .env.example .env
# add your keys (QuickBooks, Stripe, Mercury, Gusto, etc.)
```

You don't need all tools to start. FINANCE:OS works with whatever you have and tells you what's missing.

### First run
When FINANCE:OS boots, run:
```
/finance:onboard my-company
```
It asks your role (Founder, CFO, Controller, Bookkeeper, or Fractional CFO), walks you through setup, and creates your workspace. Then you're ready to track finances.

---

## What it looks like

When you open FINANCE:OS in Claude Code, this is what you see.

### Boot sequence
```
███████╗██╗███╗   ██╗ █████╗ ███╗   ██╗ ██████╗███████╗
██╔════╝██║████╗  ██║██╔══██╗████╗  ██║██╔════╝██╔════╝
█████╗  ██║██╔██╗ ██║███████║██╔██╗ ██║██║     █████╗
██╔══╝  ██║██║╚██╗██║██╔══██║██║╚██╗██║██║     ██╔══╝
██║     ██║██║ ╚████║██║  ██║██║ ╚████║╚██████╗███████╗
╚═╝     ╚═╝╚═╝  ╚═══╝╚═╝  ╚═╝╚═╝  ╚═══╝ ╚═════╝╚══════╝
       ██╗  ██████╗ ███████╗
       ╚═╝ ██╔═══██╗██╔════╝
           ██║   ██║███████╗
       ██╗ ██║   ██║╚════██║
       ╚═╝ ╚██████╔╝███████║
            ╚═════╝ ╚══════╝
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  F I N A N C E : O S                           v1.1.0
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Track it. Plan it. Report it. Optimize it.
                                          by Shyft AI

  ┌─ SYSTEM ──────────────────────────────────────────┐
  │                                                    │
  │  Workspaces:  acme-corp, startup-x                 │
  │  Mode:        solo                                 │
  │  Execution:   interactive                          │
  │                                                    │
  │  MCP servers:                                      │
  │  [x] QuickBooks          [x] Stripe                │
  │  [ ] Xero                [ ] Slack                  │
  │                                                    │
  │  API keys:                                         │
  │  [x] Mercury         [ ] Brex                      │
  │  [x] Gusto           [ ] Ramp                      │
  │                                                    │
  │  2 MCP servers · 2 API keys · 4 missing            │
  │                                                    │
  └────────────────────────────────────────────────────┘

  ┌──────────────────────────────────────────────────┐
  │  ACCOUNTS ─── BUDGET ─── FORECAST ─── TAX       │
  │                   │                              │
  │               CASH FLOW                          │
  │                   │                              │
  │      ┌────────────┼────────────┐                 │
  │      ▼            ▼            ▼                 │
  │   REVENUE     EXPENSES     PAYROLL               │
  │      │            │            │                 │
  │      ▼            ▼            ▼                 │
  │  INVOICES ─── CATEGORIZE ─── RECONCILE           │
  │                   │                              │
  │              REPORTING                           │
  │                   │                              │
  │         ANALYSIS + OPTIMIZE                      │
  │                   │                              │
  │            COMPLIANCE ──── AUDIT                  │
  └──────────────────────────────────────────────────┘

  ┌─ COMMANDS ──────────────────────────────────────────┐
  │                                                      │
  │  Start      /finance:today · /finance:dashboard      │
  │  Setup      /finance:onboard · /finance:connect      │
  │  Cash       /finance:cashflow · /finance:runway       │
  │             /finance:forecast · /finance:scenario     │
  │  Revenue    /finance:revenue · /finance:invoices      │
  │  Expenses   /finance:expenses · /finance:vendors      │
  │             /finance:burn · /finance:optimize         │
  │  Budget     /finance:budget · /finance:variance       │
  │  Tax        /finance:tax-prep · /finance:deadlines    │
  │  Report     /finance:report · /finance:pnl            │
  │             /finance:balance · /finance:metrics       │
  │  Audit      /finance:reconcile · /finance:audit       │
  │  Scale      /finance:swarm                             │
  │  Agency     /finance:portfolio                       │
  │  Review     /finance:debrief · /finance:compliance    │
  │  System     /finance:feedback                        │
  │  More       /finance:status for all commands          │
  │                                                      │
  └──────────────────────────────────────────────────────┘

  >> Which workspace are we loading?
```

---

## How it works

```
  ┌──────────────────────────────────────────────────┐
  │  ACCOUNTS ─── BUDGET ─── FORECAST ─── TAX       │
  │                   │                              │
  │               CASH FLOW                          │
  │                   │                              │
  │      ┌────────────┼────────────┐                 │
  │      ▼            ▼            ▼                 │
  │   REVENUE     EXPENSES     PAYROLL               │
  │      │            │            │                 │
  │      ▼            ▼            ▼                 │
  │  INVOICES ─── CATEGORIZE ─── RECONCILE           │
  │                   │                              │
  │              REPORTING                           │
  │                   │                              │
  │         ANALYSIS + OPTIMIZE                      │
  │                   │                              │
  │            COMPLIANCE ──── AUDIT                  │
  └──────────────────────────────────────────────────┘
```

Every task is checked against these sources of truth before anything ships:

| File | What it controls |
|------|-----------------|
| **ACCOUNTS.md** | Chart of accounts, bank connections |
| **BUDGET.md** | Annual/quarterly budget with line items |
| **CASHFLOW.md** | Cash flow tracking and projections |
| **FORECAST.md** | Revenue and expense forecasts |
| **REVENUE.md** | Revenue streams, MRR/ARR tracking |
| **EXPENSES.md** | Expense categories, recurring costs |
| **VENDORS.md** | Vendor list, contracts, payment terms |
| **TAX-CALENDAR.md** | Tax deadlines and filings |
| **METRICS.md** | KPIs, benchmarks, targets |
| **LEARNINGS.md** | Persistent financial intelligence |

If the numbers aren't verified, the report doesn't ship.

---

## The finance lifecycle

**1. Onboard** -> `/finance:onboard` walks through your company, role, tools, accounts, and fiscal calendar. `/finance:connect` links bank accounts and payment tools.

**2. Track cash** -> `/finance:cashflow` tracks inflows and outflows, `/finance:runway` calculates months of runway, `/finance:forecast` projects revenue and expenses forward.

**3. Manage revenue** -> `/finance:revenue` tracks revenue streams and MRR/ARR, `/finance:invoices` manages invoice creation and collection.

**4. Control expenses** -> `/finance:expenses` categorizes and tracks spend, `/finance:vendors` manages vendor relationships, `/finance:burn` analyzes burn rate, `/finance:optimize` finds cost savings.

**5. Budget** -> `/finance:budget` creates and maintains budgets, `/finance:variance` compares budget vs actuals with root cause analysis.

**6. Tax** -> `/finance:tax-prep` prepares tax documentation, `/finance:deadlines` tracks every filing deadline so nothing is missed.

**7. Report** -> `/finance:report` generates financial reports, `/finance:pnl` produces P&L statements, `/finance:balance` creates balance sheets, `/finance:metrics` dashboards KPIs.

**8. Audit** -> `/finance:reconcile` matches bank balances to books, `/finance:audit` prepares for financial audits.

---

## Key features

### Cash flow forecasting
Project cash position forward with configurable scenarios — best case, expected, worst case. Factor in seasonality, payment cycles, and known future expenses. Runway calculation with burn rate trending.

### Budget variance analysis
Compare budget to actuals at any granularity — monthly, quarterly, by category, by department. Root cause analysis on significant variances. Auto-flag overspend before it becomes a problem.

### Revenue tracking
MRR/ARR tracking with cohort analysis, churn visibility, expansion revenue, and contraction. Revenue by stream, by customer segment, by product line. Trend analysis and growth rate calculation.

### Expense optimization
Identify cost reduction opportunities across vendors, subscriptions, and recurring costs. Benchmark spend against industry standards. Contract renewal tracking with renegotiation prompts.

### Scenario modeling
What-if analysis for major financial decisions — hiring plans, pricing changes, market expansion, capital allocation. Model multiple scenarios simultaneously with clear tradeoff analysis.

### Quality gates
Every financial report passes 7 checks: data verified, correct period, properly categorized, reconciled, assumptions stated, standard format, compliance met.

### Learnings system
FINANCE:OS learns from every analysis — revenue patterns, expense insights, cash flow timing, tax opportunities, vendor negotiation outcomes. Updated after every debrief, loaded before every session.

### Daily briefing
`/finance:today` scans everything and tells you what needs attention right now. Cash position, overdue invoices, upcoming deadlines, budget alerts, anomalies detected.

### Role-based experience
Different roles get different depth — Founders see runway and high-level metrics, CFOs get full depth and strategy, Controllers focus on reconciliation and compliance, Bookkeepers handle categorization and data entry.

### Parallel processing (Swarm)
Spin up parallel agents for batch categorization, multi-entity reconciliation, and large-scale report generation.

---

## Smart defaults, full control

FINANCE:OS ships with sensible defaults for everything:

- **Reporting:** standard P&L, balance sheet, and cash flow formats
- **Categories:** standard chart of accounts adaptable to any business
- **Tax:** calendar pre-loaded with common filing deadlines
- **Metrics:** standard financial KPIs with industry benchmarks
- **Compliance:** configurable regulation toggles per jurisdiction

**Every default is overridable per workspace.** Change category structures in ACCOUNTS.md, adjust metrics in METRICS.md, customize report formats in workspace config. If you don't override, the defaults just work.

---

## All commands

### Start
| Command | What it does |
|---------|-------------|
| `/finance:today` | Daily financial briefing — what needs attention now |
| `/finance:dashboard` | Full financial dashboard with all key metrics |

### Setup
| Command | What it does |
|---------|-------------|
| `/finance:onboard` | Create a new workspace — guided setup |
| `/finance:connect` | Connect bank accounts, payment tools |

### Cash flow
| Command | What it does |
|---------|-------------|
| `/finance:cashflow` | Cash flow analysis and projections |
| `/finance:runway` | Runway calculation and scenarios |
| `/finance:forecast` | Revenue and expense forecasting |
| `/finance:scenario` | What-if scenario modeling |

### Revenue
| Command | What it does |
|---------|-------------|
| `/finance:revenue` | Revenue tracking and analysis |
| `/finance:invoices` | Invoice management |

### Expenses
| Command | What it does |
|---------|-------------|
| `/finance:expenses` | Expense tracking and categorization |
| `/finance:vendors` | Vendor management and optimization |
| `/finance:burn` | Burn rate analysis |
| `/finance:optimize` | Cost optimization opportunities |

### Budget
| Command | What it does |
|---------|-------------|
| `/finance:budget` | Create or update budget |
| `/finance:variance` | Budget vs actual variance analysis |

### Tax
| Command | What it does |
|---------|-------------|
| `/finance:tax-prep` | Tax preparation checklist |
| `/finance:deadlines` | Upcoming tax and filing deadlines |

### Reporting
| Command | What it does |
|---------|-------------|
| `/finance:report` | Generate financial reports |
| `/finance:pnl` | Profit & loss statement |
| `/finance:balance` | Balance sheet |
| `/finance:metrics` | Key financial metrics dashboard |

### Audit
| Command | What it does |
|---------|-------------|
| `/finance:reconcile` | Bank reconciliation |
| `/finance:audit` | Financial audit preparation |

### Scale
| Command | What it does |
|---------|-------------|
| `/finance:swarm` | Run financial operations with parallel agents |

### Agency
| Command | What it does |
|---------|-------------|
| `/finance:portfolio` | Multi-company financial dashboard |

### Review
| Command | What it does |
|---------|-------------|
| `/finance:debrief` | End-of-period financial review and retrospective |
| `/finance:compliance` | Financial compliance and regulatory settings |

### System
| Command | What it does |
|---------|-------------|
| `/finance:feedback` | Submit feedback, report a bug, or request a feature |
| `/finance:status` | Show all commands and system status |

---

## Supported tools

FINANCE:OS connects to your existing financial stack. Use what you have — skip what you don't. Every tool is optional.

| Category | Tools |
|----------|-------|
| **Accounting** | QuickBooks, Xero |
| **Banking** | Mercury, Brex, Wise, Plaid |
| **Payments** | Stripe |
| **Payroll** | Gusto, Rippling |
| **Invoicing** | FreshBooks |
| **Expenses** | Ramp, Expensify |
| **Notifications** | Slack |

---

## Workspaces

Each workspace is fully isolated — its own accounts, budget, forecasts, vendors, metrics, and costs. A workspace can be:

- A company
- A business unit
- A subsidiary
- A client (for fractional CFOs)
- Any financial entity that deserves its own context

---

## Team mode (optional)

By default FINANCE:OS runs in **solo mode** — all state lives in markdown, no database needed.

For teams, enable **team mode** with Supabase:

1. Create a Supabase project
2. Add keys to `.env`
3. Configure in `global/COLLABORATION.md`

Adds: shared reconciliation queues, approval audit trails, multi-user cost tracking, live dashboards. Falls back to files automatically if Supabase is unreachable.

---

## Repo structure

```
FINANCEOS/
├── CLAUDE.md                  <- Entrypoint for Claude Code
├── FINANCEOS.md               <- All rules and behaviour
├── CHANGELOG.md               <- Version history
├── .env.example               <- API key template
├── _template/                 <- Workspace template (copied on onboard)
├── workspaces/                <- One folder per workspace
├── global/                    <- Cross-workspace standards
│   └── COLLABORATION.md       <- Solo/team mode config
├── .claude/
│   ├── commands/finance/      <- Slash commands (/finance:*)
│   └── financeos/references/  <- System references
│       ├── ui-brand.md         <- Visual system
│       ├── defaults.md         <- All overridable defaults
│       ├── accounting-principles.md <- Accounting standards
│       ├── metrics-glossary.md <- Financial metrics definitions
│       ├── BENCHMARKS.md       <- Industry benchmarks
│       ├── notifications.md    <- Slack alert config
│       ├── swarm.md            <- Parallel processing config
│       └── tool-pricing.md     <- Per-unit costs
```

---

## Feedback

Found a bug? Want a feature? Run `/finance:feedback` inside Claude Code, or open an issue on this repo.

---

## License

MIT — Built by [Shyft AI](https://shyft.ai)
