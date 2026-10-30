---
trigger: model_decision
description: Command execution safety
---

# Command execution rules

- Inspect the package manifest and lockfiles before choosing a package manager or script.
  Reason: The repository's package manager and available scripts must be discovered, not guessed.
- Run only commands that exist in the repository or are documented by the project.
  Reason: Invented commands waste time and can cause unintended effects.
- Check what a command changes before running it.
  Reason: Some commands write files, install packages, contact services, or change data.
- Ask before adding, removing, or upgrading dependencies.
  Reason: Dependency changes affect security, compatibility, and project maintenance.
- Ask before running destructive commands or commands that may overwrite user work.
  Reason: User changes must not be lost.
- Follow the database-change approval rule in `database.md` before running any command that changes database state.
  Reason: Database changes require explicit authorization.
