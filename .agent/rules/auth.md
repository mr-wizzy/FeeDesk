---
trigger: model_decision
description: Authentication and authorization
---

# Authentication and authorization rules

- Parents must sign in through the PRD's one-time-code flow. Do not add parent passwords.
  Reason: The PRD defines one-time-code sign-in for parents.
- Staff must use passwords and the second factor required for their roles.
  Reason: Staff accounts can access sensitive school operations.
- Store only hashes of one-time codes and session tokens.
  Reason: A database disclosure should not reveal usable credentials.
- Apply the PRD's code expiry, rate limits, session expiry, cookie settings, and recovery rules.
  Reason: These controls limit account misuse.
- Check the session and permissions on every protected request.
  Reason: A hidden interface control does not prevent a direct request.
- Give a parent access only to students connected through an approved guardian link.
  Reason: A parent must not see another family's records.
- Do not let a parent approve their own guardian link.
  Reason: The approval must be independent.
- Apply the staff roles and approval responsibilities defined in the PRD.
  Reason: Sensitive actions must be performed by the correct staff role.
- Revoke access when the approved policy requires it for a withdrawn or inactive student.
  Reason: Former guardians must not retain access without authorization.
