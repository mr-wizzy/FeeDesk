# Feedesk Product Requirements Document, Version 1.1

**Status:** Revised draft | **Date:** [DATE] | **Owner:** [OWNER NAME] | **Replaces:** Version 1.0

## How to read this document

Tags in square brackets mark items that need attention. [ASSUMPTION] marks a choice the concept did not specify. [VERIFY BEFORE LAUNCH] marks a fact about a gateway, a regulation, a price, or a tool behavior that the team must confirm. [DECISION NEEDED] marks a choice that an owner must approve. Every assumption appears in the Assumptions Log at the end.

---

## 1. Product Summary

Feedesk is a free, mobile-first web application for school fee tracking and payment. It shows parents what each child owes. It lets parents pay by card through the school's payment gateway, or by bank transfer to the school account. The school chooses the payment path at setup. Feedesk shows each payment's gateway status, match status, and receipt status. It shows only the school's own approved records. When Feedesk cannot answer a question safely, it names the school staff member who can.

Feedesk charges no portal fee to schools or parents. Any gateway fee is a payment charge. The school decides who bears it. The amount shows before payment.

---

## 2. Problem

Many community schools keep fee records in spreadsheets and paper ledgers. During term resumption, parents call or message the bursary to ask what they owe, whether a transfer arrived, or whether a receipt was issued. Staff then search ledgers, match transactions to students by hand, update records, and answer the same questions again.

Failure modes in the current process:

- Students with the same name get mixed up.
- Bank transfers arrive without a reference, so staff cannot match them.
- Ledger updates lag behind payments, so parents see old balances.
- Parents act on partial information, pay twice, or believe a debt is cleared when it is not.
- Staff spend most of their time answering routine questions.

Before the pilot, the bursary logs every fee call and routine query for two weeks. These logs set the baseline for goals G3 and G4. The frequency of these failures is unknown until the baseline is measured.

The core problem: the school has no reliable, visible link between its approved ledger and the information parents can see.

---

## 3. Goals and Non-Goals

### 3.1 Goals

| ID | Goal | Measurable target (pilot) |
|---|---|---|
| G1 | Show accurate balances | 100% of balances in the automated test set match the approved ledger. Full-ledger check at term end matches 100%. |
| G2 | Never claim a receipt that does not exist | 0 false receipt claims in the pilot. |
| G3 | Let parents answer routine fee questions alone | Hypothesis: at least 80% of active pilot parents answer the three main fee questions without calling the bursary by end of term. If below 60%, the team reviews portal copy and setup. [ASSUMPTION] |
| G4 | Reduce staff time on routine questions | At least 50% reduction in median staff minutes per routine query, against the week 1 baseline. [ASSUMPTION] |
| G5 | Show gateway results in the portal quickly | 95% of gateway SUCCESSFUL states appear in the portal within 2 minutes of gateway confirmation. The webhook starts verification at once. A one-minute job acts as backup (Section 8.5). |
| G6 | Protect student records | 0 unauthorized disclosures in pre-launch access testing and in the pilot. |
| G7 | Work on low-end phones and weak connections | Balance screen shows its first content within 3 seconds on a simulated slow 3G profile. [ASSUMPTION] |

### 3.2 Non-Goals

| ID | Non-goal | Source |
|---|---|---|
| NG1 | Automated payment reminders or follow-up messages | Concept, "Do not build yet" |
| NG2 | AI agents that act for parents | Concept, "Do not build yet" |
| NG3 | Automatic waivers, reversals, refunds, or balance changes | Concept, "must never" list |
| NG4 | Predictions about which parents will pay late | Concept, "Do not build yet" |
| NG5 | Search across schools or across private records | Concept, "must never" list |
| NG6 | A general assistant that answers from internet sources | Concept, "Do not build yet" |
| NG7 | Analytics dashboards beyond what the pilot school uses | Concept, "Do not build yet" |
| NG8 | Native iOS or Android apps. Version one is a web application only. | [ASSUMPTION] |

---

## 4. Users and Personas

### 4.1 Persona 1: Parent with multiple children

| Attribute | Detail |
|---|---|
| Role | Parent or guardian of two or more students at the pilot school |
| Goals | Know what each child owes. Pay the right amount once. Confirm that a payment counts. |
| Pain points | Calls to the bursary go unanswered. Transfers arrive without a clear reference. Old balances show up in messages. Uncertainty about whether a receipt exists. |
| Device and connectivity | Low-cost Android phone, often 2 to 4 GB RAM [ASSUMPTION]. Limited mobile data. Intermittent connection. Some users share a phone. |
| Permissions | Views only approved children. Starts payments and declares transfers for approved children. Submits questions. Cannot change any fee, balance, or receipt. |
| Top three tasks | (1) Check the balance for each child. (2) Pay outstanding fees by card or declare a bank transfer. (3) Check whether a payment is applied and receipted. |

### 4.2 Persona 2: Bursary staff member

| Attribute | Detail |
|---|---|
| Role | Bursar or finance officer |
| Goals | Keep ledgers accurate. Match payments quickly. Answer escalated questions within a day. |
| Pain points | Imports from spreadsheets with inconsistent formats. Duplicate student names. Bank transfers without references. Repeated routine questions. |
| Device and connectivity | Office desktop or laptop, usually on a stable connection. Some work on phones. |
| Permissions | Imports and drafts records. Matches and applies payments. Resolves escalation tickets. Requests ledger corrections. Cannot approve own requests. Cannot publish fee rules alone. |
| Top three tasks | (1) Import a fee schedule and student list. (2) Match a bank transfer or settlement line to a payment. (3) Resolve an escalation ticket with an internal note. |

### 4.3 Persona 3: School owner or authorized approver

| Attribute | Detail |
|---|---|
| Role | Proprietor, principal, or designated approver |
| Goals | Control the school's financial data. Approve fee rules and sensitive changes. See who changed what. |
| Pain points | Unclear accountability for changes. Risk of unauthorized access. Little visibility into payment status. |
| Device and connectivity | Mobile phone for approvals, plus occasional desktop use. Connection varies. |
| Permissions | Approves sensitive changes (Section 6.5). Reads the audit log. Manages staff roles. Revokes sessions. |
| Top three tasks | (1) Approve a published fee schedule. (2) Review a ledger correction and its approval trail. (3) Revoke a former staff member's access. |

---

## 5. Scope

### 5.1 In scope for version one

| Area | Included items |
|---|---|
| Parent portal, five features | My children's fees; payment status and receipts; pay school fees; previous term records; ask the school |
| Staff administration | Record import; reconciliation; apply and receipt; draft and review; approval and audit; account and access management; clearance records |
| Payments | Card payments through one configured gateway. Bank transfers declared by parents and matched by staff. Gateway settlement reports imported for card reconciliation. |
| Access | Parent sign-in by one-time code (no password). Approved guardian links by link code. Staff sign-in with password and TOTP for OWNER, BURSAR, and any role holding APPROVE_FINANCE. Session revocation. |
| Shared records | Term dates, payment policies, and approved contacts. Publication with approval. Meaning-based search over published records. Fee amounts stay in the fee schedule. |
| Setup | Eight-step day-one setup wizard with completion gate |
| Retention | Withdrawal handling, financial history preservation, export for owners |

### 5.2 Out of scope for version one

- Automated messages of any kind to parents (reminders, receipts by email, follow-ups).
- AI-generated answers about balances, payments, receipts, or clearance.
- Multiple payment gateways active at once.
- Multiple schools in one pilot. The pilot uses one school.
- Sponsor accounts with access to a child's financial history.
- Refund and reversal requests from parents.
- Offline payment initiation or offline balance display.
- Native mobile applications.

### 5.3 Scope boundaries (edge cases)

| ID | Boundary | Resolution |
|---|---|---|
| SB-1 | Partial payments | Allowed only if the school enables them in setup and sets a minimum. Off by default. Open Question OQ-09. |
| SB-2 | One payment covering two children | Not allowed. Each payment belongs to one student. [ASSUMPTION] |
| SB-3 | Payment for a term the parent cannot see | Blocked at initiation. Server checks the term is active and published. |
| SB-4 | Payment after a child withdraws | Blocked from the withdrawal effective date. Staff handle any amount owed through the bursary. |
| SB-5 | Parent asks about a previous session | Routed to Previous term records. If not verified, shows "Record not available" and the named contact. |
| SB-6 | Parent asks an open question about a payment | Routed to Ask the school. No automated answer. |
| SB-7 | Parent declares a bank transfer | Parent can withdraw the declaration only while it is unreconciled. Once matched, staff handle any correction. |

---

## 6. Functional Requirements

Each requirement is one testable statement. The Source column names the concept section or a flagged assumption.

### 6.1 Parent Portal

| ID | Requirement | Source |
|---|---|---|
| FR-001 | The system shows only students with an APPROVED guardian link to the signed-in parent account. | Concept §8 |
| FR-002 | The balance screen shows, for each child and the current term, every fee item, charge, discount, payment, and adjustment, plus the outstanding balance. | Concept Feature 1 |
| FR-003 | The balance screen shows a combined total equal to the sum of the displayed child balances. | Concept Feature 1 |
| FR-004 | Every amount on the balance screen shows its term, entry date, and status. | Concept Feature 1 |
| FR-005 | The server computes each balance as the sum of LedgerEntry amounts for that student and term. The client does not compute balances. | Concept §11 |
| FR-006 | The balance screen shows a "last verified" timestamp taken from the most recent applied ledger entry. | Concept §5, Risk 3 |
| FR-007 | The payment status list shows, for each payment: amount, date, method, transaction or transfer reference, gateway status (card only), match status, and receipt status. | Concept Feature 2 |
| FR-008 | The receipt number appears only when receiptStatus equals ISSUED. | Concept Feature 2 |
| FR-009 | The pay screen shows the approved outstanding amount from the server, the payment paths the school enabled, and any gateway fee before payment. For bank transfer, it shows the school account details and the payment reference to quote. | Concept Feature 3, §8.6 |
| FR-010 | The pay action is disabled when the outstanding balance is zero, when no verified ledger exists, or when any open payment exists for the student. A payment is open when its gatewayStatus is not FAILED or ABANDONED, its reconciliationStatus is not REJECTED, and appliedAt is empty. | Concept §7, §9, Risk 2 |
| FR-011 | The previous term screen shows a past term only when AcademicTerm.historicalVerifiedAt is set. | Concept Feature 4 |
| FR-012 | When a past term is unverified, the previous term screen shows "This record is not available yet" and the named bursary contact. | Concept Feature 4 |
| FR-013 | The ask the school form accepts one category from the EscalationCategory list in Section 9 and a parent message of up to 500 characters. | Concept Feature 5 |
| FR-014 | On submission, the system creates an EscalationTicket with a unique reference code and shows that code, the category, the named bursary contact, and the status. | Concept Feature 5 |
| FR-015 | The parent sees the status and reference code of their own tickets. The parent never sees the internal note field. | Concept §10 |
| FR-016 | The ticket form does not accept or display any balance or payment figure as an answer. | Concept §10 |
| FR-017 | The parent sees "Cleared" for a term only when a ClearanceRecord with status CLEARED exists for that student and term and is not superseded. A DISPUTED record shows "Under review by the school." Any other case shows "Clearance not recorded." | Concept §10, Feature 4 |

### 6.2 Payments

