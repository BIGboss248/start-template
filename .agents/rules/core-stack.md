---
trigger: model_decision
description: Standards for Next.js app router, Payload CMS integration, Bun package manager, and end-to-end type safety.
---

# Core Technologies & Frameworks

## Next.js

* **Source of Truth:** ALWAYS read the relevant documentation in `node_modules/next/dist/docs/` before coding. Your training data may be outdated; the local documentation is the absolute source of truth.
* **Deployment Readiness:** Write code assuming a containerized deployment architecture. Ensure the Next.js config utilizes `output: 'standalone'` and that the application gracefully handles dynamic environment variables and reverse proxy headers (e.g., trusting proxy IPs, handling custom SSL terminations).

## Payload CMS

* **Documentation:** [Payload CMS docs](https://payloadcms.com/docs/)
* **Usage:** The website uses Payload CMS to manage all content (including blogs). This encompasses adding, removing, searching, and structuring dynamic content.
* **Database Schemas:** When designing database schemas, prioritize modular, relational structures capable of handling complex entities (like product variations or multi-language fields).

## Package Manager

* Project uses **Bun** as the package manager. Always use `bun` commands instead of `npm` or `yarn`.

## End-to-End Type Safety

* Maintain strict TypeScript adherence. Any changes to Payload CMS collections MUST be followed by regenerating Payload's TypeScript interfaces and utilizing those generated types throughout the Next.js frontend.
