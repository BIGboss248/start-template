---
description: Rules and steps to follow when designing a new web page
---

# Workflow: Create Page

This workflow defines the step-by-step process and rules to follow when designing and implementing a new page in the Next.js App Router codebase. It integrates rules from `.agents/rules/` to ensure architectural integrity, rendering strategy correctness, SEO, internationalization (i18n), styling, testing, and documentation standards.

---

## Phase 1: Pre-Flight & Planning (SEO Advisor)

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

1. **Chain of Thought Planning:** Plan the route path, layout structure, page-level data fetching, and dependencies. Output a numbered list of steps and wait for user approval.
2. **Codebase Component Inventory Check (MANDATORY):** If the new page requires layout blocks, sections, or UI elements, you **MUST** first perform a codebase search (e.g., using `grep_search` or `list_dir` on `/src/components/CustomComponents/` or page routes) to identify existing custom components that can perform the task. You are **strictly prohibited** from creating new custom components for the page if existing components can be reused, adapted, or extended (e.g., by adding optional parameters, classes, or types) to fulfill the requirements.
3. **Shadcn Component Check (MANDATORY):** Check if Shadcn UI has a component that meets your needs (e.g., Accordion, Dialog, Dropdown, etc.). If a Shadcn component is available, you **MUST** use/add that component using the `shadcn` skill instead of creating a new custom component from scratch. If the `shadcn` skill is missing, install it by running `bunx --bun skills add shadcn/ui`.
4. **GSAP Skills Check (MANDATORY):** If the page components require complex animations using GSAP, you **MUST** read the appropriate local GSAP skill files (e.g., `gsap-core`, `gsap-react`, `gsap-scrolltrigger`, etc. in `.agents/skills/`) to guide the implementation. If the GSAP skills are missing, run `bunx skills add https://github.com/greensock/gsap-skills` to install them.
5. **SEO Advisor Check (MANDATORY):** Before generating code for a new page, you **MUST** act as an SEO advisor and propose additional semantic sections that would improve the page's ranking and topical authority:
   - **FAQ section:** For long-tail search queries.
   - **Related Products / Items:** For internal linking strength.
   - **Breadcrumbs:** For hierarchy and search crawler deep-linking.
6. **Route Definition:** Determine the page placement in the Next.js App Router under `/src/app/[locale]/...` or `/src/app/...` matching dynamic/localized route requirements.

---

## Phase 2: Architectural Setup & Routing

1. **Global Navigation Registry:**
   - Add all global references, routes, and page names to the central navigation registry: `/src/constants/navigation.ts`.
   - Ensure the registry contains page names, routes, business contact details, social links, and global website metadata.
2. **Rendering Strategy (ISR-First):**
   - **Default to React Server Components (RSC):** The main `page.tsx` MUST be a Server Component. Fetch data, resolve metadata, and render layout/static contents on the server.
   - **Preferred Strategy:** Use Incremental Static Regeneration (ISR). Make sure caching respects ISR revalidation tags.
   - **Security Restriction:** **NEVER** cache content if it introduces a security risk. Do not cache user-specific data or routes containing sensitive transactional information (e.g., checkout, account dashboards).
3. **Component Splitting (Max RSC Coverage):**
   - Keep the main page file (`page.tsx`) and layout file (`layout.tsx`) as Server Components.
   - Isolate interactivity into leaf Client Components (e.g., filter sidebars, buttons, forms, GSAP animations) and push the `"use client"` directive as far down the component tree as possible.
   - **RSC Composition:** If a Client Component must wrap a Server Component, pass the Server Component as `children`. Never import a Server Component directly inside a `"use client"` file.
4. **Imports Standard:**
   - Always import files using the project root `@/` path aliases or absolute paths. Never use relative paths like `../../`.

---

## Phase 3: SEO, Structured Data, and i18n

### 1. Next.js Metadata API
- Every page **MUST** export metadata:
  - Use `generateMetadata` for dynamic routes (e.g. fetching database fields for a product page).
  - Use a static `metadata` object for static routes.
