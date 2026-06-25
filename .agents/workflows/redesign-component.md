---
description: Rules and steps to follow when trying to redesign a react component without changing its logic or core functionality
---

# Workflow: Redesign Component

This workflow defines the step-by-step process and rules to follow when refactoring or redesigning the visual styling, theme, layout, or animations of an existing React component in the codebase, without altering its core business logic or API signature.

---

## Phase 1: Pre-Flight & Planning

Before modifying any styling or markup:

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

1. **Chain of Thought Planning:** Plan the visual changes, styling strategy, and animation pipelines. Output a numbered list of steps and wait for user approval.
2. **Behavior Analysis:** Analyze the existing component's prop interface, callbacks, hooks, and internal state. The redesign **must not** break existing event handling, state progression, or consumer integration.
3. **Shadcn Component Check (MANDATORY):** If the redesign benefits from replacing parts of custom code with standard components (e.g., replacing a custom dropdown/popup with a standard dialog or popover), check if Shadcn UI has a component that meets your needs. If a Shadcn component is available, you **MUST** use/add that component using the `shadcn` skill instead of styling a custom component from scratch. If the `shadcn` skill is missing, install it by running `bunx --bun skills add shadcn/ui`.
4. **GSAP Skills Check (MANDATORY):** If the redesign introduces complex animations using GSAP, you **MUST** read the appropriate local GSAP skill files (e.g., `gsap-core`, `gsap-react`, `gsap-scrolltrigger`, etc. in `.agents/skills/`) to guide the implementation. If the GSAP skills are missing, run `bunx skills add https://github.com/greensock/gsap-skills` to install them.
5. **RSC vs. Client Boundary Check:** 
   - Check if the component is currently a React Server Component (RSC) or a Client Component.
   - Maintain its existing rendering strategy. If it is an RSC, keep it as one.
   - **Adding Interactivity/Animations:** If adding complex animations (GSAP) or client-side interactivity to an RSC, do not turn the entire component into a Client Component. Instead, isolate the animated/interactive elements, extract them into a leaf Client Component, and render/compose them inside the Server Component.

---

## Phase 2: Implementation & Styling (UI, UX, and i18n)

### 1. Styling & Tailwind CSS
- **Theme Variables:** Exclusively use Shadcn UI's CSS variable system (e.g. `bg-background`, `text-foreground`, `border-border`). Do **NOT** use hardcoded Tailwind color scales (e.g., `bg-blue-500`) unless explicitly requested. Ensure excellent color contrast in both Light and Dark modes.
- **RTL Logical Properties:** Use Tailwind's direction-aware logical classes (e.g. `ms-*` instead of `ml-*`, `pe-*` instead of `pr-*`, `inset-inline-start` instead of `left`). Physical directional classes (`ml`, `mr`, `pl`, `pr`, `left`, `right`) are **FORBIDDEN**.
- **Class Merging:** Always use the `cn(...)` utility function for class name concatenation to avoid styling conflicts.

### 2. Loading Skeletons & CLS Prevention
- Ensure that the redesign does not alter the layout dimensions in a way that causes Cumulative Layout Shift (CLS).
- If the layout changes, update the corresponding Shadcn `<Skeleton>` fallbacks to match the new dimensions exactly.

### 3. Responsive Layouts
- The redesigned component must be responsive across all devices (mobile, tablet, and desktop).
- If necessary, split the visual presentation into internal layout variations to preserve UX quality on small screens.

### 4. Animations (GSAP & CSS)
- **Micro-interactions:** Use standard Tailwind CSS transitions for simple hover states, focus rings, or toggle states.
- **Complex Sequences (GSAP):**
  - Use [GSAP](https://gsap.com/) and the `@gsap/react` `useGSAP()` hook for complex sequence timelines or scroll-triggered animations.
  - target animated DOM elements strictly using `ref`s to avoid direct virtual DOM mutations.
  - Scoped Selectors: Use `useGSAP` scope parameter to scope selectors and prevent memory leaks.
  - Ensure animations do not negatively impact Core Web Vitals or SEO.

### 5. Asset Optimization
- **Images:** Exclusively use Next.js `<Image>` components instead of `<img>` tags. Preserve or add appropriate `priority` props for images above the fold.
- **SVGs/Fonts:** Small assets must be hosted locally.

---

## Phase 3: SEO & Semantic HTML Integrity

- Ensure that layout changes do not break semantic HTML hierarchies. Keep proper heading nesting (sequential `<h2>` to `<h6>`) and ensure there is only one `<h1>` per page.
- Do not alter or break existing JSON-LD schemas, alt texts, or meta tags embedded in the component.

---

## Phase 4: Documentation & Test Verification

### 1. Documentation Preservation & Updates
- Do not remove or corrupt existing TSDoc annotations.
- If the redesign introduces helper functions, generic types, or props changes, document them with strict TSDoc according to [.agents/rules/documentation.md](file:///d:/Scripts/sampleshop/.agents/rules/documentation.md).
- **English Language Requirement:** All TSDoc comments, inline comments, and markdown documentation files (such as `README.modules.md` and `ARCHITECTURE.md`) MUST be written exclusively in **English**, regardless of the application's locale or target language (e.g., Persian/fa).
- You MUST document:
  - **Generics (`@typeParam`):** Write an individual tag for *every single* generic type parameter. Explain its constraints, purpose, and usage.
  - **Inputs (`@param`):** Write an individual tag for *every single* input parameter. Detail the parameter's name, type, constraints, and semantic description.
  - **Defaults (`@defaultValue`):** Write an individual tag for *every* optional parameter, config option, or component prop property to specify its default value.
  - **Outputs (`@returns`):** Write a detailed description of the return type/element, its meaning, and how conditional rendering or return scenarios behave.
  - **Exceptions (`@throws`):** Write an individual tag for *each* specific error that can be thrown by page helpers or functions, describing the conditions under which it occurs and potential recovery steps.
- **README.modules.md:** Update the localized `README.modules.md` (or module readme) if visual state flows or Mermaid interaction diagrams are impacted.
- **ARCHITECTURE.md:** Update the root `ARCHITECTURE.md` diagram if the component introduces new external dependencies.

### 2. Test Verification (MANDATORY)
- Do NOT run tests manually during redesign, as they are executed automatically via `pre-commit` hooks. For manual verification/testing or fixing test regressions, run the `/verify-build` workflow.
- If styling structure changes broke testing selectors (e.g. `data-testid` or query selectors), fix the selectors in the tests so they align with the new markup while keeping assertions intact.

---

## Phase 5: Verification & Cleanup

1. **Verification Gate:**
   - Do NOT manually run build checks, linting, or tests. These verification steps are automatically run via `pre-commit` hooks during staging/commit/push.
   - If manual verification or build fixing is requested, run the `/verify-build` workflow.
2. **Code Cleanup:**
   - Remove any temporary test styling or console logs. Mark remaining placeholders with `// TODO: REMOVE BEFORE PRODUCTION`.
3. **Versioning:**
   - Modify the version number in `package.json` utilizing `bun version patch` (or `minor`/`major` where appropriate) on completed features. Let the user include this in their commit.

---

## Phase 6: Handoff & User Commits

1. **Handoff:**
   - Present the completed work clearly to the user, highlighting the files created or modified.
   - Do NOT run git commit or git add commands, and do not block progress waiting for commits. Leave all staging, committing, and branching/pushing operations entirely to the user.
   - (Optional) You may suggest appropriate conventional commit messages for their reference, but do not execute them.
