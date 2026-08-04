---
trigger: framework_match
description: Next.js App Router, React Server Components (RSC), Payload CMS, ISR caching, and Next.js optimized components rules.
---

# Next.js Framework & CMS Standards

## Next.js

* **Source of Truth:** ALWAYS read the relevant documentation in `node_modules/next/dist/docs/` before coding. Your training data may be outdated; the local documentation is the absolute source of truth.
* **Deployment Readiness:** Write code assuming a containerized deployment architecture. Ensure the Next.js config utilizes `output: 'standalone'` and that the application gracefully handles dynamic environment variables and reverse proxy headers (e.g., trusting proxy IPs, handling custom SSL terminations).

## Payload CMS

* **Documentation:** [Payload CMS docs](https://payloadcms.com/docs/)
* **Usage:** The website uses Payload CMS to manage all content (including blogs). This encompasses adding, removing, searching, and structuring dynamic content.
* **Database Schemas:** When designing database schemas, prioritize modular, relational structures capable of handling complex entities (like product variations or multi-language fields).

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

## NextJS specific

If [framework](@/AGENTS.md#project-context--metadata) is NextJS use their own optimized components to create new components or pages (e.g, use `Image`, `Link`)

## Route Navigation Progress & Streaming

* **Global Route Navigation Progress:**
  * When creating pages, navigation menus, or custom links in Next.js App Router, ensure route transition progress is implemented using `@vercel/react-transition-progress` (`<ProgressBarProvider>` and progress-aware `<Link>` or `useProgress()`).
  * Ensure immediate visual acknowledgment (<100ms) upon click/tap when users navigate slow networks or stream Server Components, preventing perceived frozen states and repeated clicks.
  * **Top Bar Styling:** Render progress bars fixed at the top (`fixed top-0 left-0 right-0 z-[9999] h-1 pointer-events-none`) styled with primary theme tokens (`bg-primary`), smooth width transitions (`transition-all duration-200 ease-out`), subtle glow/shadow (`shadow-[0_0_10px_rgba(var(--primary-rgb),0.5)]`), and graceful opacity fade-outs.

## Payload CMS ERDs & Schemas

* **Payload CMS ERDs:** Any time you modify a Payload CMS Collection or Global, you MUST update the Entity-Relationship Diagram (ERD) inside `ARCHITECTURE.md` to reflect the new database schema.

