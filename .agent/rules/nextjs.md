---
trigger: model_decision
description: Next.js application implementation
---

# Next.js application rules

- Use the Next.js App Router. Follow the patterns already present in the repository.
  Reason: The project uses Next.js, but the repository's exact layout is not yet verified.
- Keep server-only operations out of client components.
  Reason: Browser code is visible and controlled by the user.
- Keep page and component code focused on presenting data and collecting user input.
  Reason: This makes the application easier to maintain and keeps business operations out of the interface.
- Use the existing route, server action, and error-handling patterns. Do not create a second pattern for the same task.
  Reason: Consistent application structure reduces defects.
- Meet the PRD's mobile-first page and error-state requirements.
  Reason: Parents and staff need usable screens on mobile devices and clear feedback when requests fail.