| ID | Requirement | Source |
|---|---|---|
| FR-020 | A card payment creates a Payment with method CARD, gatewayStatus INITIATED, and a unique paymentReference before the parent leaves the portal. | Concept §7 step 1 |
| FR-021 | The server rejects a payment whose amount is greater than the outstanding balance at that moment. | Concept §7, §10 |
| FR-022 | The server sets the payment amount. The client sends only the student ID and, if SB-1 applies, the chosen partial amount. | Concept §11 |
| FR-023 | The server sets gatewayStatus to SUCCESSFUL only after a server-side call to the gateway confirms the transaction. | Concept §7 step 2 |
| FR-024 | A redirect back to the portal triggers a verification call. A redirect alone never sets SUCCESSFUL. | Concept §7 step 2 |
| FR-025 | The webhook endpoint rejects any request whose signature does not verify. Rejected requests change no state. | [ASSUMPTION] standard practice |
| FR-026 | A webhook event already stored (matched by providerEventId) causes no state change and no duplicate PaymentEvent. | Concept Risk 2 |
| FR-027 | A repeated initiation with the same Idempotency-Key from the same parent returns the original Payment. The same key with different parameters returns HTTP 409. No second charge is created. | Concept §7 |
| FR-028 | A card payment with gatewayStatus SUCCESSFUL and reconciliationStatus UNRECONCILED displays "Received. Waiting for school confirmation." It never displays "Paid." | Concept §7 |
| FR-029 | A ledger credit posts only in the apply action defined in FR-030. Matching a payment never posts a credit. | Concept §7 steps 3 and 4 |
| FR-030 | Staff apply a payment only when reconciliationStatus is MATCHED. Apply runs in one transaction that creates the PAYMENT LedgerEntry, the Receipt, sets appliedAt, sets receiptStatus to ISSUED, and writes the AuditLog row. Bulk apply runs one transaction per payment. | Concept §7 step 4 |
| FR-031 | A gateway failure leaves the ledger unchanged, displays "Payment not completed. Your fee balance has not changed.", and allows a new initiation with a new reference. | Concept §7 |
| FR-032 | A card payment in INITIATED or PENDING for more than 30 minutes triggers the verification job. ABANDONED is set only when the gateway confirms no successful charge. If the gateway gives no final status, the payment stays PENDING. The job retries every 15 minutes for 24 hours. After 24 hours it creates a staff review item with the note "gateway status unknown." | [ASSUMPTION] 30-minute window |
| FR-033 | Staff can set reconciliationStatus to DISPUTED. The parent sees "Under review by the school." | Concept §10 |
| FR-034 | The portal shows no refund, reversal, or cancellation control to any parent. No parent endpoint writes to LedgerEntry, FeeItem, LedgerCorrection, ClearanceRecord, or Receipt. | Concept §10 |
| FR-035 | A payment applies to the ledger at most once. LedgerEntry.paymentId is unique. | Concept Risk 2 |
| FR-036 | A parent declares a bank transfer by entering the amount and the payment reference shown on the pay screen. The declaration creates a Payment with method BANK_TRANSFER and gatewayStatus NOT_APPLICABLE. The parent can withdraw the declaration only while reconciliationStatus is UNRECONCILED. | [ASSUMPTION] Option B, Section 8.6 |
| FR-037 | Bank transfer payments never display gateway status. They show "Transfer reported. Waiting for the school to find it in the bank statement." until matched. | [ASSUMPTION] Option B |
| FR-038 | A second SUCCESSFUL card payment or declared transfer for the same student and amount within 24 hours sets reconciliationStatus to DISCREPANCY with the note "possible duplicate." Staff resolve it. | Concept Risk 2 |

### 6.3 Access and Identity

| ID | Requirement | Source |
|---|---|---|
| FR-040 | A parent signs in and verifies a phone number or email address with a one-time code. No parent password exists. | Concept §8, OQ-03 |
| FR-041 | A parent requests a guardian link by entering the student's admission number and a single-use link code issued by the school. Link codes expire after 7 days. | [ASSUMPTION] |
| FR-042 | A new guardian link starts as PENDING. Staff approve it. A parent cannot approve their own link. | Concept §8 |
| FR-043 | A student's name, class, or admission number alone never grants access. | Concept §8 |
| FR-044 | Each private-record endpoint checks: valid session, parent account, APPROVED link, student ID, and school ID. Any failed check returns "not found." | Concept §6 privacy rule |
| FR-045 | A mismatch between the requested student ID and the parent's approved links returns HTTP 404, not 403. | [ASSUMPTION] |
| FR-046 | The parent can view and revoke their own active sessions. | Concept §8 |
| FR-047 | Staff with OWNER or BURSAR role can revoke all sessions for a parent account. | Concept §8 |
| FR-048 | Account recovery uses a new one-time code to a verified phone or email. A successful recovery revokes all existing parent sessions. | Concept §8 |
| FR-049 | A one-time code expires after 10 minutes and accepts 5 wrong entries. After 5 wrong entries, the code is void. Five void codes for one destination within 30 minutes block new codes for that destination for 30 minutes. A destination receives at most 3 codes in 15 minutes. | [ASSUMPTION] |
| FR-050 | Staff accounts with OWNER or BURSAR role, or any role holding APPROVE_FINANCE, require TOTP as a second factor. | [ASSUMPTION] |
| FR-051 | Sessions expire after 14 days idle or 30 days absolute, whichever comes first. | [ASSUMPTION] |
| FR-052 | A sponsor or any person without an approved guardian link receives no private records. Sponsor access is not in version one. | Concept §8 |

### 6.4 Staff Administration

| ID | Requirement | Source |
|---|---|---|
| FR-060 | The import screen accepts CSV and XLSX files that match a template version in Section 6.14. | Concept §5 |
| FR-061 | An import with a missing required column is rejected in full. The system names the missing column. | Concept §6 |
| FR-062 | Import validation rejects duplicate admission numbers within a school, non-numeric or negative amounts, unknown term labels, unknown fee item codes, and duplicate rows. | Concept §6 |
| FR-063 | An import creates an ImportBatch in DRAFT. Validation moves it to VALIDATED or FAILED. Draft data is not visible to parents. | Concept §6 |
| FR-064 | The system flags students who share the same name and class and requires an admission number to link any guardian to them. | Concept §9 step 3 |
| FR-065 | Manual entry uses the same validation as import. | Concept §5 |
| FR-066 | The reconciliation screen lists unmatched statement lines and payments that are open and unreconciled. | Concept §5 |
| FR-067 | Staff match a payment to a statement line only when the amounts are equal. Unequal amounts create a DISCREPANCY record. | Concept §7 |
| FR-068 | Staff can match a payment only to the student named on that payment. Changing the student requires a LedgerCorrection. | Concept §6 |
| FR-069 | Staff can add a note to an escalation ticket, change its status, and resolve it. Notes are visible to staff only. | Concept §5 |
| FR-070 | Staff permissions are set by role. Each role lists explicit permission codes from Section 9 (Permission model). | Concept §5 |
| FR-071 | Ledger rows from an ImportBatch post only after a user holding APPROVE_FINANCE, who did not create the batch, sets it to APPROVED. Posting creates LedgerEntry rows that carry the importBatchId. | Concept §6, Setup step 8 |
| FR-072 | A manual ledger entry follows the same approval rule as FR-071. The approver differs from the creator. | Concept §6 |
| FR-073 | A ClearanceRecord is entered by staff holding APPROVE_FINANCE. The approver differs from the creator. A new record supersedes the old one; the old record is never edited. | Concept §10 |

### 6.5 Approval and Audit

| ID | Requirement | Source |
|---|---|---|
| FR-080 | Sensitive changes need approval before they take effect. Sensitive changes are: publishing a FeeSchedule, publishing a SharedRecord, approving an ImportBatch, creating a LedgerCorrection, creating or superseding a ClearanceRecord, and changing the school settlement account. The approver is the OWNER or a user holding APPROVE_FINANCE who did not make the request. | Concept §5, §6 |
| FR-081 | Publication records effectiveFrom, approvedById, and publishedAt. | Concept §6 |
| FR-082 | A correction creates a LedgerCorrection with old value, new value, reason, requester, and status PENDING. | Concept §6 |
| FR-083 | A correction takes effect only when approved. Approval creates a reversing LedgerEntry. The original entry is never edited. | Concept §6 |
| FR-084 | The system blocks a user from approving their own sensitive change. | Concept §5 |
| FR-085 | Every write to a financial, access, publication, or setup table also writes an AuditLog row in the same transaction. | Concept §5 |
| FR-086 | The owner can view the audit log, filter it by student, actor, entity, and date, and export it as CSV. | Concept §5 |
| FR-087 | The application cannot update or delete AuditLog rows. | Concept §5 |

### 6.6 Shared Records

| ID | Requirement | Source |
|---|---|---|
| FR-090 | Staff create a shared record as DRAFT. The database requires a title, body, record type, and effectiveFrom before the status becomes SUBMITTED. | Concept §6 |
| FR-091 | Publishing a new version runs in this order. (1) The system embeds the new version with its chunks marked not current. The publish action stays disabled until embedding succeeds. (2) In one transaction, the system deletes the previous version's chunks, marks the new chunks current, sets the previous record to SUPERSEDED, and sets the new record to PUBLISHED. | Section 10.8 |
| FR-092 | Retracting a version runs in one transaction. It sets the record to RETRACTED and deletes its chunks. | Section 10.9 |
| FR-093 | Parents can browse published shared records by type. | Concept §6 |
| FR-094 | Parents can search published shared records by meaning. Each result shows the record title, version, and effective date. | Concept §6 |
| FR-095 | A search with no result above the similarity threshold returns "No approved record matches. Contact [named bursary contact]." | Section 7.4 |
| FR-096 | A shared record body must not contain currency amounts. The approver checks this before publication. Fee amounts come only from the FeeSchedule. | Concept §6, §10 |

### 6.7 Setup

| ID | Requirement | Source |
|---|---|---|
| FR-100 | The setup wizard has eight steps, matching Concept §9. Each step writes a SetupStep row and can be resumed. | Concept §9 |
| FR-101 | Parents see "Setup is not complete" until the owner marks setup complete. | Concept §9 |
| FR-102 | Setup completion requires: an approved opening ledger ImportBatch, a named bursary contact, a completed gateway test transaction (if card is enabled), and at least one sample parent account tested. | Concept §9 |
| FR-103 | Setup completion records the approver and the timestamp on School.setupCompletedAt. | Concept §9 |

### 6.8 Notifications

| ID | Requirement | Source |
|---|---|---|
| FR-110 | Version one sends no payment reminders, receipts by message, or follow-ups. | Concept §10 |
| FR-111 | Version one sends only one-time codes for sign-in, verification, and recovery. | Section 6.3 |
| FR-112 | Ticket assignment and status changes are visible in the staff app only. No message goes to the parent. | Concept §10 |

### 6.9 Data Retention

| ID | Requirement | Source |
|---|---|---|
| FR-120 | Withdrawing a student sets status WITHDRAWN and withdrawalEffectiveOn. | Concept §8 |
| FR-121 | Withdrawal revokes every APPROVED guardian link for that student as of the effective date. | Concept §8 |
| FR-122 | No financial record (ledger entries, payments, receipts, corrections, reconciliations, clearance records) is deleted when a student withdraws. | Concept §8 |
| FR-123 | Authorized staff can retrieve a withdrawn student's history. Parents cannot. | Concept §8 |
| FR-124 | The retention period for financial records is set by the school. The system default is 6 years [VERIFY BEFORE LAUNCH]. | Concept §13 |
| FR-125 | Escalation messages entered by parents are deleted or redacted after the school-defined period. Financial records are not affected. | Concept §13 |
| FR-126 | The owner can export a term's ledger as CSV. | [ASSUMPTION] |

### 6.10 Payment States

The concept requires distinct states. The table shows how the database and the screen represent each one.

| Stage | Database condition | Parent-facing text | Next allowed stage |
|---|---|---|---|
| Card initiated | gatewayStatus = INITIATED or PENDING | "Payment in progress." | Gateway successful, failed, or abandoned |
| Transfer declared | method = BANK_TRANSFER, gatewayStatus = NOT_APPLICABLE, reconciliationStatus = UNRECONCILED | "Transfer reported. Waiting for the school to find it in the bank statement." | Matched, discrepancy, rejected, or withdrawn by parent |
| Gateway confirmed | gatewayStatus = SUCCESSFUL, reconciliationStatus = UNRECONCILED | "Received. Waiting for school confirmation." | Matched, discrepancy, or disputed |
| Matched | reconciliationStatus = MATCHED, appliedAt empty | "Confirmed by the school. Receipt not yet issued." | Applied |
| Applied and receipted | appliedAt set, receiptStatus = ISSUED | "Receipt [number] issued on [date]." | Voided (staff only) |
| Failed | gatewayStatus = FAILED | "Payment not completed. Your fee balance has not changed." | New initiation |
| Abandoned | gatewayStatus = ABANDONED | "Payment not completed. Your fee balance has not changed." | New initiation |
| Rejected | reconciliationStatus = REJECTED | "The school could not match this transfer. Contact [bursary contact]." | New declaration |
| Disputed | reconciliationStatus = DISPUTED | "Under review by the school." | Staff resolution |

**Acceptance tests for these states:**

- A payment with gatewayStatus SUCCESSFUL and no appliedAt never displays "Paid."
- A payment with receiptStatus NOT_ISSUED never displays a receipt number.
- A retry after FAILED creates a new paymentReference and does not change the balance.
- A duplicate webhook for the same providerEventId leaves the payment row and its events unchanged.
- While any open payment exists, the pay action is disabled (FR-010).
- A matched payment does not change the balance until it is applied.

### 6.11 Acceptance Criteria for Access

| Test | Expected result |
|---|---|
| Parent A requests the balance for a student linked to Parent B | HTTP 404, no data in the response body |
| Parent A requests a student with a REVOKED link | HTTP 404 |
| Parent A requests a student at another school | HTTP 404 |
| Staff A approves a guardian link they requested | Rejected, error logged |
| Former parent signs in after withdrawal effective date | No history visible for that student |
| Revoked session token used | HTTP 401 |
| Five wrong codes for one code | Code void. Sign-in requires a new code. |
| Name and class match but no approved link | HTTP 404 |
| Parent calls any endpoint that writes ledger, fee, correction, clearance, or receipt data | Endpoint not available |
| Approver approves their own ImportBatch | Rejected, error logged |

### 6.12 Error and Empty States by Feature

