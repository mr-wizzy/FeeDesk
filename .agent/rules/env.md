---
trigger: model_decision
description: Environments and deployment
---

# Environment and deployment rules

- Keep development, staging, and production separate.
  Reason: Testing must not affect live school operations.
- Use fictional data in development and staging. Never copy production data into either environment.
  Reason: Real student and financial records must remain protected.
- Use gateway test mode in staging. Use live gateway credentials only in production after launch approval.
  Reason: Test payments must not move real funds.
- Store database, gateway, OTP provider, and session secrets in the deployment platform's secret store.
  Reason: Credentials must not be exposed in source control or application code.
- Rotate keys every 90 days and after suspected exposure, as specified by the PRD.
  Reason: Rotation limits the impact of leaked credentials.
- Restrict production access to named engineers and log that access.
  Reason: Access to live school data must be controlled and traceable.
- Follow the PRD's backup and restore requirements. Keep any marked assumptions identified until verified.
  Reason: The school needs a tested recovery path if data or service is lost.
- Verify pgvector and point-in-time recovery support in the selected database service before launch.
  Reason: The PRD marks these provider capabilities for verification.
- Do not choose a hosting provider, database region, or secret manager without an approved decision.
  Reason: The PRD leaves these deployment choices open.
