# Feedesk Agent Instructions

## Product purpose
Feedesk is a free, mobile-first web application for school fee tracking and payment. It connects parents to approved school records and lets authorized staff manage imports, reconciliation, payment application, receipts, access, and school information. It must never present an unverified financial fact as confirmed.

## Source of truth and decision handling
- Treat `docs/feedesk_prd_v1.1.md` as the product requirements source. Read the relevant sections and acceptance criteria before implementing a feature; do not rely on this summary where the PRD is more specific.
- Preserve the PRD's terms, state distinctions, roles, and acceptance criteria. Do not silently resolve contradictions, assumptions, or open questions.
- If the PRD is silent, ambiguous, contradictory, or marks a decision/verification as outstanding, stop the affected work and ask the user for a decision. State the blocking question and why it matters. Do not invent a default.
- Gemini's exact role is unresolved. Before implementing or connecting Gemini, ask the user to confirm its intended role. The PRD currently permits AI only for meaning-based retrieval over approved, published shared records; do not broaden that role without approval.
- Keep MVP scope aligned with the PRD. Do not add unrequested features, external services, or future-version capabilities.

## Users and authorization
- Parents/guardians may access only children linked through a school-approved guardian relationship.
- Staff permissions and approver responsibilities must follow the PRD. A parent cannot approve their own guardian link or change charges, clear debt, approve refunds, or alter financial records.
- Enforce school scope and authorization on the server for every private-record request, not only in the interface.
- When a child is withdrawn or inactive, revoke former guardian access according to the approved policy while preserving historical financial records for authorized staff.
- Never expose another family's records, staff-only notes, internal escalation details, or information from another school.

## Financial correctness and payment states
- Compute balances and totals using fixed arithmetic over approved ledger entries. Never use AI or client-side calculations as the source of financial truth.
- Keep gateway status, reconciliation/match status, application status, receipt status, and clearance status distinct. Do not infer one from another.
- A successful gateway result is not a school-issued receipt. A matched payment does not change the ledger until an authorized staff member applies it.
- Follow the PRD's verification, idempotency, duplicate-event, retry, timeout, discrepancy, and dispute rules. A repeated event or request must not create a second charge, event, ledger entry, or receipt.
- Failed, pending, unknown, disputed, unmatched, or unverified payments must not be described as paid or cleared. Never change balances on payment initiation or failure.
- Only issue or display a receipt when the approved apply process has issued it. Never fabricate a receipt number or promise clearance without an authoritative clearance record.
- Parent-facing bank-transfer declarations are not confirmation of receipt. Use the PRD's exact states and wording, and require staff reconciliation before application.
- Do not provide parent controls for refunds, reversals, or cancellations. Disputed payments remain under staff review.

## Imports, ledger changes, reconciliation, and audit
- Follow the PRD's versioned import templates, required columns, validation, duplicate checks, unknown-column rejection, draft/preview, and approval workflow.
- Do not apply unvalidated import rows. Show actionable validation errors and a preview of changes before approval.
- Follow the PRD's exact reconciliation matching rules. Do not use AI or fuzzy semantic matching to decide financial matches.
- Apply payments and other sensitive financial changes only through the approved workflow, including required maker-checker approval.
- Treat audit history as append-only and preserve correction history. Record the actions and outcomes required by the PRD; never silently overwrite history or hide a failed/denied action that the PRD requires logged.
- If a requested feature needs a database schema, migration, SQL file, trigger, or any database-state change, do not make that change. Stop and ask the user for explicit authorization before proceeding. Do not work around this restriction through generated code, scripts, ORM commands, tests, seeds, or tooling.

## Setup and school readiness
- Implement the school setup flow against the PRD's required steps, including record imports, duplicate resolution, ledger verification, payment and settlement configuration, staff permissions, bursary contact, opening-ledger approval, and parent-access testing.
- Do not show a blank or missing balance as zero. Do not enable payment for an account whose records are not verified.
- If setup or approved records are incomplete, clearly state that setup is incomplete and direct the user to the named school contact.
- Keep school configuration, published shared records, and approved financial records within the correct school boundary.