- Metadata must include localized `title`, `description`, and `keywords` (in Persian).
- **Canonical URLs:** You **MUST** generate self-referencing canonical URLs for every page to prevent duplicate content penalties across localized routes.
- **Social Metadata:** Include fully fleshed-out Open Graph (`og:title`, `og:image`, `og:type`) and Twitter Card metadata.

### 2. Strictly Typed Structured Data (schema-dts)
- Do not deploy a page without injecting relevant JSON-LD schema.
- **Type Safety:** You **MUST** use the `schema-dts` package to enforce type safety. Never use `@ts-ignore` or `any` when defining the schema object.
- **Injection Pattern:**
  ```tsx
  import { WithContext, WebPage } from 'schema-dts';

  const jsonLd: WithContext<WebPage> = {
    '@context': 'https://schema.org',
    '@type': 'WebPage',
    name: 'صفحه اصلی',
    description: 'توضیحات مربوط به صفحه',
  };

  export default function Page() {
    return (
      <>
        <script
          type="application/ld+json"
          dangerouslySetInnerHTML={{ __html: JSON.stringify(jsonLd) }}
        />
        <main>
          <h1>عنوان صفحه</h1>
        </main>
      </>
    );
  }
  ```

### 3. Internationalization (i18n) & Localized UI
- **Language:** All textual content, metadata, Schema.org attributes, and JSON-LD must be in **Persian (fa)**.
- **Locale Source of Truth:** Pages must receive the locale strictly via route parameters (e.g., `[locale]`) or headers from Next.js Edge Middleware. Never guess or read the locale from client-only variables.
- **Bidirectional (BiDi) UI:** Ensure the parent/root layout sets `lang="fa"` and `dir="rtl"` dynamically.
- **Data Formatting:** Use the native JavaScript `Intl` API (`Intl.NumberFormat` and `Intl.DateTimeFormat`) for rendering currencies, decimals, and dates (defaulting to Shamsi/Jalali calendar).

### 4. Search Crawling & Indexing
- Update `sitemap.ts` and `robots.ts` when introducing new static routes or dynamic data paths.
- **Robots Directives:** Implement `noindex, nofollow` meta tags for search results pages, user dashboards, shopping carts, or checkout flows.

---

## Phase 4: UI, Responsive Design, and Loading States

### 1. Styling & Tailwind CSS
- **Theme Variables:** Use Shadcn UI's CSS variable system (e.g. `bg-background`, `text-foreground`, `border-border`). Do **NOT** use hardcoded Tailwind colors unless explicitly requested.
- **RTL Logical Properties:** Use Tailwind's direction-aware logical classes (`ms-*` instead of `ml-*`, `pe-*` instead of `pr-*`). Usage of physical directional classes (`ml`, `mr`, `pl`, `pr`, `left`, `right`) is **FORBIDDEN**.
- **Class Merging:** Always merge classNames using the `cn(...)` utility function.

