---
description: Next.js App Router specific workflow steps for creating React components (RSC boundaries, ISR caching, optimized components)
---

# Next.js Component Creation Workflow

This workflow extends the root universal component workflow (`.agents/workflows/create-component.md`) with Next.js App Router specific execution steps.

---

## Phase 1: Pre-Flight & Planning (Next.js Specifics)

1. **RSC & Caching Strategy Check**:
   - Determine if the component is cacheable under Incremental Static Regeneration (ISR).
   - Identify interactive elements that require `"use client"` and plan boundary isolation.

---

## Phase 2: Create Component (Next.js Architecture)

1. **React Server Components (RSC) Boundary Strategy**:
   - Make component cacheable Server Component by default.
   - Push `"use client"` down to leaf interactive components.
   - Use RSC Composition Pattern (`children` prop) when wrapping Server Components inside Client Providers/Layouts.
2. **Next.js Optimized Components**:
   - Use `<Link>` from `next/link` instead of raw `<a>` tags.
   - Use `<Image>` from `next/image` with required width/height or `fill` props instead of standard `<img>` tags.

---

## Phase 3: Navigation Progress & Loading States

1. **Route Transition Progress**:
   - Wrap interactive navigation elements with progress-aware controls (`@vercel/react-transition-progress`).
   - Style top navigation bar fixed at top (`fixed top-0 left-0 right-0 z-[9999] h-1 pointer-events-none`).
2. **Streaming & Skeleton Fallbacks**:
   - Wrap slow data-fetching components in React `<Suspense>` boundaries with matching `<Skeleton>` components.
