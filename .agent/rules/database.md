---
trigger: model_decision
description: Prisma and PostgreSQL data rules
---

# Prisma and PostgreSQL data rules

- Use Prisma and PostgreSQL as specified by the project stack.
  Reason: They are the approved data technologies for Feedesk.
- Do not change Prisma models, migrations, SQL, triggers, extensions, or database contents without explicit user authorization.
  Reason: The current project instruction restricts database changes.
- Do not bypass that restriction through scripts, ORM commands, tests, seeds, or other tools.
  Reason: A different route must not be used to perform the same unapproved change.
- Store money as integer minor units. Use the API representation specified in the PRD.
  Reason: Floating-point arithmetic can produce incorrect financial values.
- Use database transactions when the PRD requires several writes to succeed or fail together.
  Reason: Partial financial operations can leave records inconsistent.
- Preserve append-only behaviour for LedgerEntry, PaymentEvent, AuditLog, and StatementLine.
  Reason: These records form the financial and operational history.
- Do not invent fields, indexes, constraints, vector dimensions, or database capabilities.
  Reason: These details must come from the approved data model or an owner decision.
