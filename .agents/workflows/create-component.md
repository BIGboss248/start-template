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

## Phase 1: Pre-Flight & Planning

Before writing any code:

1. **Chain of Thought Planning:** Plan the component structure, data flow, rendering strategy, and dependencies.
2. **Component Inventory Check (MANDATORY):** Search `components_dir` for an existing component to reuse/extend before proposing a new one. Update consumers if changing an existing API.
3. **`component_libray` Check (MANDATORY):** Prefer `component_libray` components over custom building.
4. **SEO Advisor Check:** Use semantic elements if the component has SEO impact.

Show the plan in this form:

```md

# Component Porperties

[Are we using an existing component or a shadcn component or are we creating a new custom component]

- Component dir: [Where the component will be saved]
- Cache: [Is component cachable?]
- Rendering strategy: [SSG, ISR, SSR, CSR]
- Access Level / Auth Requirement: `[e.g., Public, Authenticated User, Admin-Only]`
- Data & API Dependencies: `[e.g., None, Payload CMS 'blog' collection, Stripe API]`
- Redirects & Rewrites: `[e.g., None, redirect unauthenticated to /login]`
- Data Mutations & Actions: `[e.g., None, Submit contact form Server Action]`

# Packages

[Numbered list of packages this component will use]

```

---

## Phase 2: Create the component

1. **Directory Placement:** Create component folder in the [new component dir](@/AGENTS.md#project-context--metadata) and categorize them by the page that the component is used in (e.g., [new component dir](@/AGENTS.md#project-context--metadata)).
2. **Code:**

- Create the component using semantic HTML, Exclusively use [Style file dir](@/AGENTS.md#project-context--metadata), Create seprate versions of the component for mobile and tablet saving them as [ComponentName]Tablet  and [ComponentName]Mobile and configuring so each one is rendered on the related screen
- variables adn tailwind tokens, use Tailwind logical properties.
- Merge classes with `cn(...)` (`rules/ui-styling.md`).
- Use [Animation library](../../AGENTS.md#project-context--metadata) for animations.
- If [Framework & Version](../../AGENTS.md#project-context--metadata) is NextJS use their own optimized components instead of normal ones (e.g `<Link>` instead of `<a>`)

1. **Rendering Strategy (RSC vs. Client):** Read [new component rule](@/rules/new-react-components.md) If needed separate client components from the created code into their own files and store them
2. **Caching Configuration:** Setup caching. If the component is cacheable, configure it to be cached properly to respect Incremental Static Regeneration (ISR).

---

## Phase 3: Implementation & Styling (UI, UX, and i18n)

1. **Loading States (MANDATORY):** Create skeleton components matching content dimensions and `<Suspense>` boundaries. And store it as `[component name]Skeleton` (`[component name]TabletSkeleton`, `[component name]MobileSkeleton` for tablet and mobile)
2. **i18n & Formatting:** Create dictionary files of [Supported Languages](../../AGENTS.md#project-context--metadata) and replace the text in components (if website has one language there is no need). Use `Intl` API for formatting (`rules/i18n.md`).
3. **SEO:** Implement SEO for all [Supported Languages](../../AGENTS.md#project-context--metadata) per [seo.md](../rules/seo.md)

---

## Phase 4: Documentation & Test Generation

1. **TSDoc Standard (MANDATORY):** Document all exported functions, generics, inputs, and defaults in English using strict TSDoc (`rules/documentation.md`). Focus on the "Why".
2. **Algorithmic Documentation:** Update `README.modules.md` and `ARCHITECTURE.md` as necessary (`rules/documentation.md`).
3. **Unit Testing (MANDATORY):** Autonomously generate unit tests in the same directory (`rules/documentation.md`).

---

## Phase 5: Verification & Cleanup

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
