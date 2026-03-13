---
name: finance:feedback
description: Submit feedback, report a bug, or request a feature
argument-hint: "[feedback text]"
---
<objective>
Capture user feedback about FINANCE:OS for improvement.

Feedback: $ARGUMENTS
</objective>

<process>
1. Display: `<< FINANCE:OS // FEEDBACK >>`
2. Categorize: bug report, feature request, or general feedback
3. Log to global/feedback-log.md with timestamp and category
4. If bug: note steps to reproduce
5. If feature request: note use case and expected benefit
6. Acknowledge receipt
</process>