| Feature | Error state | Empty state |
|---|---|---|
| My children's fees | "We could not load the balance. Try again." with retry button and the bursary contact. Never show zero. | "Fees for this term are not published yet." with the bursary contact. |
| Payment status | "We could not load payments. Try again." | "No payments yet for this student." |
| Pay school fees | "Payment could not start. Your fee balance has not changed. Try again." | "No outstanding balance." No pay button. |
| Previous term records | "We could not load this record. Try again." | "This record is not available yet." with contact. |
| Ask the school | "Your message was not sent. Your text is saved. Try again." | "You have no open questions." |
| Shared record search | "Search is unavailable. Browse records by type instead." | "No approved record matches. Contact [name]." |
| Setup status | "Setup status could not be checked. Try again." | Not applicable. Parents see the setup message. |

On connection failure, the parent sees a retry button and the message "Not updated. Check your connection." No financial figure stays on the screen as if it were current.

### 6.13 Requirements Validation

Each "must never" rule in the concept maps to a requirement:

- Invent a balance or receipt: FR-005, FR-008, FR-012, FR-101.
- Show one family's records to another: FR-001, FR-044, FR-045.
- Let a parent change charges or clear a debt: FR-034.
- Claim a gateway transaction is a receipt: FR-008, FR-029, FR-030.
- Decide disputes, waivers, or refunds: FR-033, FR-034, FR-080.
- Promise clearance without a record: FR-017.
- Search private records by meaning: Section 10.1, FR-094.
- Send automated reminders: FR-110.

### 6.14 Import Templates

Each import uses a template version. Column names are fixed. Unknown columns cause rejection (FR-061).

| Import kind | Template version | Required columns |
|---|---|---|
| STUDENTS | 1 | admission_number, first_name, last_name, class_label |
| FEE_SCHEDULE | 1 | term_label, fee_item_code, fee_item_name, class_label, amount_minor, currency |
| LEDGER | 1 | admission_number, term_label, fee_item_code, entry_type, amount_minor, currency, occurred_on |
| HISTORY | 1 | admission_number, term_label, fee_item_code, entry_type, amount_minor, currency, occurred_on, source_reference |
| STATEMENT | 1 | value_date, amount_minor, narration, bank_reference, account_last4 [VERIFY per bank] |
| SETTLEMENT | 1 | external_reference, amount_minor, currency, settled_on [VERIFY with chosen gateway] |

amount_minor holds an integer in kobo. entry_type must match a LedgerEntryType value. occurred_on uses ISO 8601 dates.

---

## 7. AI and AI-Related Tools and Solutions

### 7.1 Where AI Appears

AI appears in one place in version one: meaning-based search over published shared records. No other feature uses a language model or an embedding model.

| Feature | AI use | Allowed? |
|---|---|---|
| Shared record search | Semantic retrieval of published, approved text | Yes |
| Balance and totals | None | No |
| Payment confirmation | None | No |
| Receipt and clearance status | None | No |
| Historical records | None | No |
| Ask the school | None. Routing is rule-based. | No |
| Staff reconciliation | None. Matching uses exact rules (Section 8.7). | No |

### 7.2 Retrieval Pipeline

1. The parent selects a school and submits a query. The server checks the session and confirms the school from the approved guardian context.
2. The server applies the routing rules in 7.2.1. A routed query never reaches search.
3. The server scrubs the query (Section 7.2.2) and sends the scrubbed text to the embedding model. The model returns a 1,536-dimension vector [ASSUMPTION, Section 10.3].
4. The server runs the query in Section 11.4 with mandatory filters for school ID, is_current, and the effective window. The filters apply before ordering.
5. The server keeps up to 5 results whose cosine distance is at or below the threshold in Section 10.7.
6. The server returns the chunk text, record title, version, and effective date for each kept result. No generated summary is included in version one.
7. If no result passes the threshold, the server returns the refusal in Section 7.4.

#### 7.2.1 Routing rules

Routing checks the query in this order.

- **Tier A, amount words.** If the query contains any of: how much, fee, fees, owe, owing, balance, outstanding, amount, cost. The server routes to My children's fees or the fee schedule screen. No search runs.
- **Tier B, personal words with payment or history words.** If the query contains a personal word (my, mine, I, me, my child) and any of: pay, paid, payment, transfer, receipt, refund, confirmed, cleared, clearance, last term, last session, previous term, past term. The server routes as follows. Refund words route to Ask the school. Payment and receipt words route to Payment status. History words route to Previous term records. Clearance words route to Payment status with the clearance line.
- **Tier C, everything else.** The query goes to search.

Tier A and Tier B are deliberately broad. A question that a routing rule catches still gets a correct answer, because the parent sees their own records on the right screen.

#### 7.2.2 Query scrubbing

Before the embedding call, the server removes:

- Every digit sequence.
- Every word that matches the first or last name of a child linked to the parent.

Only the scrubbed text leaves the system. The original query is not stored. Only a hash of the original query is logged (Section 7.4).

### 7.3 Grounding Rule

The search response shows approved text exactly as published, with its source. The parent reads the source. The system does not paraphrase or combine results. This removes any path where a model could invent a fee or a date.

### 7.4 Refusal and Fallback

- When no result passes the threshold, the response reads: "No approved record matches your question. Contact [bursary contact name] at [contact channel]."
- A routed query returns a link to the relevant screen and no search results.
- The fallback never reveals internal notes, other parents' details, or staff-only records.
- Refusals log a hash of the query, the school ID, and a timestamp. Raw query text is not logged [ASSUMPTION].

### 7.5 Forbidden AI Uses

AI must not:

- Calculate or estimate any balance, total, or discount.
- Decide whether a payment is confirmed, matched, applied, failed, or abandoned.
- Search private records, payments, ledgers, receipts, corrections, clearance records, or escalation notes.
- Reconstruct a historical term from any source.
- Answer questions about clearance status.
- Decide, recommend, or grant a waiver, refund, reversal, or payment plan.
- Send or schedule any message to a parent.
- Predict payment behavior.
- Answer from sources outside the school's published records.
- Receive unscrubbed parent query text. The scrub in 7.2.2 always runs first.

### 7.6 Evaluation Plan

Two sets test the feature.

**Set A: scripted regression set (14 queries).** This set must pass 14 of 14 before each release.

| # | Query | Expected result |
|---|---|---|
| 1 | "When does second term start?" | Returns the TERM_DATE record for the current session, with effective date. |
| 2 | "Do you accept bank transfer?" | Returns the PAYMENT_POLICY record that describes transfer methods. |
| 3 | "What is the fee for JSS1?" | Tier A. Routes to the fee schedule screen. No search result. |
| 4 | "Who is the bursar?" | Returns the CONTACT record with the approved name. |
| 5 | "scond trm strt" (typo) | Returns the second term date record. |
| 6 | "when school dey resume" (Nigerian Pidgin) | Returns the term date record if the embedding matches it. Otherwise returns the refusal. Logged for review. |
| 7 | "What is my balance?" | Tier A. Routes to My children's fees. |
| 8 | "Is my payment confirmed?" | Tier B. Routes to Payment status. |
| 9 | "Can I get a refund?" | Tier B. Routes to Ask the school with the bursary contact. |
| 10 | "Late payment penalty?" | Tier C. Search returns no approved record. Refusal with bursary contact. |
| 11 | A query that matches a RETRACTED record | No result from that record. Refusal if no other match. |
| 12 | A query that matches another school's record | No result. Refusal. |
| 13 | "What did my child pay last session?" | Tier B. Routes to Previous term records. |
| 14 | "Write me a poem about school" | Tier C. Search returns no result above threshold. Refusal. |

**Set B: real bursary questions (100 queries).** The team collects 100 real questions from the pilot bursary before launch. The team labels each with the expected route or record. The pass target is 95% or higher. This set calibrates the threshold in Section 10.7.

**Launch gate:** Set A passes 14 of 14. Set B reaches 95% or higher. The data processing agreement for the embedding provider is signed (OQ-20).

### 7.7 Monitoring

The team tracks the refusal rate on shared-record queries each week. A rise above 30% in any week triggers a review of the threshold and the published records [ASSUMPTION].

---

## 8. Technical Architecture

### 8.1 Components

| Component | Responsibility | Talks to |
|---|---|---|
| Parent client (browser) | Renders screens, sends requests, retries failed calls. Stores no financial data. | Next.js server |
| Staff client (browser) | Administration screens | Next.js server |
| Next.js server (App Router) | Server components, server actions, route handlers, webhook endpoint, authorization checks | Prisma, payment gateway, OTP provider, embedding provider |
| Prisma data layer | All typed database access and migrations for non-vector tables. Helpers always apply the school ID. | PostgreSQL |
| PostgreSQL | Primary store for records, audit log, sessions, OTP codes, and the job table | Prisma, pgvector (same instance) |
| pgvector extension | Vector storage and exact similarity search for published shared records only | PostgreSQL |
| Payment gateway | Card processing, transaction status, webhooks, settlement reports | Next.js server |
| Statement input | Staff upload of bank statement files and gateway settlement reports | Next.js server |
| Job runner | Runs verification, webhook processing, embedding, and retraction jobs from the Job table | PostgreSQL, Next.js server |
| OTP provider | Sends one-time codes by SMS or email. Nothing else. [OQ-15] | Next.js server |
| Embedding provider | Returns vectors for scrubbed query text and shared record chunks | Next.js server |

[ASSUMPTION] The job runner uses a PostgreSQL job table. A webhook starts its job at once. A scheduled route polls every minute as a backup. This avoids a separate queue service.

### 8.2 Request Flows

**My children's fees**

1. The client calls the balance server action with a student ID.
2. The server checks the session and the approved link (Section 8.4).
3. The server sums LedgerEntry.amountMinor for the student and the current term.
4. The server returns entries, the balance, and the last-verified timestamp.
5. The client displays the result. On failure, the client shows the error state in Section 6.12.

**Payment status and receipts**

1. The client requests the payment list for an approved student.
2. The server returns payments with method, gatewayStatus (card only), reconciliationStatus, appliedAt, and receiptStatus.
3. The client displays each state using the text in Section 6.10.

**Pay by card**

1. The client shows the outstanding balance and the payment options from the server.
2. The parent selects card and confirms.
3. The client sends the request with an Idempotency-Key.
4. The server checks FR-010, creates a Payment in INITIATED, and returns the gateway checkout address.
5. The parent pays at the gateway. The gateway sends a webhook. The parent returns to the portal.
6. The server runs verification (Section 8.5).

**Declare a bank transfer**

1. The client shows the school account details and the payment reference.
2. The parent enters the amount and confirms the declaration.
3. The server creates a Payment with method BANK_TRANSFER and gatewayStatus NOT_APPLICABLE.
4. Staff match the transfer to a statement line (Section 8.7).

**Previous term records**

1. The client requests a past term for an approved student.
2. The server checks AcademicTerm.historicalVerifiedAt.
3. If set, the server returns the historical ledger, payments, corrections, receipts, and clearance records. If not set, it returns the unavailable response.

**Ask the school**

1. The parent selects a category from the list in Section 9 and writes a message of up to 500 characters. The parent may link the ticket to a payment.
2. The server validates the input and creates an EscalationTicket with a reference code.
3. The server assigns the ticket to the named bursary contact set in setup.
4. Staff see the ticket in the staff app. No message goes to the parent.

### 8.3 Authentication and Session Model

[ASSUMPTION] Sessions and one-time codes use Prisma tables. Auth.js is not used. The stack stays Next.js, TypeScript, Prisma, and PostgreSQL.

- **Parents.** A parent enters a phone number or email address. The OTP provider sends a six-digit code. The server stores only a hash of the code in OneTimeCode. The code follows the limits in FR-049. A correct code creates a Session.
- **Staff.** Staff sign in with a password and, for roles in FR-050, a TOTP code. Staff password hashes use a memory-hard algorithm such as Argon2id [VERIFY the library in use].
- **Session tokens.** Each token is 32 random bytes, encoded for a cookie. The cookie is HttpOnly, Secure, and SameSite=Lax. The database stores only a SHA-256 hash of the token.
- **Timeouts.** Sessions expire after 14 days idle or 30 days absolute (FR-051).
- **Revocation.** Setting revokedAt ends the session. Every request checks revokedAt. Revocation applies on the next request.
- **Recovery.** A successful recovery revokes all parent sessions (FR-048).

### 8.4 Authorization Model

Every private-record request runs these checks in order. Any failure returns HTTP 404.

1. The session exists, is not revoked, and is not expired.
2. The subject is a parent account (for parent endpoints) or an active staff user holding the required permission code (for staff endpoints).
3. For parent requests, a GuardianLink exists with parentAccountId equal to the session subject, studentId equal to the requested student, and status APPROVED.
4. The student's schoolId equals the schoolId on the approved link.
5. The record's schoolId equals the student's schoolId.
6. For staff requests, the permission code for the action is held through the staff user's role.

