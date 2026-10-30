---
name: build-import-workflow
description: Build or extend a Feedesk CSV or XLSX import workflow from its approved template through validation, preview, approval, and posting.
---

# Build an import workflow

## Use this skill when

- You need to add an import type.
- You need to change an existing import template or parser.
- You need to change validation, preview, approval, or posting for an import type.

Do not use this skill to correct one record manually or to inspect a file without changing the import workflow.

## Procedure

1. Identify the import type requested by the user.
2. Find the matching requirements in the Feedesk PRD. Record the requirement IDs and the relevant acceptance criteria.
3. Read `AGENTS.md` and the applicable rules listed below.
4. Inspect the repository. Find the existing import code, related data types, templates, tests, and established patterns. Do not assume paths or scripts.
5. Confirm that the requested import type and its workflow are in the PRD. If they are not, stop and ask for approval.
6. Find the approved template version and its sample file. If the template or required columns are missing, stop and ask for the source material.
7. List the required columns, optional columns, accepted formats, and validation rules for this import type.
8. Trace how the current implementation receives, parses, validates, stores, previews, approves, and posts import data.
9. Plan the smallest change that implements the requested workflow. Keep import-specific parsing and validation separate from shared import handling.
10. Check whether the change requires a schema, migration, SQL, trigger, or database-state change. If it does, stop and request explicit authorization. Do not generate or run a workaround.
11. Implement file parsing and normalization for the approved format.
12. Implement file-level validation. Reject files that do not meet the template requirements.
13. Implement row-level validation. Return actionable errors that identify the affected row and field.
14. Implement the duplicate, unknown-column, reference, and amount checks required for this import type.
15. Ensure invalid batches cannot proceed to approval or posting.
16. Implement or update the draft and preview stages. Show the user the proposed changes before approval.
17. Connect approval and posting to the existing authorized workflow. Do not allow the requester to approve their own sensitive change.
18. Add or update focused tests for this import type. Cover a valid file, missing required columns, unknown columns, duplicate data, invalid values, and rejected batches.
19. Run only safe, relevant checks that do not mutate database state. Use verified repository scripts only.
20. Inspect the final diff. Confirm that the change is limited to the requested import workflow.
21. Report the files changed, checks actually run and their results, and any blocked decisions.

## Completion checks

- The implementation follows the approved template version.
- Required columns and accepted values are enforced.
- Invalid rows and files produce actionable errors.
- Duplicate and unknown data is handled as the PRD requires.
- Invalid batches cannot be approved or posted.
- Preview occurs before approval.
- Approval follows the existing authorization workflow.
- Focused tests cover valid and invalid inputs.
- No unauthorized database change was made.
- The report distinguishes completed checks from checks not run.

## Stop conditions

Stop and ask the user when:

- The PRD is silent, ambiguous, contradictory, or marks a required decision as open.
- The approved template or sample input is unavailable.
- The requested behaviour is outside the approved product scope.
- Implementation requires a database change that has not been explicitly authorized.
- A required provider, format, or policy has not been selected.

## Rules to follow

Read these files. Follow them as written. Do not copy their rules into this skill.

- `AGENTS.md`
- `rules/imports-reconciliation.md`
- `rules/database.md`
- `rules/auth.md`
- `rules/structure.md`
- `rules/commands.md`
- `rules/testing.md`
