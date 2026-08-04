# Agent Directives & Layered Context Routing

> [!IMPORTANT]
> **SINGLE SOURCE OF TRUTH FOR AI AGENT**: This file acts as the universal entry point and routing hub for the AI agent. Project-specific metadata is isolated in `.agents/PROJECT.md` so that central agent rules (`.agents/core/`) can be synced via Git Submodule without touching project settings.

## Project Context & Metadata Binding (Solution 1)

*Before planning or executing tasks, read the active project's specific architecture, database, framework, and client details from:*
👉 **[PROJECT.md](file:///.agents/PROJECT.md)** *(Isolated project data — preserved across rule updates)*

---

## Layer Precedence & Evaluation Order (Solution 4)

When evaluating instructions, rules, and workflows, the AI agent MUST resolve conflicts using the following precedence hierarchy:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ LEVEL 1: System Defaults (~/.gemini/config/AGENTS.md)                       │
│  └── User-wide preferences across all projects                              │
├─────────────────────────────────────────────────────────────────────────────┤
│ LEVEL 2: Core Directives (.agents/core/rules/ & .agents/core/workflows/)    │
│  └── Universal rules & workflows synced via Git Submodule                   │
├─────────────────────────────────────────────────────────────────────────────┤
│ LEVEL 3: Project Specifications (.agents/PROJECT.md)                        │
│  └── Project context, active database, framework, client info, site map      │
├─────────────────────────────────────────────────────────────────────────────┤
│ LEVEL 4: Project Overrides (.agents/overrides/rules/ & /workflows/)        │
│  └── HIGHEST PRECEDENCE: Local overrides & appends for this specific project│
└─────────────────────────────────────────────────────────────────────────────┘
```

### Override & Append Evaluation Policy

Before reading a core directive from `.agents/core/`:
1. **Check for Override**: Check `.agents/overrides/rules/` (or `.agents/overrides/workflows/`). If a file with the **exact same filename** exists, load the override version instead of the core version.
2. **Check for Appends**: Load any additional custom rules or workflows located in `.agents/overrides/`.

---

## Critical: Skill Routing

Before you begin planning or executing a task, you MUST evaluate the user's request. If the request involves any of the following domains, use your file system access to read the corresponding skill file in the `.agents/skills/` directory BEFORE writing code:

1. **Animations & Transitions**: Read `.agents/skills/gsap-**.md` (if doesn't exist, install with `bunx skills add https://github.com/greensock/gsap-skills`).
2. **UI Components & Styling**: Read `.agents/skills/shadcn/**` (if doesn't exist, install with `bunx --bun skills add shadcn/ui`).

## Skill Loading

Before substantial work:
* **Skill check**: Run `bunx @tanstack/intent@latest list`, or use skills already listed in context.
* **Skill guidance**: If one local skill clearly matches the task, run `bunx @tanstack/intent@latest load <package>#<skill>` and follow the returned `SKILL.md`.
* **Monorepos**: When working across packages, run the skill check from the workspace root and prefer the local skill for the package being changed.
* **Multiple matches**: Prefer the most specific local skill for the package or concern you are changing; load additional skills only when the task spans multiple packages or concerns.
* **TanStack Intent**: Ensure TanStack Intent is used to load version-accurate skills for installed packages: `bunx @tanstack/intent@latest install`.

## Setting Up Environment

If `pre-commit` is not installed, install it. Also, if skills or listed context files are missing, prompt the user to add them during pre-flight planning.

---

# Agent Instructions & Project Standards (Decentralized)

To reduce context pollution, project standards and step-by-step checklists are split into **Universal (Core)** and **Framework-Specific** directives:

## 1. Universal Rules & Workflows (Framework-Agnostic)

*Located in `.agents/core/rules/` and `.agents/core/workflows/` (overridden by `.agents/overrides/` if present).*

### Universal Rules
1. **Workflow & Task Management** ([workflow.md](.agents/core/rules/workflow.md)) [MANDATORY for all tasks]:
   Outlines the JSON Pre-Flight Plan & Source Audit requirements, Self-Reflection pass, branch/commit naming conventions, type/lint checks, and compliance policies.
2. **Core Stack Standards** ([core-stack.md](.agents/core/rules/core-stack.md)):
   Framework-agnostic standards for Bun package manager execution and end-to-end TypeScript safety.
3. **New React Components** ([new-react-components.md](.agents/core/rules/new-react-components.md)) [MANDATORY when creating components, pages, or layouts]:
   Framework-agnostic guidelines for component reuse, placement, Tailwind logical styling (`ms-`, `pe-`), responsiveness, loading states, and commenting.
4. **Search Engine Optimization** ([seo.md](.agents/core/rules/seo.md)):
   Covers semantic HTML, dynamic sitemap updates, JSON-LD structured data (schema-dts), and robots indexation control.
5. **Language & Internationalization** ([i18n.md](.agents/core/rules/i18n.md)):
   Covers Edge-level Persian (i18n) middleware routing, bidirectional (RTL) HTML structures, localized link formatting utils, and Intl localization formatters.
6. **Code Documentation & Testing** ([documentation.md](.agents/core/rules/documentation.md)) [Inline Documentation & TSDoc section is MANDATORY]:
   Specifies TSDoc parameters, automated unit testing guidelines (Jest/Vitest/Bun), Mermaid syntax, and localized README layouts.

### Universal Workflows
- [**/create-component**](.agents/core/workflows/create-component.md): Universal checklist for component creation.
- [**/create-utility**](.agents/core/workflows/create-utility.md): Universal checklist for TypeScript utilities and functions.
- [**/redesign-component**](.agents/core/workflows/redesign-component.md): Universal checklist for component refactoring and redesign.
- [**/verify-build**](.agents/core/workflows/verify-build.md): Universal verification workflow for running builds and fixing errors.

---

## 2. Framework-Specific Rules & Workflows (Dynamic Routing)

*Inspect **`Framework & Version`** in [.agents/PROJECT.md](file:///.agents/PROJECT.md). Load ONLY the matching framework directive below and ignore non-active framework files.*

### Framework Rules (`.agents/core/rules/frameworks/`)
- **Next.js**: [nextjs.md](.agents/core/rules/frameworks/nextjs.md) [MANDATORY when working in Next.js]:
  Contains guidelines for Next.js App Router, React Server Components (RSC), Payload CMS 3.0, ISR caching, and Next.js optimized components (`<Link>`, `<Image>`).

### Framework Workflows (`.agents/core/workflows/<framework>/`)
- **Next.js Component Workflow**: [create-component.md](.agents/core/workflows/nextjs/create-component.md):
  Extends component creation with Next.js specific steps (RSC boundary checks, ISR caching setup, `@vercel/react-transition-progress`).

*Trigger conditional rules individually using the `framework_match` or `model_decision` loader based on the active framework and prompt topic.*