Permission codes: IMPORT_RECORDS, RECONCILE_PAYMENTS, APPLY_PAYMENTS, APPROVE_FINANCE, PUBLISH_SHARED, MANAGE_ACCESS, VIEW_AUDIT, RESOLVE_ESCALATION, MANAGE_SETUP.

A shared helper enforces these checks. Every private-record query includes the school ID and the student ID. Hiding a field in the client is not a control.

### 8.5 Payment Flow

**Card payments**

1. **Initiation.** The server checks FR-010 and FR-021. It writes a Payment with status INITIATED, a unique paymentReference (random, 16 characters), and the Idempotency-Key. The unique key on parentAccountId and idempotencyKey returns the same row for a repeated call. A reused key with different parameters returns HTTP 409 (FR-027).
2. **Gateway checkout.** The server creates a transaction with paymentReference as the merchant reference. It stores gatewayReference and sets gatewayStatus to PENDING.
3. **Webhook.** The gateway posts an event. The route reads the raw body, verifies the signature with the gateway's documented method [VERIFY BEFORE LAUNCH], and stores a PaymentEvent with providerEventId. A duplicate providerEventId is ignored. The route returns HTTP 200 and starts verification at once.
4. **Verification.** The job calls the gateway's status endpoint with gatewayReference. Only a confirmed successful status sets gatewayStatus to SUCCESSFUL and gatewayVerifiedAt to the current time. The webhook body alone never sets success.
5. **Redirect.** When the parent returns to the portal, the same verification runs. The return page never sets success.
6. **Retry safety.** A retry after FAILED or ABANDONED creates a new Payment with a new reference. A retry during PENDING returns the existing payment.
7. **Abandonment and unknown status.** After 30 minutes, verification runs (FR-032). A payment with no final gateway status stays PENDING and is retried every 15 minutes for 24 hours. Then a staff review item is created.
8. **Duplicate detection.** A second successful card payment or declared transfer for the same student and amount within 24 hours sets DISCREPANCY with the note "possible duplicate" (FR-038). Money is not applied twice.

**Bank transfers (declared)**

1. The parent declares the transfer (FR-036). The Payment has gatewayStatus NOT_APPLICABLE and reconciliationStatus UNRECONCILED.
2. The parent may withdraw the declaration while the status is UNRECONCILED.
3. Staff match the transfer to a statement line (Section 8.7). The match sets reconciliationStatus to MATCHED.
4. Staff apply the payment (FR-030).

### 8.6 Bank Transfer Approach

The concept leaves one choice open (OQ-01): a unique virtual account per payment, or the school's existing account with manual matching.

| Factor | Option A: Virtual account per payment | Option B: Existing account, declared and matched |
|---|---|---|
| How parent pays | Transfers to an account number generated for the payment | Transfers to the school account and quotes the payment reference |
| Matching | Automatic by account number | Staff match by reference and amount. Exact-reference matches are suggested automatically. |
| Gateway dependency | High. Needs gateway support and an approved account-issuance arrangement. | Low. Works with any bank that produces statements. |
| Settlement | Depends on merchant-of-record terms. Confirm with the gateway. | Goes directly to the school account. |
| Regulatory exposure | Issuing accounts may need approval [VERIFY BEFORE LAUNCH] | None specific to the portal |
| Parent error risk | Low, if the account is generated correctly | Medium. Parents may omit or mistype the reference. |
| Staff workload | Low | Medium. Exact-reference suggestions reduce it. |
| Fit for pilot | Lower, until gateway terms are confirmed | Higher |

**Recommendation for the pilot:** [ASSUMPTION] Option B. Parents declare transfers in the portal, quote the payment reference, and staff match them to statement lines.

**Concept amendment:** Option B changes concept Feature 3. Feature 3 says transfers go through the gateway. Under Option B, transfers go directly to the school account. [DECISION NEEDED] The owner signs off on this amendment (OQ-21) before the pilot.

**Auto-match rule for transfers:** a statement line suggests a match when the narration contains the paymentReference exactly and the amount equals the payment amount. Staff confirm the match. The suggestion does not change reconciliationStatus on its own.

Option A is evaluated for version two, after gateway terms and regulatory review.

### 8.7 Reconciliation

Matching uses two statement sources.

- **BANK_STATEMENT.** Bank lines from the school account statement. Used for declared transfers.
- **GATEWAY_SETTLEMENT.** Settlement lines from the gateway's report. Used for card payments.

Rules:

1. Staff upload each file. The system parses it, creates a StatementLine per row with status UNMATCHED, and rejects a duplicate externalReference for the same source and school.
2. **Card matching.** A settlement line matches a SUCCESSFUL card payment when the gatewayReference and the amount are equal. The match sets reconciliationStatus to MATCHED and records matchMethod AUTO_REFERENCE. It does not post a ledger credit.
3. **Transfer matching.** Staff match a bank line to a declared transfer. A suggestion appears when the narration contains the paymentReference exactly and the amounts are equal. Staff confirm. The confirmation records matchMethod MANUAL or AUTO_REFERENCE.
4. **Unequal amounts.** Staff create a DISCREPANCY record. The payment is not matched.
5. **One match rule.** One payment has at most one MATCHED reconciliation. One statement line has at most one MATCHED reconciliation (Section 9.2).
6. **Student rule.** Staff match a payment only to the student named on it (FR-068).
7. **Apply.** Staff apply matched payments, one at a time or in bulk. Apply runs the transaction in FR-030. The ledger credit and the receipt post together.
8. **Aging.** A bank line with no match after 30 days is flagged for owner review. A payment that stays SUCCESSFUL and unmatched for more than 3 business days appears on the owner's review list [ASSUMPTION].

### 8.8 Audit Logging

- Each write to a financial, access, publication, or setup table writes one AuditLog row in the same transaction. If the audit write fails, the business write rolls back.
- An AuditLog row stores actor type, actor ID, action, entity type, entity ID, before and after JSON with personal data minimized, IP hash, user agent, and request ID.
- The application role has INSERT and SELECT on AuditLog only. Triggers block UPDATE and DELETE.
- Authentication events and denied access attempts are logged with action ACCESS_DENIED or the event name.

### 8.9 Hosting, Backups, Environments, and Secrets

- **Hosting.** [ASSUMPTION] Next.js on a managed platform with server functions. PostgreSQL on a managed service that supports pgvector. [VERIFY BEFORE LAUNCH] that the chosen service supports pgvector and point-in-time recovery.
- **Data location.** [ASSUMPTION] Database region chosen to meet Nigerian data protection obligations (OQ-08).
- **Backups.** Daily full backup plus point-in-time recovery, kept 30 days [ASSUMPTION]. A restore test runs each quarter and the result is logged.
- **Environments.** Development uses fictional data only. Staging uses fictional data and gateway test mode. Production uses real data and live gateway only after launch approval. Production data never copies to other environments.
- **Secrets.** Gateway keys, database credentials, OTP provider keys, and session secrets live in the platform's secret store. They never appear in source control. Keys rotate every 90 days and after any suspected exposure.
- **Production access.** Named engineers only, with logged access. Staff data is not used for development.

### 8.10 Mobile Performance and Device Rules

| Rule | Limit or behavior | Status |
|---|---|---|
| Initial JavaScript for the parent portal | Under 150 KB compressed | [ASSUMPTION] |
| Balance screen page weight | Under 300 KB total, no images or video | Concept §11 |
| API response size per screen | Under 100 KB | [ASSUMPTION] |
| Request timeout | 15 seconds | [ASSUMPTION] |
| Retry behavior | Up to 3 retries with exponential backoff. Only idempotent requests retry automatically. | Concept §11 |
| Payment initiation | Requires a live connection. Not queued offline. | [ASSUMPTION] |
| Financial data on device | None stored. No offline balance cache. | Review finding |
| Shared-device sign-out | Parents can sign out of the current device. Sign-out clears local app state. | [ASSUMPTION] |
| Failed request | Shows the error state, "Not updated. Check your connection.", and a retry button. The typed message is kept in memory only. | Concept §11 |

---

## 9. Prisma Data Model

Conventions:

- UUID primary keys on all tables.
- schoolId on every tenant table. The foreign key to School is added in the migration SQL.
- ParentAccount is a global identity. It has no schoolId. Its links to students carry schoolId.
- Money is stored as BigInt in minor units (kobo for NGN). The API sends money as strings. [ASSUMPTION]
- Timestamps use timestamptz.
- Actor ID columns are plain columns. Foreign keys to staff and parent tables are added in the migration.
- Append-only tables: LedgerEntry, PaymentEvent, AuditLog, StatementLine. Triggers block UPDATE and DELETE.

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider  = "postgresql"
  url       = env("DATABASE_URL")
  directUrl = env("DIRECT_URL")
}

enum TermStatus {
  DRAFT
  ACTIVE
  CLOSED
}

enum StudentStatus {
  ACTIVE
  WITHDRAWN
  INACTIVE
}

enum GuardianLinkStatus {
  PENDING
  APPROVED
  REVOKED
}

enum PublicationStatus {
  DRAFT
  SUBMITTED
  PUBLISHED
  SUPERSEDED
  RETRACTED
}

enum SharedRecordType {
  TERM_DATE
  PAYMENT_POLICY
  CONTACT
}

enum LedgerEntryType {
  OPENING_BALANCE
  CHARGE
  DISCOUNT
  PAYMENT
  CORRECTION_REVERSAL
}

enum PaymentMethod {
  CARD
  BANK_TRANSFER
}

enum GatewayStatus {
  NOT_APPLICABLE
  INITIATED
  PENDING
  SUCCESSFUL
  FAILED
  ABANDONED
}

enum PaymentReconciliationStatus {
  UNRECONCILED
  MATCHED
  DISCREPANCY
  DISPUTED
  REJECTED
}

enum ReconciliationResult {
  MATCHED
  DISCREPANCY
  REJECTED
}

enum StatementLineStatus {
  UNMATCHED
  MATCHED
  DISCREPANCY
}

enum StatementSource {
  BANK_STATEMENT
  GATEWAY_SETTLEMENT
}

enum MatchMethod {
  AUTO_REFERENCE
  MANUAL
}

enum ReceiptStatus {
  NOT_ISSUED
  ISSUED
  VOIDED
}

enum CorrectionStatus {
  PENDING
  APPROVED
  REJECTED
}

enum ClearanceStatus {
  CLEARED
  NOT_CLEARED
  DISPUTED
}

enum EscalationStatus {
  OPEN
  ASSIGNED
  RESOLVED
  CLOSED
}

enum EscalationCategory {
  BALANCE_QUERY
  PAYMENT_NOT_SHOWING
  RECEIPT_REQUEST
  RECORD_MISSING
  ACCESS_PROBLEM
  OTHER
}

enum ConsentType {
  COMMUNICATION
  PREFERRED_LANGUAGE
  CONTACT_METHOD
}

enum ActorType {
  PARENT
  STAFF
  SYSTEM
  GATEWAY
}

enum SessionSubject {
  PARENT
  STAFF
}

enum JobStatus {
  QUEUED
  RUNNING
  SUCCEEDED
  FAILED
}

enum ImportKind {
  STUDENTS
  FEE_SCHEDULE
  LEDGER
  HISTORY
  STATEMENT
  SETTLEMENT
}

enum ImportStatus {
  DRAFT
  VALIDATED
  FAILED
  APPROVED
  POSTED
  REJECTED
}

enum OtpChannel {
  SMS
  EMAIL
}

enum OtpPurpose {
  SIGN_IN
  VERIFY
  RECOVERY
}

enum SetupStepStatus {
  PENDING
  DONE
}

model School {
  id               String    @id @default(uuid()) @db.Uuid
  name             String
  currency         String    @default("NGN") @db.VarChar(3)
  setupCompletedAt DateTime?
  createdAt        DateTime  @default(now())
  setupSteps       SetupStep[]
}

model SetupStep {
  id          String          @id @default(uuid()) @db.Uuid
  schoolId    String          @db.Uuid
  school      School          @relation(fields: [schoolId], references: [id], onDelete: Restrict)
  stepNumber  Int
  status      SetupStepStatus @default(PENDING)
  completedById String?       @db.Uuid
  completedAt DateTime?

  @@unique([schoolId, stepNumber])
}

model AcademicTerm {
  id                   String            @id @default(uuid()) @db.Uuid
  schoolId             String            @db.Uuid
  sessionLabel         String
  termName             String
  startDate            DateTime
  endDate              DateTime
  status               TermStatus        @default(DRAFT)
  historicalVerifiedAt DateTime?
  createdAt            DateTime          @default(now())
  feeSchedules         FeeSchedule[]
  ledgerEntries        LedgerEntry[]
  payments             Payment[]
  clearanceRecords     ClearanceRecord[]

  @@unique([schoolId, sessionLabel, termName])
  @@index([schoolId, status])
}

model StaffRole {
  id          String           @id @default(uuid()) @db.Uuid
  schoolId    String           @db.Uuid
  name        String
  staffUsers  StaffUser[]
  permissions RolePermission[]

  @@unique([schoolId, name])
}

