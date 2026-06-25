---
trigger: model_decision
description: Rules for Tailwind CSS, RTL support, logical properties, theme variables, loading skeletons (MANDATORY when creating new pages/components), GSAP animations, and asset optimization.
---

# UI, Styling & Animations

## Tailwind CSS & Shadcn UI

* The website uses Tailwind CSS alongside a Shadcn Theme Provider.
* **Strict Class Merging:** NEVER concatenate Tailwind classes using standard template literals. Always use the `cn()` utility function (which wraps `clsx` and `tailwind-merge`) to resolve styling conflicts, especially when exposing `className` props in reusable components.
* **Theming & Design Tokens:** The project uses Shadcn UI's CSS variable system. Do NOT use hardcoded Tailwind color scales (e.g., `bg-blue-500`) unless explicitly requested. Strictly use semantic theme tokens (e.g., `bg-background`, `text-muted-foreground`, `border-border`) to ensure perfect compatibility across Light and Dark modes. Chosen colors must pass accessibility contrast ratios in both modes.
* **RTL & Logical Properties:** The website language is Persian (Right-to-Left). You **must** use Tailwind's logical classes that respect direction (e.g., use `ms` instead of `ml`, `pe` instead of `pr`). The usage of physical directional classes (like `ml`, `pr`, `left`, `right`) is FORBIDDEN to ensure the layout mirrors correctly when switching directions.

## Responsive Design

* Pages and layouts must be perfectly responsive across mobile, tablet, and desktop. If necessary, develop three distinct internal layout versions for complex pages to ensure responsiveness.

## Loading States (MANDATORY for New Pages & Components)

> [!IMPORTANT]
> The rules in this section are **MANDATORY** and must be strictly followed whenever you are creating a new page or component in the codebase.

* Create loading skeletons (using [Shadcn Skeleton](https://ui.shadcn.com/docs/components/radix/skeleton)) for all new pages and data-heavy components. Wrap them in React `<Suspense>` boundaries to improve perceived performance while loading.
* Do not block the entire page from rendering while data fetches; wrap individual slow components in React `<Suspense>` and provide accurate `<Skeleton>` fallbacks matching the exact dimensions of loaded content to prevent Cumulative Layout Shift (CLS).

## Animations

* **Tool:** Use [GSAP](https://gsap.com/resources/) for animations utilizing the `useGSAP()` hook.
* **Animation Hierarchy (GSAP vs. CSS):**
  * **Micro-interactions:** Use standard Tailwind CSS transitions (e.g., `transition-all hover:scale-105`) for simple hover states, focus rings, or basic toggles.
  * **Complex Animations:** Use GSAP only for complex sequence timelines or scroll-triggered animations.
* **Constraint:** Animations must never negatively impact Core Web Vitals or SEO.
* **React Safety:** When using GSAP, strictly wrap logic inside the `@gsap/react` `useGSAP()` hook to handle scoped selectors and automatic garbage collection. Always assign `ref`s to the animated DOM elements to avoid mutating the virtual DOM directly.

## Asset Optimization (Web Vitals)

* **Images:** NEVER use standard `<img>` tags. Exclusively use the Next.js `<Image>` component. You must provide `alt` text, explicit `width`/`height` or `fill`, and use the `priority` prop for Largest Contentful Paint (LCP) images above the fold.
* **Fonts/Small Assets:** Any small assets (SVG icons, fonts, etc.) used across the website must be hosted locally. Never use external `<link>` tags for fonts. Use `next/font/google` or `next/font/local` to ensure fonts are self-hosted and optimally loaded with zero layout shift.
* **Large Assets:** For larger assets (e.g., heavy images), explicitly notify the user and ask them to decide whether to host them locally or via a remote CDN.
