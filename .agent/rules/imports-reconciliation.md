---
trigger: model_decision
description: Imports and payment matching
---

# Import and reconciliation rules

## Imports

- Use the import template versions and required column names defined in the PRD.
  Reason: Staff need a consistent format for school data.
- Reject unknown columns and duplicate external references where the PRD requires it.
  Reason: Unexpected or repeated data can corrupt imported records.
- Validate each row before it can be approved.
  Reason: Invalid data must not enter school records.
- Show a preview and row-level errors before approval.
  Reason: Staff need to review the proposed changes.
- Do not apply an import before the required approval.
  Reason: Uploading a file is not approval to change records.

## Reconciliation

- Keep bank statement lines and gateway settlement lines as separate sources.
  Reason: They represent different payment methods and matching rules.
- Match card settlements only by the exact gateway reference and equal amount specified in the PRD.
  Reason: A loose match can assign money to the wrong payment.
- Suggest a transfer match only when the payment reference appears exactly in the narration and the amount is equal.
  Reason: The PRD requires an exact reference and amount.
- Require staff confirmation for transfer matches.
  Reason: A suggestion is not an approved match.
- Create a discrepancy when amounts differ. Do not mark the payment as matched.
  Reason: An unequal amount needs staff review.
- Enforce the PRD's limit of one matched reconciliation per payment and per statement line.
  Reason: A payment or statement line must not be applied twice.
- Match a payment only to the student named on that payment.
  Reason: Similar names must not cause funds to be assigned to the wrong student.
- Do not use AI or fuzzy matching for financial reconciliation.
  Reason: Financial matching must follow the explicit rules, not similarity scores.
