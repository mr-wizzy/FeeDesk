---
trigger: model_decision
description: Repository organization
---

# Repository structure rules

- Inspect the repository before creating, moving, or renaming project files.
  Reason: The current application structure has not been confirmed.
- Follow existing folder and naming conventions when they do not conflict with the PRD or root `AGENTS.md`.
  Reason: Consistency makes the codebase easier to navigate.
- Do not assume that the project uses `src/`, a monorepo, or a particular feature-folder layout.
  Reason: These choices are not established by the product requirements.
- Do not create a new folder or reorganize existing files only to match an example.
  Reason: Unnecessary restructuring adds risk without improving the product.
