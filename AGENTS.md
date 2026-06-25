# Agent Directives & Context Routing

## Project Context & Metadata

*This dashboard serves as the single source of truth for the AI agent regarding the active project's architecture, dependencies, and business rules.*

* **Project Name**: `[Project Name]`
* **Framework & Version**: `[e.g., Next.js 14+ (App Router)]`
* **CMS Integration**: `[e.g., Payload CMS 3.0 (Self-hosted inside Next.js) / None]`
* **Primary Database**: `[e.g., PostgreSQL (via Prisma / Drizzle) / MongoDB]`
* **Supported Languages**: `[e.g., English (LTR), Persian (RTL bidirectional support)]`
* **Package Manager**: `[e.g., Bun]`
* **Primary Styling Tool**: `[e.g., Tailwind CSS / Vanilla CSS]`
* **Hosting & Infrastructure**: `[e.g., Docker Container deployed to VPS via Caprover]`
* **CI/CD Pipelines**: `[e.g., GitHub Actions (ci.yml for pull requests, cd.yml for Semantic Release and Docker builds)]`
* **new component dir**: `[e.g., ./components/custom components]`
* **component library**: `[e.g., Shadcn]`
* **Client data file**: `[e.g.,@/constants/clientInfo.ts]`
      includes
      1. Social media links, phone numbers, and addresses.
      2. Business details and contact info.
* **project info file**: `[e.g.,@/constants/projectInfo.ts]`
      Read from `docs/sitemap-architecture.md` includes
     1. Page Name: `[e.g., Home, Blog Post]`
      2. Route Path: `[e.g., / or /blog/[slug]]`
      3. Page metadata and Schema.org attributes.
      4. Access Level / Auth Requirement
      5. Redirects & Rewrites
      6. Dynamic Route Parameters
      7. Global website metadata.
* **Style file dir**: `[e.g., @/globals.css]`
* **Animation library**: `[e.g., GSAP]`

### Workspace Documentation References

* **Client Discovery Doc**: [docs/client-requirements.md](file:///docs/client-requirements.md) *(Form answers from Phase 0)*
* **Technical Stack Doc**: [docs/tech-stack.md](file:///docs/tech-stack.md) *(Architecture choices from Phase 1)*
* **Sitemap & Caching Spec**: [docs/sitemap-architecture.md](file:///docs/sitemap-architecture.md) *(Route maps and delivery profiles from Phase 2)*

---
**CRITICAL: SKILL ROUTING**

Before you begin planning or executing a task, you MUST evaluate the user's request. If the request involves any of the following domains, use your file system access to read the corresponding skill file in the `.agents/skills/` directory BEFORE writing code:

1. **Animations & Transitions**: Read `.agents/skills/gsap-**.md` if dosen't exist install with `bunx skills add https://github.com/greensock/gsap-skills`
2. **UI Components & Styling**: Read `.agents/skills/shadcn/**` if dosen't exist install with `bunx --bun skills add shadcn/ui`

## Skill Loading

Before substantial work:

* Skill check: run `bunx @tanstack/intent@latest list`, or use skills already listed in context.
* Skill guidance: if one local skill clearly matches the task, run `bunx @tanstack/intent@latest load <package>#<skill>` and follow the returned `SKILL.md`.
* Monorepos: when working across packages, run the skill check from the workspace root and prefer the local skill for the package being changed.
* Multiple matches: prefer the most specific local skill for the package or concern you are changing; load additional skills only when the task spans multiple packages or concerns.
* Ensure TanStack Intent is used to load version-accurate skills for installed packages: `bunx @tanstack/intent@latest install`.

# Setting up enviroment

If `pre-commit` is not installed install it also if the skills and the listed files are missing prompt user to add them in pre-flight

# Agent Instructions & Project Standards (Decentralized)

To reduce context pollution and ensure the agent only loads rules relevant to the active request, the detailed project standards have been split into individual rule files located under the `.agents/rules/` directory:

1. **Core Stack Standards** ([core-stack.md](.agents/rules/core-stack.md)):
   Contains guidelines for Next.js rendering strategies, Payload CMS integration, Bun package manager execution, and end-to-end TypeScript safety.
2. **Architecture & File Structure** ([architecture.md](.agents/rules/architecture.md)) [MANDATORY when creating a new page or component]:
   Defines global navigation structures, component import/placement rules, and React Server Components (RSC) vs. Client Component choices.
3. **UI, Styling & Animations** ([ui-styling.md](.agents/rules/ui-styling.md)) [Loading States section is MANDATORY when creating a new page or component]:
   Sets standard tailwind properties, class name merging (`cn()`), RTL physical vs. logical styles, loading skeletons, GSAP configuration, and asset optimization.
4. **Search Engine Optimization** ([seo.md](.agents/rules/seo.md)):
   Covers semantic HTML, dynamic sitemap updates, JSON-LD structured data (schema-dts), and robots indexation control.
5. **Language & Internationalization** ([i18n.md](.agents/rules/i18n.md)):
   Covers Edge-level Persian (i18n) middleware routing, bidirectional (RTL) HTML structures, localized link formatting utils, and Intl localization formatters.
6. **Workflow & Task Management** ([workflow.md](.agents/rules/workflow.md)) [MANDATORY for all tasks]:
   Outlines the Chain of Thought planning format, branch/commit naming conventions, type/lint checks, semantic versioning, and compliance policies.
7. **Code Documentation & Testing** ([documentation.md](.agents/rules/documentation.md)) [Inline Documentation & TSDoc section is MANDATORY]:
   Specifies TSDoc parameters, automated unit testing guidelines (Jest/Vitest/Bun), Mermaid syntax, and localized README layouts.

*Trigger conditional rules individually using the `model_decision` loader based on the topic of the current prompt.*
