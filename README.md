# Feedesk

> **A free, mobile-first web application for community school fee tracking, ledger reconciliation, and verified parent payments.**

[![Status](https://img.shields.io/badge/Status-Active%20Development-blue.svg)](#)
[![Stack](https://img.shields.io/badge/Stack-Next.js%20%7C%20TypeScript%20%7C%20Prisma%20%7C%20PostgreSQL-0070f3.svg)](#)
[![Payments](https://img.shields.io/badge/Payments-Flutterwave-orange.svg)](#)
[![Security](https://img.shields.io/badge/Auth-Passwordless%20OTP-success.svg)](#)

---

## 📌 Overview

Feedesk connects community schools' approved financial records directly to parents and guardians. It bridges the gap between spreadsheet/paper-ledger bursary accounting and what parents see on their mobile devices, eliminating duplicate payments, missing bank transfer references, and bursary call congestion during school term resumption.

### Core Guarantee: Absolute Financial Truth
* **Zero Client Calculations:** All balances and fee sums are computed strictly on the server using signed integer arithmetic (minor units / kobo) over approved ledger entries.
* **Separation of Payment States:** Gateway confirmation, statement reconciliation, ledger application, and receipt issuance are distinct lifecycle states. A gateway success is never displayed as "Paid" until bursary staff reconcile and apply it.
* **Dual-Control Approval (Maker-Checker):** Sensitive actions—such as approving import batches, posting ledger corrections, publishing fee schedules, and issuing clearance records—require independent staff authorization.

---

## 🚀 Key Features

### 👨‍👩‍👧‍👦 Parent Portal (Mobile-First)
1. **My Children's Fees:** Real-time fee breakdowns (charges, discounts, payments, balance) per student and combined family totals across multiple children.
2. **Payment Status & Receipts:** Track status across initiated, gateway-confirmed, matched, and applied states. Official school receipts are displayed only when formally issued.
3. **Pay School Fees:** Secure card checkout via Flutterwave or bank transfer declarations quoting unique payment references.
4. **Previous Term Statements:** Access verified historical statements, receipts, and clearance records for prior academic sessions.
5. **Ask the School:** Direct support tickets with unique tracking references assigned directly to designated bursary staff.

### 🏫 Staff Administration & Bursary
* **Versioned CSV/XLSX Imports:** Pre-validated import workflows for student rosters, fee schedules, opening ledgers, bank statements, and gateway settlements.
* **Payment Reconciliation:** Exact matching engine pairing bank statement lines to declared transfers and settlement lines to card transactions.
* **Atomic Application & Receipting:** Posts ledger credits and generates sequential receipts in a single database transaction.
* **Ledger Corrections:** Reversal-based correction requests preserving full audit trails. Historical ledger records are append-only and never modified or deleted.
* **Day-One Setup Wizard:** 8-step gate ensuring opening ledgers and payment settings are verified before opening portal access to parents.

---

## 🎨 Design System & Mobile Optimization

The application follows an accessible, Material-aligned design system optimized for budget smartphones (<300 KB total page weight, <150 KB initial JS) and intermittent network connections:
* **Brand Foundation:** Primary brand palette centered around deep clinic/education navy (`#23257D` / `#0F4C81`).
* **Semantic Roles:** Two-tier token architecture separating primitive reference palettes (`--ref-*`) from contextual system roles (`--sys-color-*`).
* **Tinted Elevation:** Shadows tinted with primary brand hue rather than flat black drop shadows.
* **Typography:** Strict Roboto scale spanning Display, Headline, Title, Body, and Label styles with tuned line-height multipliers (1.4–1.6).

Design system tokens and deliverables are located in [`task-01-design-system/`](./task-01-design-system/).

---

## 🔒 Security & Identity Model

* **Passwordless Parent Authentication:** Parents sign in strictly via 6-digit One-Time Codes (OTP) sent to verified phone numbers or email addresses. No parent passwords are ever stored or used.
* **Approved Guardian Links:** Parent accounts must link to students using school-issued, single-use link codes that expire in 7 days and require bursary staff approval before private records can be accessed.
* **Role-Based Staff Access with 2FA:** Staff sign-in requires TOTP second-factor authentication for sensitive roles (`OWNER`, `BURSAR`, and any role holding financial approval permissions).
* **Multi-Tenant Isolation:** All database tenant queries enforce strict `schoolId` scoping on the server.
* **Tamper-Proof Audit Logging:** Append-only audit log recording actor identity, IP hash, user agent, before/after JSON snapshots, and request IDs on every financial or administrative change.

---

## 🛠️ Technology Stack

| Layer | Technology | Details |
| :--- | :--- | :--- |
| **Frontend & API** | Next.js (App Router), TypeScript | Server Components, Server Actions, Route Handlers |
| **Database** | PostgreSQL | Multi-tenant schema with append-only ledger & audit tables |
| **ORM** | Prisma | Strongly typed client with raw SQL bindings for vector operations |
| **Vector Engine** | `pgvector` | 1,536-dimensional exact similarity search over approved, published shared records |
| **Payments** | Flutterwave | Server-side transaction verification, webhook signatures, and settlement parsing |
| **Styling** | CSS Custom Properties & Design Tokens | Token-based variables, fully mobile-responsive |

---

## 📁 Repository Structure

```text
├── .agent/                  # Antigravity agent rules, guidelines, and skills
│   ├── rules/               # Architecture, payments, ledger, DB, and auth guardrails
│   └── skills/              # Workflows for imports, reconciliation, and search eval
├── docs/                    # Formal product documentation
│   └── feedesk_prd_v1.1.md  # Comprehensive Product Requirements Document (PRD v1.1)
├── task-01-design-system/   # Design system tokens, Figma exports, and evidence
│   ├── docs/                # Design tokens JSON, business brief, and prompt records
│   ├── evidence/            # Export proofs, palette pins, and screen screenshots
│   └── readme.md            # Task 1 design system milestone summary
├── AGENTS.md                # Agent instructions and system guardrails
└── README.md                # Repository landing documentation
```

---

## 📄 License & Governance

Feedesk is built as a free, open-access solution for community education fee administration. Financial data privacy and security comply with Nigerian Data Protection Act (NDPA) principles.