model Permission {
  id    String           @id @default(uuid()) @db.Uuid
  code  String           @unique
  roles RolePermission[]
}

model RolePermission {
  roleId       String     @db.Uuid
  permissionId String     @db.Uuid
  role         StaffRole  @relation(fields: [roleId], references: [id], onDelete: Restrict)
  permission   Permission @relation(fields: [permissionId], references: [id], onDelete: Restrict)

  @@id([roleId, permissionId])
}

model StaffUser {
  id           String    @id @default(uuid()) @db.Uuid
  schoolId     String    @db.Uuid
  roleId       String    @db.Uuid
  role         StaffRole @relation(fields: [roleId], references: [id], onDelete: Restrict)
  email        String
  displayName  String
  passwordHash String
  mfaSecretRef String?
  disabledAt   DateTime?
  createdAt    DateTime  @default(now())

  @@unique([schoolId, email])
}

model ParentAccount {
  id               String         @id @default(uuid()) @db.Uuid
  email            String?        @unique
  phone            String?        @unique
  displayName      String
  lockedUntil      DateTime?
  disabledAt       DateTime?
  createdAt        DateTime       @default(now())
  guardianLinks    GuardianLink[]
  otpCodes         OneTimeCode[]
}

model OneTimeCode {
  id              String     @id @default(uuid()) @db.Uuid
  parentAccountId String?    @db.Uuid
  parent          ParentAccount? @relation(fields: [parentAccountId], references: [id], onDelete: Restrict)
  channel         OtpChannel
  destinationHash String
  purpose         OtpPurpose
  codeHash        String
  wrongAttempts   Int        @default(0)
  voidedAt        DateTime?
  expiresAt       DateTime
  consumedAt      DateTime?
  createdAt       DateTime   @default(now())

  @@index([destinationHash, purpose, createdAt])
}

model Session {
  id                String         @id @default(uuid()) @db.Uuid
  tokenHash         String         @unique
  subjectType       SessionSubject
  parentAccountId   String?        @db.Uuid
  staffUserId       String?        @db.Uuid
  schoolId          String?        @db.Uuid
  deviceLabel       String?
  ipHash            String?
  createdAt         DateTime       @default(now())
  lastSeenAt        DateTime       @default(now())
  idleExpiresAt     DateTime
  absoluteExpiresAt DateTime
  revokedAt         DateTime?

  @@index([parentAccountId, revokedAt])
  @@index([staffUserId, revokedAt])
}

model Student {
  id                    String          @id @default(uuid()) @db.Uuid
  schoolId              String          @db.Uuid
  admissionNumber       String
  firstName             String
  lastName              String
  classLabel            String
  status                StudentStatus   @default(ACTIVE)
  withdrawalEffectiveOn DateTime?
  withdrawnById         String?         @db.Uuid
  createdAt             DateTime        @default(now())
  guardianLinks         GuardianLink[]
  guardianLinkCodes     GuardianLinkCode[]
  ledgerEntries         LedgerEntry[]
  payments              Payment[]
  clearanceRecords      ClearanceRecord[]

  @@unique([schoolId, admissionNumber])
  @@index([schoolId, lastName, firstName])
}

model GuardianLinkCode {
  id          String   @id @default(uuid()) @db.Uuid
  schoolId    String   @db.Uuid
  studentId   String   @db.Uuid
  student     Student  @relation(fields: [studentId], references: [id], onDelete: Restrict)
  codeHash    String   @unique
  createdById String   @db.Uuid
  createdAt   DateTime @default(now())
  expiresAt   DateTime
  usedAt      DateTime?

  @@index([schoolId, studentId])
}

model GuardianLink {
  id              String             @id @default(uuid()) @db.Uuid
  schoolId        String             @db.Uuid
  parentAccountId String             @db.Uuid
  parent          ParentAccount      @relation(fields: [parentAccountId], references: [id], onDelete: Restrict)
  studentId       String             @db.Uuid
  student         Student            @relation(fields: [studentId], references: [id], onDelete: Restrict)
  relationship    String
  status          GuardianLinkStatus @default(PENDING)
  requestedAt     DateTime           @default(now())
  approvedById    String?            @db.Uuid
  approvedAt      DateTime?
  revokedById     String?            @db.Uuid
  revokedAt       DateTime?
  revokeReason    String?

  @@unique([parentAccountId, studentId])
  @@index([schoolId, studentId, status])
}

model ImportBatch {
  id              String       @id @default(uuid()) @db.Uuid
  schoolId        String       @db.Uuid
  kind            ImportKind
  fileName        String
  fileSha256      String
  templateVersion Int
  status          ImportStatus @default(DRAFT)
  rowCount        Int
  rejectedRowCount Int         @default(0)
  createdById     String       @db.Uuid
  approvedById    String?      @db.Uuid
  createdAt       DateTime     @default(now())
  approvedAt      DateTime?
  postedAt        DateTime?
  ledgerEntries   LedgerEntry[]
  statementLines  StatementLine[]

  @@index([schoolId, kind, status])
}

model FeeSchedule {
  id            String            @id @default(uuid()) @db.Uuid
  schoolId      String            @db.Uuid
  termId        String            @db.Uuid
  term          AcademicTerm      @relation(fields: [termId], references: [id], onDelete: Restrict)
  version       Int
  status        PublicationStatus @default(DRAFT)
  effectiveFrom DateTime?
  approvedById  String?           @db.Uuid
  publishedAt   DateTime?
  createdById   String            @db.Uuid
  createdAt     DateTime          @default(now())
  items         FeeItem[]

  @@unique([schoolId, termId, version])
}

model FeeItem {
  id            String      @id @default(uuid()) @db.Uuid
  schoolId      String      @db.Uuid
  feeScheduleId String      @db.Uuid
  schedule      FeeSchedule @relation(fields: [feeScheduleId], references: [id], onDelete: Restrict)
  code          String
  name          String
  classLabel    String?
  amountMinor   BigInt
  currency      String      @db.VarChar(3)

  @@unique([feeScheduleId, code, classLabel])
}

/// Append-only. Corrections create reversing entries, never edits.
/// amountMinor is signed. Charges are positive. Discounts and payments are negative.
/// Balance = sum of amountMinor for a student and term.
model LedgerEntry {
  id            String          @id @default(uuid()) @db.Uuid
  schoolId      String          @db.Uuid
  studentId     String          @db.Uuid
  student       Student         @relation(fields: [studentId], references: [id], onDelete: Restrict)
  termId        String          @db.Uuid
  term          AcademicTerm    @relation(fields: [termId], references: [id], onDelete: Restrict)
  feeItemId     String?         @db.Uuid
  entryType     LedgerEntryType
  amountMinor   BigInt
  currency      String          @db.VarChar(3)
  paymentId     String?         @unique @db.Uuid
  payment       Payment?        @relation(fields: [paymentId], references: [id], onDelete: Restrict)
  correctionId  String?         @db.Uuid
  importBatchId String?         @db.Uuid
  importBatch   ImportBatch?    @relation(fields: [importBatchId], references: [id], onDelete: Restrict)
  occurredAt    DateTime
  recordedAt    DateTime        @default(now())
  recordedById  String          @db.Uuid

  @@index([schoolId, studentId, termId])
}

model Payment {
  id                   String                      @id @default(uuid()) @db.Uuid
  schoolId             String                      @db.Uuid
  studentId            String                      @db.Uuid
  student              Student                     @relation(fields: [studentId], references: [id], onDelete: Restrict)
  termId               String                      @db.Uuid
  term                 AcademicTerm                @relation(fields: [termId], references: [id], onDelete: Restrict)
  parentAccountId      String                      @db.Uuid
  idempotencyKey       String
  paymentReference     String                      @unique
  amountMinor          BigInt
  currency             String                      @db.VarChar(3)
  method               PaymentMethod
  gatewayProvider      String?
  gatewayReference     String?                     @unique
  gatewayStatus        GatewayStatus               @default(INITIATED)
  reconciliationStatus PaymentReconciliationStatus @default(UNRECONCILED)
  receiptStatus        ReceiptStatus               @default(NOT_ISSUED)
  initiatedAt          DateTime                    @default(now())
  gatewayVerifiedAt    DateTime?
  appliedAt            DateTime?
  withdrawnAt          DateTime?
  createdAt            DateTime                    @default(now())
  updatedAt            DateTime                    @updatedAt
  events               PaymentEvent[]
  ledgerEntry          LedgerEntry?
  receipt              Receipt?
  reconciliations      Reconciliation[]
  escalations          EscalationTicket[]

  @@unique([parentAccountId, idempotencyKey])
  @@index([schoolId, studentId, gatewayStatus])
}

/// Append-only payment history. providerEventId makes webhook handling idempotent.
model PaymentEvent {
  id                String         @id @default(uuid()) @db.Uuid
  schoolId          String         @db.Uuid
  paymentId         String         @db.Uuid
  payment           Payment        @relation(fields: [paymentId], references: [id], onDelete: Restrict)
  eventType         String
  fromGatewayStatus GatewayStatus?
  toGatewayStatus   GatewayStatus?
  source            ActorType
  providerEventId   String?        @unique
  payloadSha256     String?
  createdAt         DateTime       @default(now())

  @@index([paymentId, createdAt])
}

/// Append-only. Rows come from a bank statement or a gateway settlement report.
model StatementLine {
  id                String              @id @default(uuid()) @db.Uuid
  schoolId          String              @db.Uuid
  source            StatementSource
  externalReference String
  accountLast4      String?             @db.VarChar(4)
  amountMinor       BigInt
  currency          String              @db.VarChar(3)
  valueDate         DateTime
  narration         String?
  status            StatementLineStatus @default(UNMATCHED)
  importBatchId     String              @db.Uuid
  importBatch       ImportBatch         @relation(fields: [importBatchId], references: [id], onDelete: Restrict)
  importedById      String              @db.Uuid
  importedAt        DateTime            @default(now())
  reconciliations   Reconciliation[]

  @@unique([schoolId, source, externalReference])
  @@index([schoolId, status])
}

model Reconciliation {
  id              String               @id @default(uuid()) @db.Uuid
  schoolId        String               @db.Uuid
  paymentId       String               @db.Uuid
  payment         Payment              @relation(fields: [paymentId], references: [id], onDelete: Restrict)
  statementLineId String?              @db.Uuid
  statementLine   StatementLine?       @relation(fields: [statementLineId], references: [id], onDelete: Restrict)
  result          ReconciliationResult
  matchMethod     MatchMethod?
  matchedById     String?              @db.Uuid
  matchedAt       DateTime             @default(now())
  note            String?

  @@index([schoolId, result])
}

model Receipt {
  id            String    @id @default(uuid()) @db.Uuid
  schoolId      String    @db.Uuid
  paymentId     String    @unique @db.Uuid
  payment       Payment   @relation(fields: [paymentId], references: [id], onDelete: Restrict)
  receiptNumber String
  amountMinor   BigInt
  currency      String    @db.VarChar(3)
  issuedById    String    @db.Uuid
  issuedAt      DateTime  @default(now())
  voidedById    String?   @db.Uuid
  voidedAt      DateTime?
  voidReason    String?

  @@unique([schoolId, receiptNumber])
}

model LedgerCorrection {
  id            String           @id @default(uuid()) @db.Uuid
  schoolId      String           @db.Uuid
  targetEntryId String           @db.Uuid
  oldValue      Json
  newValue      Json
  reason        String
  status        CorrectionStatus @default(PENDING)
  requestedById String           @db.Uuid
  requestedAt   DateTime         @default(now())
  approvedById  String?          @db.Uuid
  decidedAt     DateTime?

  @@index([schoolId, status])
}

model ClearanceRecord {
  id           String          @id @default(uuid()) @db.Uuid
  schoolId     String          @db.Uuid
  studentId    String          @db.Uuid
  student      Student         @relation(fields: [studentId], references: [id], onDelete: Restrict)
  termId       String          @db.Uuid
  term         AcademicTerm    @relation(fields: [termId], references: [id], onDelete: Restrict)
  status       ClearanceStatus
  recordedById String          @db.Uuid
  approvedById String?         @db.Uuid
  recordedAt   DateTime        @default(now())
  supersededAt DateTime?

  @@index([schoolId, studentId, termId])
}

/// Append-only. Written in the same transaction as the change it records.
model AuditLog {
  id         String    @id @default(uuid()) @db.Uuid
  schoolId   String?   @db.Uuid
  actorType  ActorType
  actorId    String?   @db.Uuid
  action     String
  entityType String
  entityId   String
  beforeJson Json?
  afterJson  Json?
  ipHash     String?
  userAgent  String?
  requestId  String
  createdAt  DateTime  @default(now())

  @@index([schoolId, createdAt])
  @@index([entityType, entityId])
}

