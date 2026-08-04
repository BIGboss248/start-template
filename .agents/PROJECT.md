# Active Project Context & Metadata

> [!IMPORTANT]
> **PROJECT-SPECIFIC DATA BINDING**: This file serves as the single source of truth for the AI agent regarding this specific project's architecture, dependencies, and business rules. It lives outside the central rules submodule (`.agents/core/`) and is **never overwritten** when syncing universal rules or updating templates.

## Project Context & Metadata

* **Project Name**: `[Project Name]`
* **Framework & Version**: `[e.g., Next.js 14+ (App Router)]`
* **CMS Integration**: `[e.g., Payload CMS 3.0 (Self-hosted inside Next.js) / None]`
* **Primary Database**: `[e.g., PostgreSQL (via Prisma / Drizzle) / MongoDB]`
* **Supported Languages**: `[e.g., English (LTR), Persian (RTL bidirectional support)]`
* **Package Manager**: `[e.g., Bun]`
* **Primary Styling Tool**: `[e.g., Tailwind CSS / Vanilla CSS]`
* **Hosting & Infrastructure**: `[e.g., Docker Container deployed to VPS via Caprover]`
* **CI/CD Pipelines**: `[e.g., GitHub Actions (ci.yml for pull requests, cd.yml for Semantic Release and Docker builds)]`
* **new component dir**: `[e.g., ./components/custom components]`
* **component library**: `[e.g., Shadcn]`
* **Client data file**: `[e.g., @/constants/clientInfo.ts]`
      includes
      1. Social media links, phone numbers, and addresses.
      2. Business details and contact info.
* **project info file**: `[e.g., @/constants/projectInfo.ts]`
      Read from `docs/sitemap-architecture.md` includes
      1. Page Name: `[e.g., Home, Blog Post]`
      2. Route Path: `[e.g., / or /blog/[slug]]`
      3. Page metadata and Schema.org attributes.
      4. Access Level / Auth Requirement
      5. Redirects & Rewrites
      6. Dynamic Route Parameters
      7. Global website metadata.
* **Style file dir**: `[e.g., @/globals.css]`
* **Animation library**: `[e.g., GSAP]`
* **Utility dir**: `[e.g., src/utilities]`

### Workspace Documentation References

* **Client Discovery Doc**: [docs/client-requirements.md](file:///docs/client-requirements.md) *(Form answers from Phase 0)*
* **Technical Stack Doc**: [docs/tech-stack.md](file:///docs/tech-stack.md) *(Architecture choices from Phase 1)*
* **Sitemap & Caching Spec**: [docs/sitemap-architecture.md](file:///docs/sitemap-architecture.md) *(Route maps and delivery profiles from Phase 2)*
