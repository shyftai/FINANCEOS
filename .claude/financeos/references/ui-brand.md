<ui_patterns>

Visual patterns for user-facing FINANCE:OS output. All commands @-reference this file.

## Brand Color

FINANCE:OS brand color is **green** (ANSI 34).
- Use `\033[38;5;34m` to set green text, `\033[0m` to reset
- Apply green to: the FINANCE:OS block-letter banner, mode headers (`<< FINANCE:OS // MODE >>`), section titles in dashboards
- Use white/default for: body text, data values, box borders
- If terminal doesn't support color, display everything in plain white
- Never use orange, blue, or red as brand colors — those are reserved for status indicators

## Startup Banner

Display once when FINANCE:OS loads. Render the block letters in green.

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
  F I N A N C E : O S
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Mode Headers

Use for workflow transitions. FINANCE:OS uses angle brackets.

```
<< FINANCE:OS // {MODE NAME} >>
```

**Mode names (uppercase, rendered in green):**
- `ONBOARDING`
- `TODAY`
- `DASHBOARD`
- `CASHFLOW`
- `RUNWAY`
- `FORECAST`
- `SCENARIO`
- `REVENUE`
- `INVOICES`
- `EXPENSES`
- `VENDORS`
- `BURN`
- `OPTIMIZE`
- `BUDGET`
- `VARIANCE`
- `TAX PREP`
- `DEADLINES`
- `REPORT`
- `PNL`
- `BALANCE`
- `METRICS`
- `RECONCILE`
- `AUDIT`
- `PORTFOLIO`
- `CONNECT`
- `DEBRIEF`
- `FEEDBACK`
- `COMPLIANCE`
- `SWARM`

---

## System Flow Diagram

Show below the banner on startup.

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

---

## Workspace Header

Show after loading a workspace. Always displayed before any task.

```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  WORKSPACE: {Name}                                         ┃
┃  ENTITY:    {Company/entity name}                          ┃
┃  STATUS:    {active/paused}         TOOLS: {tool list}     ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

---

## Approval Gates

When human decision is needed. Uses double borders.

**Interactive mode:**
```
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃  APPROVAL REQUIRED                                         ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛

{What is being approved}

>> Approve / Edit / Reject
```

**Auto mode:** Skip the gate — auto-approve and continue. Show a one-line confirmation instead:
```
>> Auto-approved: {what was approved}
```
Log the auto-approval in `logs/decisions.md` with timestamp and what was approved.

**Hard gates (always stop, even in auto mode):**
- Tax filings — always requires explicit approval before filing
- External payments — never auto-approve outbound payments
- Reconciliation sign-offs — always requires explicit approval to mark reconciled
- Budget overages — always flags if spend exceeds budget allocation
- Compliance/legal violations — never auto-skip regulation violations

Hard gates always use the full approval gate format above, regardless of execution mode.

---

## Credit Check

Before any tool write operation.

```
  ┌ CREDIT CHECK ─────────────────────────────────┐
  │ Tool:   {tool name}                            │
  │ Action: {what will happen}                     │
  │ Cost:   {record count or credit estimate}      │
  │ Rule:   {confirm-before-every-use / auto / ..} │
  └────────────────────────────────────────────────┘
  >> Proceed? (y/n)
```

---

## Quality Gates

Run before every financial output. Compact inline format.

```
  ── QUALITY GATES ──────────────────────────────────
  [x] Data sourced    [x] Period correct   [x] Categorized
  [x] Reconciled      [x] Assumptions      [x] Format
  [x] Compliance
  ─────────────────────────────────────────────────
```

If a check fails:
```
  ── QUALITY GATES ──────────────────────────────────
  [x] Data sourced    [x] Period correct   [ ] Categorized
  [x] Reconciled      [x] Assumptions      [x] Format
  [x] Compliance
  ─────────────────────────────────────────────────
  !! Categorized: 12 transactions uncategorized in March
```

---

## Status Symbols

```
[x]  Passed / Complete
[ ]  Failed / Missing
[~]  Borderline / Needs review
>>   Action prompt — waiting for human input
!!   Warning or flag
--   Skipped / Not applicable
```

---

## Financial Health Dashboard

```
  ┌─ FINANCIAL HEALTH ──────────────────────────────┐
  │                                                  │
  │  Cash Position    [GREEN]  $124,500              │
  │  Burn Rate        [AMBER]  $18,200/mo            │
  │  Runway           [GREEN]  6.8 months            │
  │  Budget Variance  [GREEN]  -2.1% under budget    │
  │                                                  │
  │  Overall: HEALTHY                                │
  └──────────────────────────────────────────────────┘
```

---

## Completion Summary

Always at end of a completed workflow.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  >> Next: {suggested next command}
     Also: {alternative command}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Swarm Display

Spawning indicator:
```
  << FINANCE:OS // SWARM >>

  Spawning 4 agents...
    [~] Agent 1: invoices 1-25
    [~] Agent 2: invoices 26-50
    [~] Agent 3: accounts 1-5 reconciliation
    [~] Agent 4: vendor analysis batch
```

Swarm results summary:
```
  ┌─ SWARM RESULTS ─────────────────────────────────┐
  │                                                  │
  │  Agents spawned:  4                              │
  │  Items processed: 55                             │
  │  Completed:       52                             │
  │  Flagged:         3 (need manual review)         │
  │  Errors:          0                              │
  │                                                  │
  └──────────────────────────────────────────────────┘

  >> Options:
     1. Review all results
     2. Approve clean items, review flagged only
     3. Export for offline review
```

---

## Anti-Patterns — What FINANCE:OS never does

- Never use `GSD >` prefix — that is GSD's brand
- Never use `GTM:OS` headers — that is GTM:OS's brand
- Never use random emoji (no rockets, sparkles, stars)
- Never use `<< GTM:OS` — always use `<< FINANCE:OS`
- Never vary box widths within the same output
- Never skip the quality gates display on financial reports
- Never show financial data without source attribution

</ui_patterns>
