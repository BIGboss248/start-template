---
description: Steps and rules to follow when designing, creating, or editing React components
---

# Workflow: Create or Edit Component

This workflow defines the step-by-step process and rules to follow when creating a new React component or editing/refactoring an existing component (Server or Client) in the codebase. It integrates rules from `.agents/rules/` to ensure architectural integrity, styling consistency, accessibility, SEO, internationalization (i18n), testing, and documentation standards.

---

## Phase 1: Pre-Flight & Planning

Before writing any code:

<!-- AI SYNC NOTICE: Keep the following Skill Routing and Skill Loading sections in sync with AGENTS.md on any change -->
### Skill Routing & Loading

**CRITICAL: SKILL ROUTING**

Before you begin planning or executing a task, you MUST evaluate the user's request. If the request involves any of the following domains, use your file system access to read the corresponding skill file in the `.agents/skills/` directory BEFORE writing code:

1. **Animations & Transitions**: Read `.agents/skills/gsap-**.md` if dosen't exist install with `bunx skills add https://github.com/greensock/gsap-skills`
2. **UI Components & Styling**: Read `.agents/skills/shadcn/**` if dosen't exist install with `bunx --bun skills add shadcn/ui`

#### Skill Loading

Before substantial work:
- Skill check: run `bunx @tanstack/intent@latest list`, or use skills already listed in context.
- Skill guidance: if one local skill clearly matches the task, run `bunx @tanstack/intent@latest load <package>#<skill>` and follow the returned `SKILL.md`.
- Monorepos: when working across packages, run the skill check from the workspace root and prefer the local skill for the package being changed.
- Multiple matches: prefer the most specific local skill for the package or concern you are changing; load additional skills only when the task spans multiple packages or concerns.
- Ensure TanStack Intent is used to load version-accurate skills for installed packages: `bunx @tanstack/intent@latest install`.
<!-- END AI SYNC NOTICE -->

1. **Chain of Thought Planning:** Plan the component structure, data flow, rendering strategy, and dependencies. Output a numbered list of steps and wait for user approval.
2. **Codebase Component Inventory Check (MANDATORY):**
   - Before proposing or writing any code for a component, you **MUST** perform a codebase search (e.g., using `grep_search` or `list_dir` on `/src/components/CustomComponents/` or route directories) to see if there is an existing custom component that can perform the requested task or be adapted for it.
   - If a similar component exists, you **MUST** reuse it as-is or extend/refactor it (e.g., by adding optional props, parameters, generic types, or styling configurations) instead of creating a new one. Creating a new component is strictly prohibited if an existing component can be adapted without breaking its current consumers.
   - If you modify an existing component, search the codebase to find all consumers/imports of that component. Analyze its current prop interface and behavior to prevent unexpected breaking changes. If a change modifies the component's API, you MUST update all consumers within the same task.
3. **Shadcn Component Check (MANDATORY):** Check if Shadcn UI has a component that meets your needs (e.g., Accordion, Dialog, Dropdown, etc.). If a Shadcn component is available, you **MUST** use/add that component using the `shadcn` skill instead of creating a new custom component from scratch. If the `shadcn` skill is missing, install it by running `bunx --bun skills add shadcn/ui`. Creating a new custom component when a Shadcn component is available or can be adapted is strictly prohibited.
4. **GSAP Skills Check (MANDATORY):** If the component requires complex animations using GSAP, you **MUST** read the appropriate local GSAP skill files (e.g., `gsap-core`, `gsap-react`, `gsap-scrolltrigger`, etc. in `.agents/skills/`) to guide the implementation. If the GSAP skills are missing, run `bunx skills add https://github.com/greensock/gsap-skills` to install them.
5. **SEO Advisor Check:** If the component has SEO impact (e.g. FAQ, product grids), propose additional semantic elements or structure.

---

## Phase 2: Architectural Setup & Placement

1. **Directory Placement:**
   - Store all new custom components under `/src/components/CustomComponents/` (or structured subdirectories).
2. **Imports Standard:**
   - Always import packages and files relative to the project root using `@/` path aliases (or absolute paths). Never use relative paths like `../../components/...`.
