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

* **Local Skeleton Fallbacks & Suspense Boundaries:**
  * Do not block entire page rendering while data fetches. Wrap individual slow components or page segments in React `<Suspense>` boundaries.
  * Provide accurate `<Skeleton>` fallbacks matching the exact layout dimensions of loaded content to prevent Cumulative Layout Shift (CLS). When building multi-layout responsive components, create matching skeleton variants (e.g. `[Component]Skeleton`, `[Component]TabletSkeleton`, `[Component]MobileSkeleton`).
* **Global Route Navigation Progress:**
  * When creating pages, navigation menus, or custom links in Next.js App Router, ensure route transition progress is implemented using `@vercel/react-transition-progress` (`<ProgressBarProvider>` and progress-aware `<Link>` or `useProgress()`).
  * Ensure immediate visual acknowledgment (<100ms) upon click/tap when users navigate slow networks or stream Server Components, preventing perceived frozen states and repeated clicks.
* **Interactive Component Pending States:**
  * Track pending transition states (`isPending` from `useProgress()` or React `useTransition`) on interactive components (buttons, links, form triggers).
  * Render inline loading indicators/spinners and temporarily disable interactive elements during active transitions to prevent double-submits and duplicate clicks.
* **UI/UX & Accessibility Loading Rules:**
  * **Strict Visual Hierarchy:** Maintain clear separation between **Global Route Progress** (top fixed progress bar) and **Local Layout Structure** (content skeletons/spinners).
  * **Top Bar Styling:** Render progress bars fixed at the top (`fixed top-0 left-0 right-0 z-[9999] h-1 pointer-events-none`) styled with primary theme tokens (`bg-primary`), smooth width transitions (`transition-all duration-200 ease-out`), subtle glow/shadow (`shadow-[0_0_10px_rgba(var(--primary-rgb),0.5)]`), and graceful opacity fade-outs.
  * **Micro-Flicker Prevention:** Apply a ~100ms delay threshold before rendering progress indicators on fast sub-100ms navigations to avoid jarring flashes.
  * **Accessibility (a11y):** Must include ARIA attributes (`role="progressbar"`, `aria-busy={isPending}`, `aria-valuemin={0}`, `aria-valuemax={100}`, dynamic `aria-valuenow`) and respect `@media (prefers-reduced-motion: reduce)` by disabling sliding animations in favor of opacity fades.

## Commenting

Each page or component section must have a comment above their code
