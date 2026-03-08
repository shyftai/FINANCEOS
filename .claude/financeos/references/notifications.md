# Slack Notifications

## Alert types

| Alert | Channel | Trigger |
|-------|---------|---------|
| Runway critical | #finance | Runway drops below 3 months |
| Runway warning | #finance | Runway drops below 6 months |
| Cash balance alert | #finance | Balance below threshold |
| Invoice overdue | #finance | Invoice past due date |
| Tax deadline | #finance | Filing due within 7 days |
| Budget variance | #finance | Variance >15% on any line |
| Reconciliation gap | #finance | Bank vs book mismatch |
| Vendor renewal | #finance | Contract renewal within 30 days |

## Severity levels

| Level | When | Action |
|-------|------|--------|
| 🔴 Critical | Runway <3mo, cash crisis, missed filing | Immediate attention |
| 🟡 Warning | Runway <6mo, high variance, overdue invoices | Address today |
| 🟢 Info | Reports generated, reconciliation complete | FYI |
