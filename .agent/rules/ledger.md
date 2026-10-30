---
trigger: model_decision
description: Financial records and balances
---

# Ledger and financial record rules

- Treat approved ledger entries as the source for student balances.
  Reason: The ledger is the school's approved financial record.
- Calculate balances on the server from approved entries for the relevant student and term.
  Reason: Client calculations must not determine financial truth.
- Use integer minor units for all money calculations.
  Reason: This avoids floating-point rounding errors. Storage rules are defined in `database.md`.
- Do not change a ledger balance when a payment is initiated, declared, or verified by the gateway.
  Reason: A gateway result alone does not apply money to a student's account.
- Post a ledger credit only through the approved payment-application process.
  Reason: Staff must control when a payment changes the school ledger.
- Post the ledger credit and receipt together as required by the PRD.
  Reason: A receipt must correspond to an applied ledger transaction.
- Show a receipt only when the apply process has issued it.
  Reason: A gateway success or reconciliation match is not a receipt.
- Show clearance only when an eligible, valid clearance record exists.
  Reason: A balance alone must not be used to invent a clearance status.
- Preserve corrections and historical financial records according to the PRD.
  Reason: The school needs a reliable record of past changes.