### 2. Loading States & CLS Prevention (MANDATORY)
- Every new page **MUST** implement matching loading skeletons (using Shadcn's `<Skeleton>` component).
- Wrap data-heavy components or the page body in React `<Suspense>` boundaries.
- Skeletons must match the loaded components' dimensions exactly to prevent **Cumulative Layout Shift (CLS)**. Do not block the entire page from rendering while fetching; resolve individual slow sections asynchronously.

### 3. Responsive Design
- Ensure layouts are perfectly responsive across mobile, tablet, and desktop viewports.
- For highly complex pages, design and implement three distinct internal layout variations (Mobile, Tablet, Desktop) if necessary to maintain clean UX.

### 4. Asset Optimization
- **Images:** Exclusively use the Next.js `<Image>` component instead of standard `<img>` tags. Provide `alt` text, explicit `width`/`height` or `fill`.
- **LCP Images:** Ensure images above the fold use the `priority` prop to optimize Largest Contentful Paint.
- **Fonts/SVGs:** Small assets must be hosted locally. Use `next/font/google` or `next/font/local` to eliminate layout shift. Ask the user before hosting large media assets locally vs. CDN.

---

## Phase 5: Documentation & Test Generation

### 1. TSDoc Standard (MANDATORY)
- Document all page exports, metadata generators, hooks, and helper functions using strict **TSDoc** syntax according to the guidelines in [.agents/rules/documentation.md](file:///d:/Scripts/sampleshop/.agents/rules/documentation.md).
- **English Language Requirement:** All TSDoc comments, inline comments, and markdown documentation files (such as `README.modules.md` and `ARCHITECTURE.md`) MUST be written exclusively in **English**, regardless of the application's locale or target language (e.g., Persian/fa).
- You MUST document:
  - **Generics (`@typeParam`):** Write an individual tag for *every single* generic type parameter. Explain its constraints, purpose, and usage.
  - **Inputs (`@param`):** Write an individual tag for *every single* input parameter. Detail the parameter's name, type, constraints, and semantic description.
  - **Defaults (`@defaultValue`):** Write an individual tag for *every* optional parameter, config option, or component prop property to specify its default value.
  - **Outputs (`@returns`):** Write a detailed description of the return type/element, its meaning, and how conditional rendering or return scenarios behave.
  - **Exceptions (`@throws`):** Write an individual tag for *each* specific error that can be thrown by page helpers or functions, describing the conditions under which it occurs and potential recovery steps.
- Explain the "Why" behind the layout, data structure, or CMS data integrations.

### 2. Localized Algorithmic Documentation (`README.modules.md`)
- Create or update the `README.modules.md` (or route-specific markdown) in the page's directory.
- Must include:
  1. **Algorithmic Breakdown:** Step-by-step explanation of page loading, caching, dynamic route resolution, and data flow.
  2. **Execution Flow:** Mermaid diagram illustrating data fetch sequence, state changes, or server mutations.
  3. **Consumer Tracking:** Document entry points, layout wrapping, and referenced custom components.
  4. **Integration Graph:** A Mermaid flowchart connecting the route/page to the wider application layout tree.
  5. **Dependencies:** external APIs, Payload CMS collections, or queries the page relies on.

### 3. Global Architecture Map (`ARCHITECTURE.md`)
- If the new page introduces new system-wide routes, data flows, or dependencies, update the high-level Mermaid diagram in the root `ARCHITECTURE.md`.
- **Payload CMS modifications:** If you modify collections or globals to feed this page, you **MUST** update the ERD in `ARCHITECTURE.md`.

### 4. Unit & Integration Testing (MANDATORY)
- Generate a unit test file (e.g., `page.test.tsx`) in the same page directory.
- Use the testing framework identified in `package.json` (e.g. Vitest or Jest). If none, default to `bun test`.
- Do NOT run tests manually during page creation, as they are executed automatically via `pre-commit` hooks. For manual verification/testing, run the `/verify-build` workflow.

---

## Phase 6: Verification & Cleanup

1. **Verification Gate:**
   - Do NOT manually run build checks, linting, or tests. These verification steps are automatically run via `pre-commit` hooks during staging/commit/push.
   - If manual verification or build fixing is requested, run the `/verify-build` workflow.
2. **Code Cleanup:**
   - Mark temporary mock data, placeholder UI, or debug logs with `// TODO: REMOVE BEFORE PRODUCTION`.
3. **Versioning:**
   - Update the version number in `package.json` utilizing `bun version patch` (or `minor`/`major`) on completed features. Let the user include this in their commit.

---

## Phase 7: Handoff & User Commits

1. **Handoff:**
   - Present the completed work clearly to the user, highlighting the files created or modified.
   - Do NOT run git commit or git add commands, and do not block progress waiting for commits. Leave all staging, committing, and branching/pushing operations entirely to the user.
   - (Optional) You may suggest appropriate conventional commit messages for their reference, but do not execute them.
