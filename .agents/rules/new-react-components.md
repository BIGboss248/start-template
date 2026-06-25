---
trigger: model_decision
description: Guidelines for creating new react components mandatory when deciding to craete new react components, new web pages or layouts
---

# new react component rules

## Component Reuse First

* Before creating any new component, you **MUST** search the codebase (especially [new components dir](@/AGENTS.md#project-context--metadata) and [component library](@/AGENTS.md#project-context--metadata)) for existing components that can be reused, adapted, or extended.
* You are **strictly forbidden** from creating a new component if an existing component can perform the requested task, or can be modified, refactored, or extended (e.g., by adding optional props, parameters, generic types, or configurable CSS classes) to do so without breaking existing consumers.
* Creating a new component is a measure of absolute last resort, allowed **ONLY** when no other component in the workspace can fulfill the required capability and adapting existing ones is structurally impossible.

## component save dir

* **Placement:** Store and categorize all new components under [new components dir](@/AGENTS.md#project-context--metadata).
* **Imports:** Always import packages and files relative to the project root (e.g., using `@/` path).

## React Server components(RSC)

* **Default to React Server Components (RSC):** Make the component a cacheable Server Component by default. Fetch data, resolve metadata, and render layout/static contents on the server.
* **Maximize RSC Coverage:** Prioritize splitting components to isolate interactivity. Never turn a large, layout-heavy component into a Client Component just because one sub-element (e.g. a button, filter, or input field) requires interactivity. If the component had more than one file create a directory with the name of the compoenent and store all relevant components there.
* **Isolate Client Components (Leaf Components):**
  * Push the `"use client"` boundary as far down the component tree as possible (often to leaf components).
  * Extract interactive elements (state managers, click handlers, input forms, GSAP animations) into separate small client components.
  * Keep the parent component as a Server Component.
* **RSC Composition Pattern:**
  * If a Client Component needs to wrap or contain Server Components (e.g. wrapper layouts, context providers, or animation container frames), pass the Server Components as `children` or standard React props.
  * Do not import Server Components directly inside a `"use client"` file, as this automatically forces the imported Server Component to run on the client, losing server-side data fetching and cacheability.

## Component caching

* **Rendering Strategy:** The preferred rendering strategy is Incremental Static Regeneration (ISR). Refer to the [Next.js ISR guide](node_modules/next/dist/docs/02-pages/02-guides/incremental-static-regeneration.md) if the [project framewrok](@/AGENTS.md#project-context--metadata). **Never cache content if it introduces a security risk.** Do not cache user-specific data or routes containing sensitive transactional information.

## Component styling

* **centeral design**: The project uses [Style file](@/AGENTS.md#project-context--metadata) CSS variable and tailwind token system. Do NOT use hardcoded Tailwind color scales (e.g., `bg-blue-500`) unless explicitly requested. Strictly use semantic theme tokens (e.g., `bg-background`, `text-muted-foreground`, `border-border`) to ensure perfect compatibility across Light and Dark modes. Chosen colors must pass accessibility contrast ratios in both modes.
* **Strict Class Merging:** NEVER concatenate Tailwind classes using standard template literals. Always use the `cn()` utility function (which wraps `clsx` and `tailwind-merge`) to resolve styling conflicts, especially when exposing `className` props in reusable components.
* **RTL & Logical Properties:** You **must** use Tailwind's logical classes that respect direction (e.g., use `ms` instead of `ml`, `pe` instead of `pr`). The usage of physical directional classes (like `ml`, `pr`, `left`, `right`) is FORBIDDEN to ensure the layout mirrors correctly when switching directions.

## Animations

* **Animation Hierarchy:**
  * **Micro-interactions:** Use standard Tailwind CSS transitions (e.g., `transition-all hover:scale-105`) for simple hover states, focus rings, or basic toggles.
  * **Complex Animations:** Use [Animation library](@/AGENTS.md#project-context--metadata) only for complex sequence timelines or scroll-triggered animations.
* **Constraint:** Animations must never negatively impact Core Web Vitals or SEO.

## Responsive Design

* Pages, layouts and components must be perfectly responsive across mobile, tablet, and desktop. If necessary, develop three distinct internal layout versions for complex pages to ensure responsiveness.

## Loading states

* Create loading skeletons for all new pages and data-heavy components. Wrap them in React `<Suspense>` boundaries to improve perceived performance while loading.
* Do not block the entire page from rendering while data fetches; wrap individual slow components in React `<Suspense>` and provide accurate `<Skeleton>` fallbacks matching the exact dimensions of loaded content to prevent Cumulative Layout Shift (CLS).

## NextJS specific

If [framework](@/AGENTS.md#project-context--metadata) is NextJS use their own optimized components to create new components or pages (e.g, use `Image`, `Link`)

## Commenting

Each page or component section must have a comment above their code