## AI, semantic search, and privacy
- AI is not a financial decision-maker, calculator, reconciliation tool, general chatbot, or source of truth.
- Until the user resolves Gemini's role, do not implement Gemini integration. If later approved, follow the PRD and the user's explicit decision.
- Meaning-based search may use only approved, published shared records for the active school. Never search private records, ledgers, payment history, receipts, corrections, clearance records, or escalation notes by meaning.
- Apply the PRD's routing rules before semantic search. Routed questions must go to the relevant screen and must not also run search.
- Scrub query text as specified in the PRD before any embedding-provider call. Never send the original unsanitized query to a third party or store raw query text where the PRD forbids it.
- Apply school, current-publication, and effective-date filters before ranking search results. Exclude retracted, stale, expired, and other-school records.
- Return approved published text with its source details. Do not generate summaries, paraphrase sources, combine records, or answer from general knowledge.
- If no approved result qualifies, use the PRD's refusal and named-contact fallback. Never reveal internal notes or staff-only information.
- Follow the PRD's scripted and real-query evaluation plans before release; a search failure must degrade to refusal, not an invented answer.

## Security, authentication, and errors
- Implement parent one-time-code sign-in and staff authentication with the second factor required by the PRD. Do not introduce parent passwords.
- Follow the PRD for guardian-link issuance, single-use and expiry rules, staff approval, account recovery, session visibility, and session revocation.
- Treat gateway callbacks as untrusted until their signature and transaction status are verified as specified in the PRD.
- Handle slow or lost connections with clear, retryable states while keeping payment state on the server. Never ask a client to guess a payment outcome.
- Reject unauthorized requests without changing state. Log access denials and other security events where the PRD requires them.
- Do not claim production readiness until the PRD's relevant security, backup/recovery, privacy, provider, and launch gates have been verified by the responsible owner.

## Technology and repository workflow
- Required stack: Next.js, TypeScript, Prisma, PostgreSQL, Flutterwave for payments, pgvector for vector data, and Gemini with its role unresolved as noted above.
- The repository structure has not been established. Inspect the actual repository before editing; do not assume directories, files, scripts, conventions, or installed dependencies exist.
- Reuse existing patterns when they are consistent with the PRD. Prefer the smallest change that fully meets the requirement.
- Do not add or upgrade dependencies without asking first. Do not run destructive commands or commands that mutate database state.
- Never modify database schemas, migrations, SQL, or database state under the current restriction. If implementation requires any of these, stop and request explicit authorization.
- Do not create detailed rules files or claim they exist unless they are present in the repository. Keep these instructions self-contained until the user approves and creates a more detailed rules structure.
- Before editing, inspect relevant files and tests. After editing, run only safe, relevant checks that do not alter database state; report exactly what ran and what did not.
- Do not claim a test passed unless it was actually run and its result was observed.

## Scope exclusions
Unless the PRD and user explicitly approve a change, do not build:
- Automated parent reminders, follow-ups, or message-based receipts.
- AI-generated financial answers, AI agents acting for parents, or payment-behaviour predictions.
- Automatic waivers, refunds, reversals, or balance changes.
- Parent refund/reversal/cancellation controls.
- Multiple active gateways, multi-school pilot features beyond the PRD, sponsor access to full financial history, offline payment initiation or offline balances, or native mobile apps.
- Cross-school or private-record semantic search, internet-sourced answers, or analytics beyond the pilot's stated needs.
- Future revenue features, advertising on financial screens, parent portal fees, or any arrangement where Feedesk holds or distributes school funds.

## Completion and release checks
- Trace each change to the relevant PRD requirement and acceptance criteria.
- Test parent access boundaries, multiple children, unauthorized access, missing/unapproved records, and withdrawn-student access where relevant.
- Test payment state transitions, webhook verification, idempotency, duplicate detection, failure/retry paths, reconciliation, payment application, and receipt issuance where relevant.
- Test import validation, preview/approval, correction history, audit behavior, and setup-completion gates where relevant.
- Test semantic-search routing, query scrubbing, school/publication/effective-date filtering, refusal behavior, and the PRD's evaluation sets where relevant.
- Confirm errors are actionable, mobile-first screens remain usable on weak connections, and no private information leaks in logs or fallback messages.
- Compare relevant metrics and acceptance results with the definitions in the PRD. Do not invent targets or replace PRD thresholds with new ones.

## Work log and response format
- Maintain `journal.md` at the repository root as the milestone log. Inspect it first; create it only if needed and permitted. Record each meaningful milestone with date and time, work completed, files changed, checks run and outcomes, blockers, and decisions still needed.
- After each task, report: summary of changes, files changed, checks actually run and their results, unresolved blockers/decisions, and assumptions or interpretations. If there are no assumptions, say so.
- Do not state that the task is complete while a required decision, blocked database change, or required check remains unresolved.
