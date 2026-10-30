---
trigger: model_decision
description: Testing and acceptance
---

# Testing rules

- Link each test to a PRD requirement or acceptance criterion.
  Reason: Tests must show whether Feedesk meets its agreed requirements.
- Test parent and staff access boundaries, including guardian links, roles, revoked sessions, and school separation.
  Reason: Unauthorized access could expose student information.
- Test balances against approved ledger entries using the PRD's money rules.
  Reason: Incorrect balances undermine the main purpose of Feedesk.
- Test payment initiation, verification, retries, duplicate events, and failure states.
  Reason: Payment errors can create false confirmations or duplicate records.
- Test import validation, preview, rejection, and approval.
  Reason: Invalid imports must not change school records.
- Test exact reconciliation rules, discrepancies, and duplicate-match prevention.
  Reason: Incorrect matching can assign payments to the wrong student.
- Test that a matched payment does not affect the ledger until staff apply it, and that receipt issuance follows application.
  Reason: Payment stages must remain distinct.
- Test semantic search routing, privacy filters, source output, and refusal behaviour.
  Reason: Search must not expose private records or invent answers.
- Test mobile layouts and the loading, empty, and error states required by the PRD.
  Reason: Users need clear feedback on different devices and network conditions.
- Run the PRD's scripted and real-query search evaluations before releasing semantic search.
  Reason: The PRD requires both forms of evaluation.
- Report which tests ran, which did not, and the observed results.
  Reason: Release decisions must be based on evidence.
