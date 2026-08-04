---
trigger: model_decision
description: Framework-agnostic standards for Bun package manager and end-to-end TypeScript safety.
---

# Core Stack Standards

## Package Manager

* Project uses **Bun** as the package manager. Always use `bun` commands instead of `npm` or `yarn`.

## End-to-End Type Safety

* Maintain strict TypeScript adherence. Any changes to Payload CMS collections MUST be followed by regenerating Payload's TypeScript interfaces and utilizing those generated types throughout the Next.js frontend.
