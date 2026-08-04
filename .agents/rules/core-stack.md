---
trigger: model_decision
description: Framework-agnostic standards for Bun package manager and end-to-end TypeScript safety.
---

# Core Stack Standards

## Package Manager

* Project uses **Bun** as the package manager. Always use `bun` commands instead of `npm` or `yarn`.

## End-to-End Type Safety

* Maintain strict TypeScript adherence. Any changes to CMS collections or backend models MUST be followed by regenerating TypeScript interfaces and utilizing those generated types throughout the application frontend.
