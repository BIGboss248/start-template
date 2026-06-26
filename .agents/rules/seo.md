---
trigger: model_decision
description: Instructions for SEO optimization, schema-dts structured data, semantic HTML, sitemaps, and indexing restrictions. Mandatory when craeting new web page or editing one
---

# SEO & Semantic HTML

## General SEO & Semantic HTML

* Always use Semantic HTML tags (`<article>`, `<nav>`, `<aside>`, etc.) to follow accessibility and SEO best practices.
* **Proactive Content & Section Recommendations:** Before generating code for a new page, you MUST act as an SEO advisor and propose additional semantic sections that would improve the page's ranking and topical authority (e.g., `FAQ` for long-tail queries, `Related Products` for internal linking, or `Breadcrumbs` for deep hierarchies).
* **Heading Hierarchy:** Use a single `<h1>` per page with proper heading hierarchy (sequential `<h2>` to `<h6>`).
* **Next.js App Router Metadata:** Every page must utilize Next.js's Metadata API (`generateMetadata` for dynamic pages or `metadata` export for static pages). Include localized `title`, `description`, and `keywords`. You MUST generate self-referencing canonical URLs for every page to prevent duplicate content penalties across localized routes. Include fully fleshed-out Open Graph (`og:title`, `og:image`, `og:type`) and Twitter Card metadata.
* **Strictly Typed Structured Data (schema-dts):** Do not deploy a page without injecting relevant JSON-LD schema. To prevent invalid schema properties, you MUST use the `schema-dts` package to enforce type safety. Never use `@ts-ignore` or `any` when defining the schema object. on each new schema insertion mark it with a TODO comment asking user to validate using [validator.schema.org](https://validator.schema.org/)
  * **Injection Pattern:**

    ```tsx
    import { WithContext, WebPage } from 'schema-dts';
    const jsonLd: WithContext<WebPage> = {
      '@context': 'https://schema.org',
      '@type': 'WebPage',
      name: 'Page Title',
      description: 'Page description text',
    };
    export default function Page() {
      return (
        <>
          <script
            type="application/ld+json"
            dangerouslySetInnerHTML={{ __html: JSON.stringify(jsonLd) }}
          />
          <main>
            <h1>Page Title</h1>
          </main>
        </>
      );
    }
    ```

* **Crawlability & Indexation Control:**
  * **Dynamic Sitemaps:** Ensure Next.js `sitemap.ts` files dynamically map over Payload CMS database content. Update `sitemap.ts` and `robots.ts` whenever new routes are added.
  * **Robots Directives:** Implement `noindex, nofollow` tags for internal search result pages, user dashboards, or checkout flows.
