Task 1: Design System & Agent Proof — Completion Summary
1. Business Brief (docs/business-brief.md)
Business: Private multi-specialty medical clinic handling appointments, prescriptions, and diagnostic records.

Persona & Feel: Outpatients (25–65) requiring a reassuring, high-contrast, clean medical blue interface.

2. Colour System (docs/colour-roles.md, evidence/)
Pinned brand color (#0F4C81) as Primary 40 in Figma (evidence/01-palette-before-pin.png, evidence/02-palette-after-pin.png).

Generated tonal palettes, functional errors/warnings, and semantic roles (surface, primary, on-surface).

Audited and documented contrast ratios for all active text-background pairs.

3. Typography & Divergences (docs/type-scale.md, docs/divergences.md)
Created 15 styles (Display, Headline, Title, Body, Label) using precise line-height multipliers (1.4–1.6).

Captured spacing adjustments (evidence/03-spacing-default.png, evidence/04-spacing-chosen.png).

Implemented two deliberate Material baseline divergences, documenting their trade-offs and production failure points.

4. Shadows (docs/shadows.md)
Built soft, medium, and hard shadows tinted with the primary brand color instead of flat black.

5. Token Export (evidence/05-tokens-incomplete.json, evidence/06-tokens-complete.json)
Captured the initial export omission (missing variables) and the corrected full export, with a one-line diff note.

6. CSS Conversion (docs/prompts.md)
Prompted an agent to convert token JSON into CSS variables enforcing primitive/role separation.

7. Agent Consumption Proof (evidence/07 to evidence/10)
Ran a raw-value codebase search (evidence/07-raw-value-search.txt) confirming zero hardcoded colors.

Verified typography usage with the agent (evidence/08-agent-typography-answer.txt).

Compared default UI (evidence/09-screen-without-system.png) against system-governed UI (evidence/10-screen-with-system.png).

8. Deliberate Regression (evidence/11, evidence/12)
Broke token referencing on a branch, captured the UI failure (evidence/11-broken-referencing.png), and deleted the unmerged branch (evidence/12-git-log-branch-unmerged.txt).

9. Written Deliverables (docs/)
Completed all 12 viva defence answers (docs/interview-answers.md) and logged real code defects (docs/defects.md).