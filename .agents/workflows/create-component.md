---
description: Steps and rules to follow when designing and creating React components
---

# Workflow: Create react Component

## Phsae 0: Variables and globals

Refer to these variables when needed

- `components_dir`: `/src/components/` or `./components/`
- `component_libray`: shadcn
- `animation_library`: Gsap
- `new_components_dir`: `/src/components/CustomComponents/`
- `styles_file`: `globals.css`

---

## Phase 1: Pre-Flight Planning & Source Audit

Before writing any code:

1. **JSON Pre-Flight Plan:** Output the JSON plan schema as required by `rules/workflow.md`. Audit relevant sources (`AGENTS.md`, `rules/new-react-components.md`, `rules/documentation.md`).
2. **Component Inventory Check (MANDATORY):** Search `components_dir` for an existing component to reuse/extend before proposing a new one. Update consumers if changing an existing API.
3. **`component_libray` Check (MANDATORY):** Prefer `component_libray` components over custom building.
4. **SEO Advisor Check:** Use semantic elements if the component has SEO impact.
5. **Skill loading:** Based on user requests load relevant skills.

---

## Phase 2: Create the component

1. **Directory Placement:** Create component folder in the [new component dir](@/AGENTS.md#project-context--metadata) and categorize them by the page that the component is used in (e.g., [new component dir](@/AGENTS.md#project-context--metadata)).
2. **Code:**

- Create the component using semantic HTML, Exclusively use [Style file dir](@/AGENTS.md#project-context--metadata), Create separate versions of the component for mobile and tablet saving them as `[ComponentName]Tablet` and `[ComponentName]Mobile` and configuring so each one is rendered on the related screen.
- Variables and tailwind tokens, use Tailwind logical properties.
- Merge classes with `cn(...)` (`rules/new-react-components.md`).
- Use [Animation library](../../AGENTS.md#project-context--metadata) for animations.
- If [Framework & Version](../../AGENTS.md#project-context--metadata) is NextJS use their own optimized components instead of normal ones (e.g `<Link>` instead of `<a>`)

1. **Rendering Strategy (RSC vs. Client):** Read [new component rule](@/rules/new-react-components.md). If needed separate client components from the created code into their own files and store them.
2. **Caching Configuration:** Setup caching. If the component is cacheable, configure it to be cached properly to respect Incremental Static Regeneration (ISR).

---

## Phase 3: Implementation & Styling (UI, UX, and i18n)

1. **Loading States (MANDATORY):** Create skeleton components matching content dimensions and `<Suspense>` boundaries. Store it as `[component name]Skeleton` (`[component name]TabletSkeleton`, `[component name]MobileSkeleton` for tablet and mobile).
2. **i18n & Formatting:** Create dictionary files of [Supported Languages](../../AGENTS.md#project-context--metadata) and replace the text in components. Use `Intl` API for formatting (`rules/i18n.md`).
3. **SEO:** Implement SEO for all [Supported Languages](../../AGENTS.md#project-context--metadata) per [seo.md](../rules/seo.md).

---

## Phase 4: Documentation & Test Generation

1. **TSDoc Standard (MANDATORY):** Document all exported functions, generics, inputs, and defaults in English using strict TSDoc (`rules/documentation.md`). Focus on the "Why".
2. **Algorithmic Documentation:** Update `README.modules.md` and `ARCHITECTURE.md` as necessary (`rules/documentation.md`).
3. **Unit Testing (MANDATORY):** Autonomously generate unit tests in the same directory (`rules/documentation.md`).

---

## Phase 5: Self-Reflection & Quality Audit (MANDATORY)

Execute an explicit Self-Reflection Pass and output the `### Self-Reflection & Quality Audit` block (`rules/workflow.md`) evaluating:

1. **Adversarial Checks:** Test responsive behavior (mobile/tablet/desktop breakpoint leaks), skeleton rendering during streaming, and missing prop handling.
2. **Component Rule Checklist:**
   - [ ] RSC Boundary: Is `"use client"` pushed down to leaf interactive nodes only?
   - [ ] Styling: Are semantic color tokens used (`bg-background`) instead of hardcoded tailwind colors?
   - [ ] RTL/i18n: Are Tailwind logical properties (`ms-`, `pe-`) used exclusively?
   - [ ] TSDoc: Are `@typeParam`, `@param`, `@defaultValue`, `@returns`, `@throws` present in English?
3. **Identified Bugs & Fixes:** Document any layout shifts, missing ARIA tags, or type errors found and corrected.

---

## Phase 6: Verification & Cleanup

1. **Verification Gate:** Do NOT manually run checks. Rely on `pre-commit` hooks, or use the `/verify-build` workflow if manual verification is requested.
2. **Code Cleanup:** Remove or mark temporary mocks (`// TODO: REMOVE BEFORE PRODUCTION`).
3. **Privacy:** Ensure GDPR/Privacy compliance if collecting telemetry.

---

## Phase 6: Handoff & User Commits

1. **Handoff:** Present completed work. Do NOT run git commit commands. Leave staging and committing to the user.

Provide the following out put

1. name of the component or page created
2. directory it was saved
3. rendering and caching
4. Dependencies
5. Data and API dependencies
6. Data mutations and actions