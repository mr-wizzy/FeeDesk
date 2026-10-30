---
name: evaluate-shared-record-search
description: Run Feedesk's approved semantic-search evaluation sets and report routing, privacy, source-grounding, refusal, and acceptance results.
---

# Evaluate shared-record search

## Use this skill when

- You need to run the scripted search evaluation before a release.
- You need to run the real-query evaluation before launch or for the scheduled monthly review.
- You need to verify search behaviour after an approved search change.

Do not use this skill to select an embedding provider, change the search architecture, create evaluation data, or implement search features.

## Procedure

1. Identify the evaluation trigger: release, pre-launch, monthly review, or verification after an approved change.
2. Read the search evaluation requirements and acceptance thresholds in the Feedesk PRD.
3. Read `AGENTS.md` and the applicable rules listed below.
4. Inspect the repository. Find the evaluation datasets, search implementation, evaluation runner, test configuration, and existing reports. Do not assume paths or scripts.
5. Confirm that the required evaluation datasets are present and approved. The scripted set contains 14 queries. The real-query set contains 100 queries.
6. Confirm which evaluation set is required for this trigger. Run the 14-query scripted set before each release. Run the 100-query real set for pre-launch and monthly evaluation.
7. Check that the evaluation environment and its data are appropriate for the test. Do not use live private student, parent, payment, or ledger data.
8. Inspect the current search configuration. Record the version or commit being evaluated and the relevant configuration values.
9. Confirm that the configured embedding provider and model have been approved. If the provider or model decision is unresolved, stop and ask.
10. Run the required evaluation using the repository's verified command or documented procedure.
11. For each scripted case, record the expected route or search outcome and the observed result.
12. Check that routed questions go to their required destination and do not also invoke semantic search.
13. Check that query scrubbing occurs before any embedding-provider request.
14. Check that retrieval is limited to the active school's approved, published, current, effective shared records.
15. Check that private records and excluded record types do not enter retrieval results.
16. Check that returned content is approved source text with its source details. Do not accept generated summaries or unsupported answers.
17. Check that cases with no qualifying result produce the required refusal and named-contact fallback.
18. Compare the results with the PRD thresholds. The scripted set must pass all 14 cases. The real-query set must achieve at least 95% when that set is required.
19. Record every failed case with its query identifier, expected result, observed result, and failure category. Do not include private query text in the report if the PRD prohibits storing or exposing it.
20. Produce an evaluation report with the evaluation date, code version, dataset version, set run, pass rate, failed cases, and threshold result.
21. If a failure needs a code change, report it separately. Do not make an unrequested architecture or provider change as part of evaluation.
22. Run only safe, relevant checks that do not mutate database state. Use verified repository scripts only.
23. Report the files changed, checks actually run and their results, and any blocked decisions. If this was evaluation only, state that no implementation files were changed.

## Completion checks

- The correct evaluation set was run for the stated trigger.
- The dataset version and code version are recorded.
- Routing and privacy safeguards were checked.
- Retrieval uses only eligible shared records.
- Results are grounded in approved source text.
- Refusal and named-contact fallback behaviour was checked.
- Results are compared with the PRD thresholds.
- Failed cases are recorded without exposing prohibited private query content.
- The report clearly states whether the threshold was met.
- No unauthorized database change was made.

## Stop conditions

Stop and ask the user when:

- A required evaluation dataset is missing or unapproved.
- The evaluation environment could expose real private records.
- The embedding provider or model decision is unresolved.
- The PRD does not define the expected result or acceptance threshold for a required case.
- The requested change goes beyond evaluation into unapproved implementation work.
- A required check would mutate database state without explicit authorization.

## Rules to follow

Read these files. Follow them as written. Do not copy their rules into this skill.

- `AGENTS.md`
- `rules/ai-search.md`
- `rules/testing.md`
- `rules/env.md`
- `rules/commands.md`
