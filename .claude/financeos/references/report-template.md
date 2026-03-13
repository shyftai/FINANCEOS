# FINANCE:OS Report Templates

Standard formats for financial reports. Use these when running `/finance:report`.

---

## Daily Cash Position

```
<< FINANCE:OS // DAILY CASH >>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Date:** {date}
**Workspace:** {workspace}

### Cash Position

| Account | Balance | Change (24h) |
|---------|---------|-------------|
| Operating | ${n} | {+/-}${n} |
| Savings/Reserve | ${n} | {+/-}${n} |
| Payroll | ${n} | {+/-}${n} |
| **Total Cash** | **${n}** | **{+/-}${n}** |

### Runway
**Current burn:** ${n}/mo
**Cash runway:** {n} months
**Runway status:** {green/yellow/red}

### Pending Transactions

| Type | Count | Total |
|------|-------|-------|
| Invoices receivable (due < 7 days) | {n} | ${n} |
| Bills payable (due < 7 days) | {n} | ${n} |
| Pending payroll | {n} | ${n} |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Weekly Financial Summary

```
<< FINANCE:OS // WEEKLY SUMMARY >>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Period:** {start_date} – {end_date}
**Workspace:** {workspace}

### Revenue

| Source | This Week | Last Week | Trend | MTD |
|--------|-----------|-----------|-------|-----|
| Recurring (MRR) | ${n} | ${n} | {↑/↓/→} | ${n} |
| One-time | ${n} | ${n} | {↑/↓/→} | ${n} |
| Expansion | ${n} | ${n} | {↑/↓/→} | ${n} |
| **Total Revenue** | **${n}** | **${n}** | **{↑/↓/→}** | **${n}** |

### Expenses

| Category | This Week | Budget (weekly) | Variance |
|----------|-----------|----------------|----------|
| Payroll & contractors | ${n} | ${n} | {+/-}${n} |
| Software & tools | ${n} | ${n} | {+/-}${n} |
| Marketing & ads | ${n} | ${n} | {+/-}${n} |
| Infrastructure | ${n} | ${n} | {+/-}${n} |
| Other | ${n} | ${n} | {+/-}${n} |
| **Total Expenses** | **${n}** | **${n}** | **{+/-}${n}** |

### Key Ratios

| Metric | Current | Target | Status |
|--------|---------|--------|--------|
| Gross margin | {n}% | {n}% | {green/yellow/red} |
| Net burn rate | ${n}/mo | ${n}/mo | {green/yellow/red} |
| Cash runway | {n} months | {n} months | {green/yellow/red} |

### Accounts Receivable Aging

| Aging | Count | Amount | % of Total |
|-------|-------|--------|-----------|
| Current (0-30 days) | {n} | ${n} | {n}% |
| 31-60 days | {n} | ${n} | {n}% |
| 61-90 days | {n} | ${n} | {n}% |
| 90+ days | {n} | ${n} | {n}% |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Monthly P&L Report

```
<< FINANCE:OS // MONTHLY P&L >>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Period:** {month} {year}
**Workspace:** {workspace}

### Income Statement

| Line Item | Actual | Budget | Variance | vs. Last Month |
|-----------|--------|--------|----------|---------------|
| **Revenue** | | | | |
| Recurring revenue | ${n} | ${n} | {+/-}${n} | {↑/↓/→} |
| Services revenue | ${n} | ${n} | {+/-}${n} | {↑/↓/→} |
| Other income | ${n} | ${n} | {+/-}${n} | {↑/↓/→} |
| **Total Revenue** | **${n}** | **${n}** | **{+/-}${n}** | |
| | | | | |
| **COGS** | | | | |
| Hosting & infrastructure | ${n} | ${n} | {+/-}${n} | |
| Third-party costs | ${n} | ${n} | {+/-}${n} | |
| **Total COGS** | **${n}** | **${n}** | **{+/-}${n}** | |
| **Gross Profit** | **${n}** | **${n}** | **{+/-}${n}** | |
| **Gross Margin** | **{n}%** | **{n}%** | | |
| | | | | |
| **Operating Expenses** | | | | |
| Payroll & benefits | ${n} | ${n} | {+/-}${n} | |
| Contractors | ${n} | ${n} | {+/-}${n} | |
| Software & subscriptions | ${n} | ${n} | {+/-}${n} | |
| Marketing & advertising | ${n} | ${n} | {+/-}${n} | |
| Office & admin | ${n} | ${n} | {+/-}${n} | |
| Professional services | ${n} | ${n} | {+/-}${n} | |
| **Total OpEx** | **${n}** | **${n}** | **{+/-}${n}** | |
| | | | | |
| **Net Income (Loss)** | **${n}** | **${n}** | **{+/-}${n}** | |
| **Net Margin** | **{n}%** | **{n}%** | | |

### SaaS Metrics

| Metric | This Month | Last Month | Trend |
|--------|-----------|-----------|-------|
| MRR | ${n} | ${n} | {↑/↓/→} |
| Net new MRR | ${n} | ${n} | {↑/↓/→} |
| Churned MRR | ${n} | ${n} | {↑/↓/→} |
| Expansion MRR | ${n} | ${n} | {↑/↓/→} |
| NRR | {n}% | {n}% | {↑/↓/→} |
| CAC payback (months) | {n} | {n} | {↑/↓/→} |
| LTV:CAC | {n}:1 | {n}:1 | {↑/↓/→} |

### Cash Flow Summary

| Category | Amount |
|----------|--------|
| Cash from operations | ${n} |
| Cash from investing | ${n} |
| Cash from financing | ${n} |
| **Net change in cash** | **${n}** |
| **Ending cash balance** | **${n}** |

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Notes

- All reports auto-populate from workspace data where available
- Financial data requires integration with accounting platform (QBO, Xero, Stripe)
- Benchmark comparisons reference BENCHMARKS.md thresholds
- Always verify figures against source systems before sharing externally