3. **Rendering Strategy & Component Splitting (RSC vs. Client):**
   - **Default to React Server Components (RSC):** Make components cacheable Server Components by default. Fetch data, resolve metadata, and render layout/static contents on the server.
   - **Maximize RSC Coverage:** Prioritize splitting components to isolate interactivity. Never turn a large, layout-heavy component into a Client Component just because one sub-element (e.g. a button, filter, or input field) requires interactivity.
   - **Isolate Client Components (Leaf Components):**
     - Push the `"use client"` boundary as far down the component tree as possible (often to leaf components).
     - Extract interactive elements (state managers, click handlers, input forms, GSAP animations) into separate small client components.
     - Keep the parent component as a Server Component.
   - **RSC Composition Pattern:**
     - If a Client Component needs to wrap or contain Server Components (e.g. wrapper layouts, context providers, or animation container frames), pass the Server Components as `children` or standard React props.
     - Do not import Server Components directly inside a `"use client"` file, as this automatically forces the imported Server Component to run on the client, losing server-side data fetching and cacheability.
   - **ISR Readiness:** Ensure any data-fetching or caching respects Incremental Static Regeneration (ISR). Never cache user-specific or sensitive transactional data.

---

## Phase 3: Implementation & Styling (UI, UX, and i18n)

### 1. Styling & Tailwind CSS

- **Theme Tokens:** Exclusively use Shadcn UI's CSS variable system (e.g., `bg-background`, `text-muted-foreground`, `border-border`). **DO NOT** use hardcoded Tailwind colors (e.g., `bg-blue-500`) unless explicitly requested. Ensure proper contrast in both Light and Dark modes.
- **Class Merging (MANDATORY):** Never use standard template literals to concatenate Tailwind classes. Always use the `cn(...)` utility (wrapping `clsx` and `tailwind-merge`) from `@/utilities/cn` for class name merging, especially when exposing or overriding a `className` prop.
- **Logical Properties (RTL Support):** The site language is Persian (fa). You **MUST** use Tailwind's direction-aware logical classes (e.g., `ms-*` instead of `ml-*`, `pe-*` instead of `pr-*`, `start` instead of `left`, `end` instead of `right`). Physical directional classes (`ml`, `mr`, `pl`, `pr`, `left`, `right`) are **FORBIDDEN** to ensure mirroring works correctly.

### 2. Accessibility & Semantic HTML

- Use Semantic HTML tags (`<article>`, `<nav>`, `<aside>`, `<section>`, etc.) rather than generic `<div>` tags where appropriate.
- **Heading Hierarchy:** Ensure proper heading nesting (sequential `<h2>` to `<h6>`). Never have more than a single `<h1>` per page.
- Ensure screen-reader support, proper ARIA labels, and interactive states.

### 3. Internationalization (i18n) & Localized Formatting

- All textual content, labels, metadata, and JSON-LD schema must be in **Persian (fa)**.
- **Locale Management:** Components must never guess the locale. Receive it strictly via route parameters (e.g. `[locale]`) or headers from Next.js Edge Middleware.
- **Data Formatting:** Never manually format numbers, dates, or currency. Always use the native JavaScript `Intl` API:
61:   - `Intl.NumberFormat` for numbers/currency (rendering Persian numerals if needed).
62:   - `Intl.DateTimeFormat` for dates (defaulting to the Shamsi/Jalali calendar).

### 4. Loading States & Skeletons (MANDATORY)