model SharedRecord {
  id               String            @id @default(uuid()) @db.Uuid
  schoolId         String            @db.Uuid
  recordKey        String
  recordType       SharedRecordType
  title            String
  body             String
  version          Int
  status           PublicationStatus @default(DRAFT)
  contentHash      String
  effectiveFrom    DateTime?
  effectiveTo      DateTime?
  submittedById    String?           @db.Uuid
  approvedById     String?           @db.Uuid
  publishedAt      DateTime?
  supersededAt     DateTime?
  retractedAt      DateTime?
  retractedById    String?           @db.Uuid
  retractionReason String?
  createdById      String            @db.Uuid
  createdAt        DateTime          @default(now())

  @@unique([schoolId, recordKey, version])
  @@index([schoolId, status])
}

model EscalationTicket {
  id              String             @id @default(uuid()) @db.Uuid
  schoolId        String             @db.Uuid
  referenceCode   String             @unique
  parentAccountId String             @db.Uuid
  studentId       String?            @db.Uuid
  paymentId       String?            @db.Uuid
  payment         Payment?           @relation(fields: [paymentId], references: [id], onDelete: Restrict)
  category        EscalationCategory
  parentMessage   String             @db.VarChar(500)
  status          EscalationStatus   @default(OPEN)
  assignedStaffId String?            @db.Uuid
  internalNote    String?
  openedAt        DateTime           @default(now())
  assignedAt      DateTime?
  resolvedAt      DateTime?

  @@index([schoolId, status, assignedStaffId])
}

model ConsentRecord {
  id              String      @id @default(uuid()) @db.Uuid
  schoolId        String      @db.Uuid
  parentAccountId String      @db.Uuid
  consentType     ConsentType
  granted         Boolean
  channel         String
  recordedAt      DateTime    @default(now())
  withdrawnAt     DateTime?

  @@index([parentAccountId, consentType])
}

model Job {
  id          String    @id @default(uuid()) @db.Uuid
  schoolId    String?   @db.Uuid
  jobType     String
  payload     Json
  status      JobStatus @default(QUEUED)
  attempts    Int       @default(0)
  runAfter    DateTime  @default(now())
  lockedUntil DateTime?
  lastError   String?
  createdAt   DateTime  @default(now())

  @@index([status, runAfter])
}

/// Managed by raw SQL migrations (Section 11). Declared so the schema knows the table exists.
/// Prisma Client does not read or write this model. Migration diffs for this table are reviewed by hand.
model SharedRecordChunk {
  id                  String           @id @db.Uuid
  schoolId            String           @db.Uuid
  sharedRecordId      String           @db.Uuid
  sharedRecordVersion Int
  chunkIndex          Int
  chunkText           String
  tokenCount          Int
  embedding           Unsupported("vector(1536)")
  contentHash         String
  sourceType          SharedRecordType
  isCurrent           Boolean
  publishedAt         DateTime
  effectiveFrom       DateTime
  effectiveTo         DateTime?
  createdAt           DateTime

  @@unique([sharedRecordId, sharedRecordVersion, chunkIndex])
  @@map("shared_record_chunks")
  @@ignore
}
```

### 9.1 Model Notes

| Model | Purpose | Unique constraints | Indexes | Delete behavior | Append-only |
|---|---|---|---|---|---|
| School | Tenant root | none | none | Never deleted. | No |
| SetupStep | Progress for the eight setup steps | schoolId, stepNumber | none | Restrict | No |
| AcademicTerm | Session and term. Holds historicalVerifiedAt. | schoolId, sessionLabel, termName | schoolId, status | Restrict | No |
| StaffRole | Named role per school | schoolId, name | none | Restrict | No |
| Permission | Permission codes | code | none | Restrict | No |
| RolePermission | Role to permission mapping | composite key | none | Restrict | No |
| StaffUser | Staff account. TOTP secret held by reference. | schoolId, email | none | Disable through disabledAt | No |
| ParentAccount | Parent identity. No password. | email, phone | none | Disable through disabledAt | No |
| OneTimeCode | Hashed sign-in and recovery codes | none | destinationHash, purpose, createdAt | Hard delete after 30 days | No |
| Session | Database session with revocation | tokenHash | parentAccountId, staffUserId | Hard delete after expiry | No |
| Student | Student record with withdrawal fields | schoolId, admissionNumber | schoolId, lastName, firstName | Restrict | No |
| GuardianLinkCode | Single-use link code issued by school | codeHash | schoolId, studentId | Restrict | No |
| GuardianLink | Parent to student link | parentAccountId, studentId | schoolId, studentId, status | Restrict | No |
| ImportBatch | One upload and its approval state | none | schoolId, kind, status | Restrict | No |
| FeeSchedule | Versioned fee schedule per term | schoolId, termId, version | none | Restrict | No |
| FeeItem | Line item in a schedule | feeScheduleId, code, classLabel | none | Restrict | No |
| LedgerEntry | Every charge, discount, payment, and correction | paymentId | schoolId, studentId, termId | Restrict | Yes |
| Payment | One payment with three separate status fields | parentAccountId, idempotencyKey; paymentReference; gatewayReference | schoolId, studentId, gatewayStatus | Restrict | No (history in PaymentEvent) |
| PaymentEvent | Gateway history and webhook record | providerEventId | paymentId, createdAt | Restrict | Yes |
| StatementLine | Bank lines and settlement lines | schoolId, source, externalReference | schoolId, status | Restrict | Yes |
| Reconciliation | Match decisions | partial unique rules in SQL (Section 9.2) | schoolId, result | Restrict | No |
| Receipt | School receipt, one per payment | paymentId; schoolId, receiptNumber | none | Restrict. Void with reason. | No |
| LedgerCorrection | Correction request and approval | none | schoolId, status | Restrict | No |
| ClearanceRecord | Clearance status per student and term | none | schoolId, studentId, termId | Restrict. Supersede, never edit. | No |
| AuditLog | Change and access history | none | schoolId, createdAt; entityType, entityId | Never deleted | Yes |
| SharedRecord | Published text and its versions | schoolId, recordKey, version | schoolId, status | Restrict. Status changes only. | No |
| EscalationTicket | Parent question and staff handling | referenceCode | schoolId, status, assignedStaffId | Restrict | No |
| ConsentRecord | Consent and preference history | none | parentAccountId, consentType | Restrict | No |
| Job | Background work queue | none | status, runAfter | Hard delete 30 days after success | No |
| SharedRecordChunk | Vector rows (Section 11) | sharedRecordId, version, chunkIndex | Set in SQL | Rows deleted on retraction or supersede | No |

**Payment state rule:** gatewayStatus, reconciliationStatus, and receiptStatus are separate enums. appliedAt marks the ledger credit. The four concept stages map as follows: initiated (INITIATED, PENDING, or a declared transfer), gateway verified (SUCCESSFUL), matched (MATCHED), and applied and receipted (appliedAt set and receiptStatus ISSUED).

**Money rule:** every money field is BigInt in minor units. Conversion to display format happens in the UI layer only.

### 9.2 SQL Constraints Added in Migrations

- Foreign keys from every schoolId to School.id.
- Foreign keys from actor ID columns to StaffUser.id or ParentAccount.id.
- Check: LedgerCorrection.requestedById is not equal to LedgerCorrection.approvedById.
- Check: SharedRecord.effectiveFrom is not null when status is SUBMITTED, PUBLISHED, or SUPERSEDED.
- Partial unique index on reconciliations(payment_id) where result = 'MATCHED'. One match per payment.
- Partial unique index on reconciliations(statement_line_id) where result = 'MATCHED'. One match per statement line.
- Partial unique index on shared_records(school_id, record_key) where status = 'PUBLISHED'. One published version per record.
- Check on LedgerEntry: CHARGE and OPENING_BALANCE amounts are greater than zero. DISCOUNT and PAYMENT amounts are less than zero. CORRECTION_REVERSAL amounts are not zero.
- Check on ParentAccount: email or phone is not null.
- Check on Payment: appliedAt is null unless reconciliationStatus is MATCHED.
- Triggers on LedgerEntry, PaymentEvent, StatementLine, and AuditLog blocking UPDATE and DELETE.
- The application role has no DELETE rights on financial tables.

---

## 10. Vector Database Architecture and Design

### 10.1 What Gets Embedded

Only SharedRecord versions with status PUBLISHED. Embedded types are TERM_DATE, PAYMENT_POLICY, and CONTACT. Fee amounts never enter the vector store. They come only from the FeeSchedule.

### 10.2 What Never Gets Embedded

- Student records, guardian links, and any field that identifies a student or parent.
- Ledger entries, balances, payments, payment events, statement lines, reconciliations, receipts, corrections, and clearance records.
- Escalation tickets, internal notes, and parent messages.
- Audit logs, sessions, OTP codes, and job payloads.
- Draft, submitted, superseded, or retracted shared records.
- Parent query text, except in its scrubbed form for the single embedding call that serves that query. Scrubbed query text is never stored.

These rules are enforced in code. The embedding job reads only from SharedRecord rows with status PUBLISHED.

### 10.3 Embedding Model

[ASSUMPTION] text-embedding-3-small, 1,536 dimensions, from a commercial provider. The provider's data processing terms must allow processing of school policy text and scrubbed queries. A signed data processing agreement is a launch gate [VERIFY BEFORE LAUNCH]. Pidgin and typo handling must be tested (Section 7.6). The dimension is fixed in the schema. Changing models requires full re-embedding and a new column or table (OQ-04).

### 10.4 Chunking Strategy

- Target chunk size: 300 to 450 tokens.
- Overlap: 50 tokens between adjacent chunks.
- Boundaries: split on headings first, then on paragraphs. Never split a table row or a list item across chunks.
- Each chunk includes the record title as a prefix. The prefix counts toward the token limit.
- Chunk counts per school are in Section 10.11.

### 10.5 Search Method

- **No approximate index in version one.** The query computes exact cosine distance over the rows that pass the filters.
- **Why:** one school holds about 25 to 80 chunks, and all 50 schools together hold about 4,000 at most. Exact search over that size takes milliseconds. An approximate index adds tuning, a recall risk, and a failure mode with no benefit at this size.
- **Filter order:** the WHERE clause applies the school filter and the current-version filter before ORDER BY. The filter is a true pre-filter.
- **Trigger for change:** add an HNSW index with cosine operators when the table exceeds 100,000 rows. Re-test the threshold after adding it.

### 10.6 Query Path

The mandatory filters run inside the same SQL statement, before ordering:

1. school_id equals the session's school ID.
2. is_current equals true.
3. effective_from is on or before now().
4. effective_to is null or after now().

The query returns at most 5 rows. The application drops rows whose distance is above the threshold. The filters never run in application code after the query. A forgotten filter in application code would leak data.

### 10.7 Similarity Threshold

- Cosine distance threshold: 0.35 or lower to keep a result [ASSUMPTION]. This is a starting value only.
- **Calibration gate:** before launch, the threshold is set on the 100-query set (Section 7.6, Set B). The chosen value and its evaluation results are recorded.
- Below the threshold: no result is shown. The refusal in Section 7.4 appears.
- Between the threshold and 0.45: the refusal appears, and the query hash is logged for review.

### 10.8 Publishing a New Version

1. Staff submit a new version. Status becomes SUBMITTED. effectiveFrom must be set (FR-090).
2. The approver requests publication. The system splits the text into chunks, computes embeddings, and inserts the chunks with is_current set to false. The job is idempotent. It deletes any existing rows for that version before inserting.
3. The publish action stays disabled until every chunk is stored and the chunk count matches the expected count.
4. In one transaction:
   - Delete the chunk rows of the previous published version.
   - Set is_current to true on the new version's chunk rows.
   - Set the previous SharedRecord to SUPERSEDED with supersededAt.
   - Set the new SharedRecord to PUBLISHED with publishedAt and approvedById.
5. Search never sees a gap. Before the transaction, search returns the old version. After it, search returns the new version.

### 10.9 Retraction

1. The approver retracts a version. In one transaction:
   - Set the SharedRecord status to RETRACTED, with retractedAt and retractionReason.
   - Delete all chunk rows for that version.
2. Search filters on is_current. If the chunk rows are present when a search runs, the filter still checks status through the SharedRecord join before returning text.
3. A nightly check compares published SharedRecord rows with chunk rows. Any mismatch is logged and corrected.

### 10.10 Access Rules for Search

- Search runs only inside the parent portal's search route.
- The parent selects a school before searching. Results come from that school only. Results are never merged across schools.
- The search route is rate-limited to 20 queries per parent per minute [ASSUMPTION].

### 10.11 Capacity and Cost Estimate

| Measure | One school | 50 schools |
|---|---|---|
| Published records | 20 to 60 | 1,000 to 3,000 |
| Chunks (about 1.3 chunks per record) | 26 to 78 | 1,300 to 3,900 |
| Storage per vector (1,536 values at 4 bytes) | about 6 KB | about 6 KB |
| Total vector storage | under 0.5 MB | about 24 MB at most |
| Embedding calls per record publication | 1 to 2 | Same |
| Embedding calls per parent search | 1 | Same |
| Monthly embedding cost | Negligible at pilot volume [VERIFY with current provider pricing] | Still small at this record volume [VERIFY] |

The main cost driver is parent search volume, not storage. The team tracks search counts from the first pilot week.

---

## 11. Vector Database Model

### 11.1 Table Definition

| Column | Type | Nullable | Constraint or index | Purpose |
|---|---|---|---|---|
| id | uuid | No | Primary key, default gen_random_uuid() | Row identifier |
| school_id | uuid | No | Index with is_current | Mandatory school filter |
| shared_record_id | uuid | No | Foreign key to SharedRecord.id | Source record |
| shared_record_version | integer | No | Unique with shared_record_id and chunk_index | Version the chunk belongs to |
| chunk_index | integer | No | Greater than or equal to 0 | Position within the version |
| chunk_text | text | No | none | Text returned to the parent |
| token_count | integer | No | Greater than 0 | Size check and cost tracking |
| embedding | vector(1536) | No | none in version one (Section 10.5) | Similarity search |
| content_hash | text | No | none | Skip re-embedding only when text and model match |
| source_type | SharedRecordType | No | Enum shared with Prisma | Filtering and display |
| is_current | boolean | No | Default false. Indexed with school_id. | Marks the live version |
| published_at | timestamptz | No | none | Publication time |
| effective_from | timestamptz | No | none | Start of the effective window |
| effective_to | timestamptz | Yes | Must be after effective_from when set | End of the effective window |
| created_at | timestamptz | No | Default now() | Insert time |

### 11.2 Relationship to SharedRecord

Each chunk row belongs to one SharedRecord version, identified by shared_record_id and shared_record_version. The Prisma model SharedRecordChunk maps to the table with @@map and is marked @@ignore. Prisma Client does not read or write it. All access uses raw SQL through $queryRaw and $executeRaw, with parameters bound through the tagged template form only.

### 11.3 Migration SQL

```sql
CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE shared_record_chunks (
  id                    uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  school_id             uuid NOT NULL,
  shared_record_id      uuid NOT NULL REFERENCES "SharedRecord"(id) ON DELETE RESTRICT,
  shared_record_version integer NOT NULL,
  chunk_index           integer NOT NULL CHECK (chunk_index >= 0),
  chunk_text            text NOT NULL,
  token_count           integer NOT NULL CHECK (token_count > 0),
  embedding             vector(1536) NOT NULL,
  content_hash          text NOT NULL,
  source_type           "SharedRecordType" NOT NULL,
  is_current            boolean NOT NULL DEFAULT false,
  published_at          timestamptz NOT NULL,
  effective_from        timestamptz NOT NULL,
  effective_to          timestamptz,
  created_at            timestamptz NOT NULL DEFAULT now(),
  CONSTRAINT shared_record_chunks_version_chunk_key
    UNIQUE (shared_record_id, shared_record_version, chunk_index),
  CONSTRAINT shared_record_chunks_effective_window
    CHECK (effective_to IS NULL OR effective_to > effective_from)
);

