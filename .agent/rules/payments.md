---
trigger: model_decision
description: Flutterwave integration
---

# Flutterwave integration rules

- Use Flutterwave as the selected card payment gateway.
  Reason: This is the gateway chosen for the Feedesk product.
- Verify current Flutterwave API, webhook, signature, transaction verification, and settlement requirements against official documentation before implementation.
  Reason: Provider behaviour can change and must not be guessed.
- Treat browser redirects and webhook notifications as unverified until the server verifies the transaction.
  Reason: A client or callback alone is not proof of payment.
- Require the idempotency behaviour defined in the PRD for payment initiation and event processing.
  Reason: Retries and duplicate events must not create duplicate payment records or effects.
- Keep gateway status separate from reconciliation, application, receipt, and clearance status.
  Reason: Each status describes a different stage of the payment process.
- Do not mark a payment successful until server-side verification confirms it.
  Reason: The portal must not report an unverified payment as successful.
- Do not expose gateway credentials in browser code.
  Reason: Browser code can be inspected by users. Secret storage is governed by `env.md`.
- Do not invent gateway event names, response fields, signature rules, or retry schedules.
  Reason: These details must be verified with the provider and the PRD.
