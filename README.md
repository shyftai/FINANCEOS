# FINANCE:OS

A financial operations system that runs inside Claude Code. Track cash flow, manage budgets, forecast runway — all from the terminal.

Not a spreadsheet. Not a dashboard tool. An operating system for finance teams.

## Install

```bash
git clone https://github.com/shyftai/FINANCEOS.git
cd FINANCEOS
claude
```

That's it. FINANCE:OS boots automatically when Claude Code opens the repo.

### Connect your tools (recommended)

| MCP Server | What it unlocks |
|-----------|----------------|
| **QuickBooks** | Direct accounting data access |
| **Stripe** | Payment and revenue data |
| **Xero** | Accounting data (alternative to QB) |
| **Slack** | Team notifications and alerts |

Add them in Claude Code → Settings → MCP Servers.

### API keys (optional)

Copy `.env.example` to `.env` and add keys for deeper integrations:

- **Accounting:** QuickBooks, Xero
- **Banking:** Mercury, Brex, Wise, Plaid
- **Payments:** Stripe
- **Payroll:** Gusto, Rippling
- **Invoicing:** FreshBooks
- **Expenses:** Ramp, Expensify
- **Scale:** Anthropic API (for batch processing)

## What it does

### Sources of truth
| File | Purpose |
|------|---------|
| `ACCOUNTS.md` | Chart of accounts, bank connections |
| `BUDGET.md` | Annual/quarterly budget with line items |
| `CASHFLOW.md` | Cash flow tracking and projections |
| `FORECAST.md` | Revenue and expense forecasts |
| `REVENUE.md` | Revenue streams, MRR/ARR tracking |
| `EXPENSES.md` | Expense categories, recurring costs |
| `VENDORS.md` | Vendor list, contracts, payment terms |
| `TAX-CALENDAR.md` | Tax deadlines and filings |
| `METRICS.md` | KPIs, benchmarks, targets |
| `LEARNINGS.md` | Persistent financial intelligence |
| `ROADMAP.md` | Financial planning pipeline |

### Roles

| Role | Focus |
|------|-------|
| **Founder** | Cash flow, runway, high-level metrics |
| **CFO** | Everything — full depth, strategy, reporting |
| **Controller** | Reconciliation, compliance, reporting |
| **Bookkeeper** | Categorization, reconciliation, data entry |
| **Fractional CFO** | Multi-company, portfolio, advisory |

## Commands

| Category | Commands |
|----------|---------|
| **Daily** | `today`, `dashboard` |
| **Setup** | `onboard`, `connect` |
| **Cash** | `cashflow`, `runway`, `forecast`, `scenario` |
| **Revenue** | `revenue`, `invoices` |
| **Expenses** | `expenses`, `vendors`, `burn`, `optimize` |
| **Budget** | `budget`, `variance` |
| **Tax** | `tax-prep`, `deadlines` |
| **Report** | `report`, `pnl`, `balance`, `metrics` |
| **Audit** | `reconcile`, `audit` |
| **Agency** | `portfolio` |

All commands prefixed with `/finance:` — e.g., `/finance:runway`

---

v1.0.0

MIT — Built by [Shyft AI](https://shyft.ai)