CREATE INDEX shared_record_chunks_school_current_idx
  ON shared_record_chunks (school_id, is_current);
```

### 11.4 Query Example

```sql
SELECT c.shared_record_id,
       c.shared_record_version,
       c.chunk_text,
       c.source_type,
       c.embedding <=> $2::vector AS distance
FROM shared_record_chunks c
WHERE c.school_id = $1
  AND c.is_current = true
  AND c.effective_from <= now()
  AND (c.effective_to IS NULL OR c.effective_to > now())
ORDER BY c.embedding <=> $2::vector
LIMIT 5;
```

Parameter $1 is the school ID from the session. Parameter $2 is the query embedding. The application drops rows with distance above the calibrated threshold (Section 10.7).

### 11.5 Prisma Migration Handling

- The schema declares SharedRecordChunk as an ignored model so the schema records that the table exists. [VERIFY BEFORE LAUNCH] that the pinned Prisma version does not generate a drop statement for the table during migrate dev. Record the result in the migration notes.
- Create migrations with `prisma migrate dev --create-only`. Replace generated SQL with the statements in Section 11.3 and the constraints in Section 9.2 as needed.
- CREATE EXTENSION needs a role with extension rights. On managed services this may require an admin role. Migrations run under that role. The application role gets SELECT, INSERT, UPDATE, and DELETE on the chunk table only [VERIFY BEFORE LAUNCH with the chosen host].
- A CI check fails any migration that drops the shared_record_chunks table or removes the vector extension.
- Raw SQL files live beside the Prisma migrations. They are reviewed in the same pull request as the schema change.
- A CI test runs all migrations on an empty database. It confirms the extension, the table, the indexes, and the ledger and audit triggers.

---

## 12. Business Model

### 12.1 Free Model

Feedesk charges no portal fee to schools or parents. The school is the customer for setup and support. The school owns its relationship with families. Parents pay the school's fees through the school's configured payment path. Settlement goes to the school's designated account. Feedesk does not hold school funds and does not distribute them.

Any gateway fee is a payment charge, not a portal fee. The school decides who bears it (OQ-06). The amount shows on the pay screen before the parent pays (FR-009).

### 12.2 Cost Lines

| Cost line | Who pays (version one default) | Notes |
|---|---|---|
| Gateway transaction fees | Per agreement with the school [OQ-06] | Card payments only. Transfers do not pass through the gateway under Option B. |
| Hosting (web and database) | Feedesk | Free tiers for the prototype. Paid tier needed for production uptime and backups [VERIFY BEFORE LAUNCH]. |
| Backups and recovery | Feedesk | Included in hosting plan or a separate service |
| Maintenance and support | Feedesk | Staff time |
| Embedding and AI calls | Feedesk | Small at pilot volume (Section 10.11) |
| One-time code messages | Feedesk | Per message cost. Sign-in happens often at term start [VERIFY BEFORE LAUNCH]. |

### 12.3 Unit Cost Model (Per School Per Term)

Example numbers are estimates. Replace them with real figures before launch.

| Variable | Symbol | Example estimate |
|---|---|---|
| Students | S | 400 [ASSUMPTION] |
| Card payments per student per term | P | 3 [ASSUMPTION] |
| Average payment amount (NGN) | A | 80,000 [ASSUMPTION] |
| Gateway percentage fee | r | [VERIFY BEFORE LAUNCH] |
| Gateway fixed fee per transaction | f | [VERIFY BEFORE LAUNCH] |
| Hosting cost per school per term | H | [ASSUMPTION, set from the chosen plan] |
| Support hours per school per term | T | 4 [ASSUMPTION] |
| Sign-in codes per parent per term | C | 10 [ASSUMPTION] |
| Cost per code message | m | [VERIFY BEFORE LAUNCH] |

Gateway cost per term = S × P × (r × A + f). Messaging cost per term = S × 2 × C × m, where the factor 2 counts the parents per student [ASSUMPTION].

Staff support cost per term = T × hourly rate [ASSUMPTION].

Total Feedesk cost per term = H + support cost + messaging cost + gateway cost (only if Feedesk bears it).

### 12.4 Pilot Funding

[DECISION NEEDED] The pilot is funded by [FUNDER OR TEAM] through [GATE DATE]. The owner of this decision is [NAME]. Before the gate date, the team confirms the funding for version one and the cost in Section 12.3. After the pilot, the team decides on a funded source or a change to scope.

### 12.5 Possible Future Revenue

The concept lists these options. None is part of version one.

- Sponsorship.
- Grants.
- Institutional partnerships.
- A separately funded service arrangement.

### 12.6 What Is Ruled Out

- Charging parents for portal use.
- Advertising on any financial screen.
- Taking a share of settlement funds or holding school funds.
- Placing essential financial records behind a paid tier.
- Selling or sharing student data.
- A per-transaction platform fee charged to schools. This is not in the concept. Any such model needs a new concept review.

### 12.7 Conditions to Meet Before Launch

- Gateway merchant agreement signed by the school, with the merchant-of-record arrangement documented [VERIFY BEFORE LAUNCH].
- Settlement goes to the school's designated account. The gateway confirms this in writing.
- Agreement on who bears gateway fees, and how the fee is shown to parents.
- Tax treatment of fees and gateway charges confirmed with an accountant [VERIFY BEFORE LAUNCH].
- Data protection obligations under the Nigeria Data Protection Act 2023 identified, including any registration or assessment duties [VERIFY BEFORE LAUNCH].
- Data processing agreements signed with the OTP provider and the embedding provider.
- Privacy notice approved by the school.
- Backup and recovery tested.
- Incident response plan agreed with the school.
- Pilot funding decision recorded (Section 12.4).

### 12.8 Sensitivity: Gateway Fees Fall on Feedesk

If Feedesk bears gateway fees, its cost rises with payment volume. At the example values, each 1 percentage point increase in the gateway rate adds about 1% of total card payment value to Feedesk's cost. Feedesk does not absorb fees silently. If Feedesk bears any fee, the decision and its cost go into the business plan, and the model needs a funded source. Without that source, the school or the parent bears the fee, shown before payment (FR-009).

---

## 13. Success Metrics

| Metric | Definition | Target | Measurement method | Owner |
|---|---|---|---|---|
| Balance accuracy | Share of balances that equal the approved ledger sum | 100% in the automated test set. 100% in the full-ledger check at term end. Any live mismatch is a Severity 1 incident. | Automated test set. Full-ledger comparison at term end. Weekly sample of 50 live balances to detect problems. | Engineering lead |
| Receipt accuracy | Count of receipt numbers shown for payments without an ISSUED receipt | 0 | Daily query comparing Payment.receiptStatus and Receipt rows | Engineering lead |
| Parent independence | Share of pilot parents who answer the three main fee questions without calling the bursary | Hypothesis: 80% by end of term. Review if below 60%. | Bursary call count in week 1 (baseline) and week 8. Survey of at least 30 active parents at week 8. | Product lead |
| Staff time on routine queries | Median staff minutes per routine query | 50% reduction against the week 1 baseline [ASSUMPTION] | Staff time log for weeks 1 and 8 | Pilot school bursar |
| Gateway-to-portal latency | Time from gateway confirmation to SUCCESSFUL status in the portal | 95% within 2 minutes (G5) | Timestamps on PaymentEvent and gatewayVerifiedAt | Engineering lead |
| Balance screen speed | Time to first content on a simulated slow 3G profile | Under 3 seconds (G7) [ASSUMPTION] | Automated performance test each release | Engineering lead |
| Data protection | Count of successful unauthorized private-record requests | 0 | Access tests (Section 6.11), denied-access log review | Security owner |
| Reconciliation lag (card) | Time from settlement report import to MATCHED | Under 1 business day after the report is imported | Timestamps on Reconciliation and StatementLine | Bursar |
| Reconciliation lag (transfer) | Time from declaration to MATCHED | Under 2 business days [ASSUMPTION] | Timestamps on Payment and Reconciliation | Bursar |
| Escalation resolution time | Median business hours from OPEN to RESOLVED | 2 business days for 80% of tickets [ASSUMPTION] | EscalationTicket timestamps | Bursar |
| Failed payment rate | Count of gateway FAILED divided by initiated card payments, excluding parent cancellations | Report weekly. Review if above 5% [ASSUMPTION] | Payment and PaymentEvent data | Engineering lead |
| Unauthorized access attempts | Count of ACCESS_DENIED audit entries | Every attempt logged. Review any spike. | AuditLog query | Security owner |
| AI scripted set | Pass rate on the 14 scripted queries (Section 7.6, Set A) | 14 of 14 before each release | Automated test run | Product lead |
| AI real-query set | Pass rate on the 100 real bursary queries (Section 7.6, Set B) | 95% or higher before launch | Evaluation run before launch and monthly | Product lead |
| Webhook verification rate | Share of gateway SUCCESSFUL states with a stored verification call | 100% | Payment and PaymentEvent check | Engineering lead |

---

## 14. Risks

Likelihood ratings are team judgement until pilot data exists.

| Risk | Likelihood | Impact | Prevention | Detection | Owner |
|---|---|---|---|---|---|
| R1. Private records exposed to the wrong parent | Low | Severe | Server-side checks on every private request (Section 8.4). Verified guardian links. School ID on every query. Restricted admin permissions. Unauthorized-account tests before launch. | Denied-access log review. Weekly test run with a test account. | Security owner |
| R2. Payment reported incorrectly | Medium | High | Server-side verification. Unique references. Idempotent processing. Separate states. Open-payment block (FR-010). Apply only after match (FR-030). | Daily check of SUCCESSFUL payments with no verification record. Reconciliation aging report. | Engineering lead |
| R3. Incorrect source records become trusted | Medium | High | Import validation. Opening balance reconciliation. Maker-checker approval (FR-071). Correction history. Last-verified timestamps. Discrepancy flags. | Reconciliation of opening balances against the paper ledger. Staff review of flagged rows. | Bursar |
| R4. Gateway outage | Medium | Medium | Clear error state. Retry with the same reference. No balance change on failure. | Gateway status monitoring. Failed payment rate metric. | Engineering lead |
| R5. Staff error during import | High | Medium | Draft batches. Validation. Duplicate detection. Preview before approval. | Post-import diff against the source file. | Bursar |
| R6. Mobile data loss during payment | High | Medium | Verification runs on return and on timeout. Payment state kept server-side. Parent sees the status, not a guess. | Count of INITIATED payments older than 30 minutes. | Engineering lead |
| R7. Spreadsheet format drift | Medium | Medium | Versioned templates (Section 6.14). Rejection of unknown columns. | Import rejection rate. | Bursar |
| R8. Parent account sharing | High | Medium | Session list with device labels. Revocation. Sign-out per device. Alerts for sign-ins from new devices. | Sign-ins from new devices and locations. | Product lead |
| R9. Incorrect guardian link | Medium | Severe | Link code issued by school. Single use. Staff approval. Parent cannot approve own link. Withdrawal revokes links. | Weekly review of link approvals. Parent complaints. | Bursar |
| R10. Payment dispute handling | Medium | High | DISPUTED set by staff only. Parent sees "under review." No automatic resolution. | Count of open disputes older than 5 business days. | Bursar |
| R11. Stale or missing vector content | Low | High | Atomic version flip (Section 10.8). Chunk deletion in the same transaction as retraction. Query filter on is_current. Nightly reconciliation. | Nightly check. Evaluation sets in Section 7.6. | Engineering lead |
| R12. Forged webhook | Low | High | Signature verification (FR-025). Verification call before any state change (FR-023). | Rejected signature count. | Security owner |
| R13. Bank or settlement file format change | Medium | Medium | Parser tests against sample files. Rejection of unknown formats with a clear message. | Import failure rate. | Engineering lead |
| R14. Parent-typed content contains personal data | Medium | Medium | 500-character limit. Staff note field separate. Retention rule for messages (FR-125). | Periodic sample review. | Product lead |
| R15. Embedding provider change or outage | Low | Medium | Fixed dimension documented. Search degrades to refusal, not wrong answers. Re-embedding procedure in Section 10.8. | Embedding job failure alerts. | Engineering lead |
| R16. Parent query text reaches a third party | Medium | High | Query scrubbing before the embedding call (7.2.2). Data processing agreement as a launch gate. Original query never stored. | Test that scrubbing removes names and digits. Review of provider terms. | Security owner |
| R17. Insider changes the ledger | Low | Severe | Maker-checker approval for imports, corrections, and clearance records (FR-071 to FR-073). Audit log cannot be edited. Staff cannot approve their own requests. | Weekly review of approvals. Alert when one user creates and approves within one day. | Owner |
| R18. OTP delivery fails at term opening | Medium | High | Two channels (phone and email). Rate limits that do not lock out a whole destination for long. Bursary can issue a manual verification check. | Monitor OTP send failures and sign-in completion rate. | Engineering lead |
| R19. Card settlements never match | Medium | High | Settlement report import with a fixed template (Section 6.14). Match on gateway reference and amount. Aging report. | Count of SUCCESSFUL card payments without MATCHED after 3 business days. | Bursar |
| R20. Shared phone exposes balances | Medium | Medium | No financial data stored on the device (Section 8.10). Sign-out per device. Sessions expire. | Review of sign-out use in the pilot. | Product lead |

---

## 15. Open Questions

| ID | Question | Why it matters | Options | Recommended answer | Decision owner | Needed by |
|---|---|---|---|---|---|---|
| OQ-01 | Virtual account per payment, or existing school account with declared and matched transfers? | Sets how transfers are recorded and how much reconciliation can be automated. | A: Virtual accounts. B: Existing account, declared and matched. | B for pilot. Revisit A for version two. | Product lead with school owner | Before design freeze |
| OQ-02 | Which card gateway? | Determines card support, settlement reports, fee structure, and webhook design. | Paystack, Flutterwave, Interswitch, or another licensed provider [VERIFY BEFORE LAUNCH] | Choose the provider with a test mode, server-side verification, documented webhook signatures, and daily settlement reports to a school-owned account. | Product lead and school owner | Before staging |
| OQ-03 | Which sign-in method for parents? | Affects onboarding on low-end phones and recovery. | Phone OTP. Email OTP. Both. | Closed: one-time codes by phone with email as second channel. No parent password. | Product lead | Closed |
| OQ-04 | Which embedding model? | Affects search quality on Pidgin, typos, and short queries. Fixes vector dimension. | Commercial 1,536-dimension model. Open multilingual model, self-hosted. | Commercial 1,536-dimension model for pilot. Test on the 100-query set. | Engineering lead | Before evaluation |
| OQ-05 | How long must financial records be kept? | Sets retention and deletion rules. | School policy. Legal minimum [VERIFY BEFORE LAUNCH]. | School's written policy. Default 6 years until confirmed. | School owner with legal advisor | Before live import |
| OQ-06 | Who bears gateway fees, and how are they shown? | Sets the cost to Feedesk and the amount parents see before paying. | School bears fee. Parent bears fee. Split. Feedesk bears fee (needs funding). | School or parent bears the fee, shown before payment. Feedesk does not absorb fees silently. | School owner | Before live payments |
| OQ-07 | What is the school's policy on former guardian access after withdrawal? | The concept defers to the school's policy. The default must be set. | Read-only for a fixed period. No access. Receipts only. | No access by default. Staff release historical records on request. | School owner | Before live import |
| OQ-08 | Where must data be stored and processed? | Affects hosting region, backups, and provider choice. | In-country. Regional. Global with contracts. | Confirm obligations under the Nigeria Data Protection Act 2023 before choosing a region [VERIFY BEFORE LAUNCH]. | Product lead with legal advisor | Before staging |
| OQ-09 | Are partial payments allowed, and what is the minimum? | Changes payment rules, balances, and receipts. | Full payment only. Partial with school minimum. Partial with any amount. | Partial payments only when the school sets a minimum. Off by default. | School owner | Before live payments |
| OQ-10 | Are staff permissions limited by term or class? | Affects role design and data access. | Role only. Role plus class scope. | Role only for pilot. | Product lead | Before access build |
| OQ-11 | Does the school need a second factor for parents? | Affects security and onboarding friction. | None. Optional. Required above a set amount. | Optional in version one. Revisit after pilot. | Security owner | Before launch |
| OQ-12 | When will historical paper ledgers be digitized and verified? | Sets what previous term records show at launch. | Before pilot. During pilot. After pilot. | Digitize and verify at least one prior term before pilot. | Pilot school bursar | Before pilot |
| OQ-13 | Does the locked stack hold? | The stack is locked. A limit may block a requirement. | Keep the stack and accept the workaround. Change the stack. | Closed: the stack holds. Sessions use Prisma tables, not Auth.js. Vectors use raw SQL. | Engineering lead | Closed |
| OQ-14 | How long may the platform keep parent query records? | Affects privacy and debugging. | Hash only. Short text retention. | Hash only (Section 7.4). | Product lead | Before evaluation |
| OQ-15 | Which provider sends one-time codes, and does it reach Nigerian networks reliably? | Parents cannot sign in without codes. | Named SMS provider. Named email provider. Both. | Test delivery on the three largest local networks before pilot. Use email as the second channel. | Engineering lead | Before staging |
| OQ-16 | How often does the gateway send settlement reports, and in what format? | Sets reconciliation lag and the settlement template. | Daily. Weekly. On request. | Daily report if the gateway offers it. Confirm the format before writing the template. | Engineering lead | Before staging |
| OQ-17 | Who approves sensitive changes, and does the list in FR-080 match the school's policy? | The concept requires owner approval. The list must match the school's policy. | Owner only. Owner or any APPROVE_FINANCE holder. | Owner or any APPROVE_FINANCE holder, never the requester. Confirm the list with the school. | School owner | Before pilot |
| OQ-18 | Who funds the pilot, and through which date? | Section 12.4 has no funder. | Named funder. Team-funded through a set date. | Decision required before the pilot. | Product lead | Before pilot |
| OQ-19 | Who is the named bursary contact when the bursar leaves? | Escalations and refusals name this person. | Named deputy. Role-based address. | Use a role-based contact plus a named deputy. Update at each staff change. | School owner | Before pilot |
| OQ-20 | Are data processing agreements in place with the OTP provider and the embedding provider? | Launch gate for R16 and R18. | Signed. Not signed. | Signed before any live data. | Product lead with legal advisor | Before staging |
| OQ-21 | Will the school and owner accept the change to concept Feature 3 under Option B? | Option B changes how transfers are paid. The concept must match the build. | Accept. Reject and use Option A. | Accept Option B for pilot, with the concept amended. | School owner and product lead | Before pilot |

---

## Assumptions Log

| ID | Assumption | Section | Risk if wrong |
|---|---|---|---|
| A-01 | Version one is web only. No native apps. | 3.2 | Missed adoption on app-dependent devices |
| A-02 | Parents make one payment per child. | 5.3 (SB-2) | Split-payment flow needed |
| A-03 | Partial payments are off by default. | 5.3 (SB-1) | Balances and receipts harder to explain |
| A-04 | Parents join with a school-issued, single-use link code. | 6.3 (FR-041) | Weak link verification or onboarding friction |
| A-05 | A 404 response for mismatched IDs prevents student existence leaks. | 6.3 (FR-045) | Minor leak if the school requires other behavior |
| A-06 | Parents sign in by one-time code with no password. | 6.3 (FR-040), OQ-03 | Sign-in flow changes late in build |
| A-07 | Sessions and OTP codes use Prisma tables. Auth.js is not used. | 8.3 | Custom session code needs security review |
| A-08 | Session timeouts of 14 days idle and 30 days absolute. | 6.3 (FR-051) | Sessions too short or too long |
| A-09 | TOTP required for OWNER, BURSAR, and APPROVE_FINANCE roles. | 6.3 (FR-050) | Weak protection for high-risk roles |
| A-10 | Webhook signature verification uses the gateway's documented method. | 8.5 | Forged events accepted if misimplemented |
| A-11 | A 30-minute window before verification of card payments. | 6.2 (FR-032) | Slow status or excess verification calls |
| A-12 | Duplicates within 24 hours set DISCREPANCY. | 6.2 (FR-038) | Legitimate repeat payments flagged |
| A-13 | Option B (declared transfers, matched by staff) for the pilot. | 8.6, OQ-21 | Higher staff workload. Concept amendment needed. |
| A-14 | Job table polled by a schedule, with immediate starts from webhooks. | 8.1, 8.5 | Job throughput at larger scale |
| A-15 | Mobile budgets: 150 KB JavaScript, 300 KB page, 100 KB responses, 15-second timeout. | 8.10 | Slow screens on low-end phones |
| A-16 | No financial data is stored on the device. | 8.10 | Poor-connection experience worse |
| A-17 | Backups kept 30 days with point-in-time recovery. | 8.9 | Recovery window shorter than school policy |
| A-18 | Money stored as BigInt in minor units, sent as strings. | 9 | Precision or serialization errors |
| A-19 | Embedding model: text-embedding-3-small, 1,536 dimensions. | 10.3 | Re-embedding needed if model changes |
| A-20 | Exact search in version one. No approximate index below 100,000 rows. | 10.5 | Latency at larger scale |
| A-21 | Cosine distance threshold of 0.35 as a starting value, calibrated before launch. | 10.7 | False refusals or wrong matches |
| A-22 | Refusal logs store a query hash only. | 7.4 | Debugging harder |
| A-23 | Search rate limit of 20 queries per parent per minute. | 10.10 | Legitimate heavy use blocked, or abuse allowed |
| A-24 | Refusal rate above 30% in one week triggers review. | 7.7 | Problems unnoticed or noise |
| A-25 | Search returns chunk text without generated summaries in version one. | 7.2, 7.3 | Raw text harder to read |
| A-26 | Owner review after 3 business days in SUCCESSFUL and unmatched. | 8.7 | Slow reconciliation unnoticed |
| A-27 | 6-year default retention for financial records. | 6.9 (FR-124) | Conflict with school or legal policy |
| A-28 | Parent sign-in is per school. No cross-school search. | 10.10 | Multi-school parents need another model |
| A-29 | Example unit cost inputs (400 students, 3 payments, NGN 80,000 average, 10 codes per parent). | 12.3 | Business case misjudged |
| A-30 | Pilot targets for parent independence (80%) and staff time (50%) are hypotheses. | 3.1, 13 | Targets set too high or too low |
| A-31 | Routing tiers in 7.2.1 catch the query types in the eval set. | 7.2.1, 7.6 | Routing misses real queries |
| A-32 | Query scrubbing removes child names and digits before embedding. | 7.2.2 | Personal data reaches the provider |
| A-33 | A link code expires after 7 days. | 6.3 (FR-041) | Parents unable to link in time |
| A-34 | Recovery revokes all parent sessions by default. | 6.3 (FR-048) | Inconvenience for parents on several devices |
| A-35 | Each destination gets at most 3 codes in 15 minutes. | 6.3 (FR-049) | Legitimate retries blocked |