- Create matching loading skeletons (using Shadcn's `<Skeleton>` component) for data-heavy components.
- Wrap the component in React `<Suspense>` boundaries.
- Ensure skeleton fallback dimensions exactly match the loaded content's dimensions to prevent **Cumulative Layout Shift (CLS)**.

### 5. Animations

- **Micro-interactions:** Use standard Tailwind CSS transitions for simple hover states, focus rings, or toggle states.
- **Complex Animations (GSAP):**
  - Use [GSAP](https://gsap.com/) and the `@gsap/react` `useGSAP()` hook for complex sequence timelines or scroll-triggered animations.
  - Always assign `ref`s to animated DOM elements to avoid mutating the virtual DOM directly.
  - Ensure animations do not negatively impact Core Web Vitals or SEO.

### 6. Asset Optimization

- **Images:** Exclusively use the Next.js `<Image>` component instead of standard `<img>` tags. Provide `alt` text, explicit `width`/`height` (or `fill`), and set `priority` for LCP images above the fold.
- **Local Assets:** Small assets (SVG icons, fonts) must be self-hosted locally. Use `next/font/google` or `next/font/local` to eliminate layout shift.
- **Large Assets:** Notify the user and ask them to decide whether to host large files locally or via a CDN.

---

## Phase 4: Documentation & Test Generation

### 1. TSDoc Standard (MANDATORY)

- Document all exported functions, types, props, and components using strict **TSDoc** syntax according to the guidelines in [.agents/rules/documentation.md](file:///d:/Scripts/sampleshop/.agents/rules/documentation.md).
- **English Language Requirement:** All TSDoc comments, inline comments, and markdown documentation files (such as `README.modules.md` and `ARCHITECTURE.md`) MUST be written exclusively in **English**, regardless of the application's locale or target language (e.g., Persian/fa).
- You MUST document:
  - **Generics (`@typeParam`):** Write an individual tag for *every single* generic type parameter. Explain its constraints, purpose, and usage.
  - **Inputs (`@param`):** Write an individual tag for *every single* input parameter. Detail the parameter's name, type, constraints, and semantic description. For component props, document each property of the props interface/type individually.
  - **Defaults (`@defaultValue`):** Write an individual tag for *every* optional parameter, config option, or component prop property to specify its default value.
  - **Outputs (`@returns`):** Write a detailed description of the return type/element, its meaning, and how conditional rendering or return scenarios behave.
  - **Exceptions (`@throws`):** Write an individual tag for *each* specific error that can be thrown by hook functions, helper functions, or handlers, describing the conditions under which it occurs and potential recovery steps.
- Focus comments on the **"Why"** rather than the "What"—explain the rationale and handling of edge cases.
- Use clear inline comments to explain complex logical blocks.

### 2. Localized Algorithmic Documentation (`README.modules.md`)

- Create or update the localized `README.modules.md` (or module-specific markdown file) in the component's immediate directory.
- Must include:
  1. **Algorithmic Breakdown:** Step-by-step prose explanation of how the component/algorithm works.
  2. **Execution/State Flow:** A localized Mermaid diagram showing state changes, conditional branches, or internal control loops.
  3. **Consumer Tracking:** A list of absolute file paths that import/consume this component.
  4. **Integration Graph:** A Mermaid flowchart connecting this module/component to the pages/routes that render it.
  5. **Dependencies:** External packages or specific API endpoints this module relies on.

### 3. Architecture Mapping (`ARCHITECTURE.md`)

- If the component changes the system-wide data flow, module boundaries, or introduces new dependencies, update the high-level Mermaid diagram in the root `ARCHITECTURE.md` file.
- Validate Mermaid syntax (e.g. quote labels containing special characters) to prevent errors.

### 4. Unit Testing (MANDATORY)

- Autonomously generate or update the corresponding unit test file (e.g., `[component-name].test.tsx` or `index.test.tsx`) in the same directory.
- Check `package.json` to identify the established testing framework (e.g. Jest, Vitest, Cypress) and use its syntax. If none exists, default to `bun test`.
- Do NOT run unit tests manually during component creation/editing, as they are executed automatically via `pre-commit` hooks. For manual verification/testing, run the `/verify-build` workflow.

---

## Phase 5: Verification & Cleanup

1. **Verification Gate:**
   - Do NOT manually run build checks, linting, or tests. These verification steps are automatically run via `pre-commit` hooks during staging/commit/push.
   - If manual verification or build fixing is requested, run the `/verify-build` workflow.
2. **Code Cleanup:**
   - Mark temporary mock data, placeholder UI, or debug logs with `// TODO: REMOVE BEFORE PRODUCTION`.
3. **Versioning:**
   - Modify the version number in `package.json` utilizing `bun version patch` (or `minor`/`major`) on completed features. Let the user include this in their commit.
4. **Privacy Check:**
   - If the component collects cookies, telemetry, or user data, update the Privacy Policy & Terms of Service, and implement a consent banner.

---

## Phase 6: Handoff & User Commits

1. **Handoff:**
   - Present the completed work clearly to the user, highlighting the files created or modified.
   - Do NOT run git commit or git add commands, and do not block progress waiting for commits. Leave all staging, committing, and branching/pushing operations entirely to the user.
   - (Optional) You may suggest appropriate conventional commit messages for their reference, but do not execute them.
