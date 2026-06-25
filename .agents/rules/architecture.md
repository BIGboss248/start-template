---
trigger: model_decision
description: Guidelines for system architecture, folder layouts, component imports, global navigation, and Server vs Client Component strategy. MANDATORY when creating any new page or component.
---

# Architecture & File Structure

> [!IMPORTANT]
> This rule file is **MANDATORY** and must be strictly followed whenever you are creating a new page or component in the codebase.

## Global Navigation & Metadata

* Add all global references to the central navigation file: `/src/constants/navigation.ts`. Reference them across the site from this single source of truth.
* This file must contain:
  1. Page names and routes.
  2. Social media links, phone numbers, and addresses.
  3. Business details and contact info.
  4. Global website metadata.
  5. Page metadata and Schema.org attributes.
* *Note:* If any metadata or Schema.org attributes are missing, add them to the file with a `// TODO:` comment to remind the developer to fill them out.

## New Pages & Components

* **Component Reuse First (MANDATORY & CRITICAL):**
  * Before creating any new component, you **MUST** exhaustively search the codebase (especially `/src/components/CustomComponents/` and page route directories) for existing components that can be reused, adapted, or extended.
  * You are **strictly forbidden** from creating a new component if an existing component can perform the requested task, or can be modified, refactored, or extended (e.g., by adding optional props, parameters, generic types, or configurable CSS classes) to do so without breaking existing consumers.
  * Creating a new component is a measure of absolute last resort, allowed **ONLY** when no other component in the workspace can fulfill the required capability and adapting existing ones is structurally impossible.
* **Placement:** Store and categorize all new components under `/src/components/CustomComponents/`.
* **Imports:** Always import packages and files relative to the project root (e.g., using `@/` path aliases if configured, or absolute paths).
* **Rendering Strategy & Component Splitting (RSC vs. Client):**
  * **Default to React Server Components (RSC):** Make the component a cacheable Server Component by default. Fetch data, resolve metadata, and render layout/static contents on the server.
  * **Maximize RSC Coverage:** Prioritize splitting components to isolate interactivity. Never turn a large, layout-heavy component into a Client Component just because one sub-element (e.g. a button, filter, or input field) requires interactivity.
  * **Isolate Client Components (Leaf Components):**
    * Push the `"use client"` boundary as far down the component tree as possible (often to leaf components).
    * Extract interactive elements (state managers, click handlers, input forms, GSAP animations) into separate small client components.
    * Keep the parent component as a Server Component.
  * **RSC Composition Pattern:**
    * If a Client Component needs to wrap or contain Server Components (e.g. wrapper layouts, context providers, or animation container frames), pass the Server Components as `children` or standard React props.
    * Do not import Server Components directly inside a `"use client"` file, as this automatically forces the imported Server Component to run on the client, losing server-side data fetching and cacheability.
* **Rendering Strategy:** The preferred rendering strategy is Incremental Static Regeneration (ISR). Refer to the [Next.js ISR guide](node_modules/next/dist/docs/02-pages/02-guides/incremental-static-regeneration.md). **Never cache content if it introduces a security risk.** Do not cache user-specific data or routes containing sensitive transactional information.
