---
trigger: model_decision
description: AI semantic search
---

# Shared-record semantic search rules

- Limit semantic search to approved and published shared school records.
  Reason: The PRD permits AI retrieval only for this content.
- Do not connect Gemini until the product owner confirms its intended role.
  Reason: The PRD leaves Gemini's exact role unresolved.
- Route each query using the PRD's rules before creating an embedding.
  Reason: Queries that need another route must not enter semantic search.
- Scrub the query before sending it to the embedding provider.
  Reason: The PRD requires sensitive query content to be removed before embedding.
- Filter by the user's school, publication status, and effective dates before ranking results.
  Reason: Search must not return records from another school or records that are no longer current.
- Exclude private student, ledger, payment, receipt, correction, clearance, and internal escalation records.
  Reason: These records are outside the approved search scope.
- Return the approved source text and source details. Do not generate summaries or combine records.
  Reason: The PRD requires source-grounded retrieval without generated answers.
- Use the PRD's refusal and named-contact response when no result qualifies.
  Reason: Search must not invent an answer when evidence is missing.
