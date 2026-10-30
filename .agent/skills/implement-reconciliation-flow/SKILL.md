---
name: implement-reconciliation-flow
description: Implement or change Feedesk bank-transfer or card-settlement reconciliation, including exact matching, discrepancy handling, and staff confirmation.
---

# Implement a reconciliation flow

## Use this skill when

- You need to implement or change bank-transfer reconciliation.
- You need to implement or change card-settlement reconciliation.
- You need to change how statement lines and payments are matched or marked as discrepancies.

Do not use this skill to display payment status, investigate one transaction manually, or apply a matched payment to the ledger.

## Procedure

1. Identify the reconciliation source: bank statement or gateway settlement report.
2. Find the matching PRD requirements and acceptance criteria for that source.
3. Read `AGENTS.md` and the applicable rules listed below.
4. Inspect the repository. Find the payment, statement import, reconciliation, staff review, and test implementations. Do not assume paths or scripts.
5. Confirm that the requested reconciliation behaviour is supported by the PRD.
6. Check whether the behaviour depends on an unresolved decision. In particular, confirm the approval status of the bank-transfer approach and the selected gateway. If approval is unresolved, stop and ask.
7. Record the exact matching fields and conditions required by the PRD for this source. Do not add fuzzy or inferred matching.
8. Trace the current flow from imported statement line to candidate payment, match decision, discrepancy, and staff confirmation.
9. Plan the smallest change that implements the requested reconciliation behaviour.
10. Check whether the change requires a schema, migration, SQL, trigger, or database-state change. If it does, stop and request explicit authorization. Do not generate or run a workaround.
11. Implement the matching procedure using only the approved matching fields and conditions.
12. Implement the required amount comparison. Record unequal amounts as discrepancies rather than matches.
13. Implement the required handling for missing references, unmatched lines, duplicate candidates, and already-matched records.
14. Enforce the PRD's one-match-per-payment and one-match-per-statement-line restrictions through the existing approved implementation.
15. For bank transfers, preserve the required staff confirmation step. Do not treat a parent declaration or an automatic suggestion as confirmation.
16. Keep matching separate from payment application. Do not create a ledger credit, issue a receipt, or mark a payment as applied in this workflow.
17. Connect discrepancy and review outcomes to the existing staff workflow.
18. Add or update focused tests for this source. Cover an exact match, unequal amount, missing or incorrect reference, duplicate candidate, previously matched payment, previously matched statement line, and required staff confirmation.
19. Run only safe, relevant checks that do not mutate database state. Use verified repository scripts only.
20. Inspect the final diff. Confirm that it changes reconciliation only and does not change ledger application or receipt issuance.
21. Report the files changed, checks actually run and their results, and any unresolved decisions.

## Completion checks

- The correct source-specific matching rules are implemented.
- A match uses only the PRD-approved conditions.
- Unequal amounts produce a discrepancy.
- Duplicate matches are prevented.
- Bank-transfer staff confirmation is preserved where required.
- Matching does not post a ledger entry.
- Matching does not issue a receipt.
- Tests cover the source-specific match and exception cases.
- No unauthorized database change was made.
- The report distinguishes completed checks from checks not run.

## Stop conditions

Stop and ask the user when:

- The PRD does not define the matching condition.
- The bank-transfer approach or gateway choice is not approved.
- The requested behaviour conflicts with the PRD.
- Implementation requires an unauthorized database change.
- The existing implementation cannot safely enforce a required uniqueness or confirmation condition.

## Rules to follow

Read these files. Follow them as written. Do not copy their rules into this skill.

- `AGENTS.md`
- `rules/imports-reconciliation.md`
- `rules/payments.md`
- `rules/ledger.md`
- `rules/database.md`
- `rules/auth.md`
- `rules/commands.md`
- `rules/testing.md`
