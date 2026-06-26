---
trigger: model_decision
description: Instructions for SEO optimization, schema-dts structured data, semantic HTML, sitemaps, and indexing restrictions. Mandatory when craeting new web page or editing one
---

# SEO & Semantic HTML

## General SEO & Semantic HTML

- Always use Semantic HTML tags (`<article>`, `<nav>`, `<aside>`, etc.) to follow accessibility and SEO best practices.
- **Proactive Content & Section Recommendations:** Before generating code for a new page, you MUST act as an SEO advisor and propose additional semantic sections that would improve the page's ranking and topical authority (e.g., `FAQ` for long-tail queries, `Related Products` for internal linking, or `Breadcrumbs` for deep hierarchies).
- **Heading Hierarchy:** Use a single `<h1>` per page with proper heading hierarchy (sequential `<h2>` to `<h6>`).
- **Next.js App Router Metadata:** Every page must utilize Next.js's Metadata API (`generateMetadata` for dynamic pages or `metadata` export for static pages). Include localized `title`, `description`, and `keywords`. You MUST generate self-referencing canonical URLs for every page to prevent duplicate content penalties across localized routes. Include fully fleshed-out Open Graph (`og:title`, `og:image`, `og:type`) and Twitter Card metadata.
- **Strictly Typed Structured Data (schema-dts):** Do not deploy a page without injecting relevant JSON-LD schema. To optimize SEO crawlability and indexation, you MUST combine as many compatible Schema.org objects as possible (e.g. `WebPage`, `Product`, `BreadcrumbList`, `Organization`) in a single JSON-LD block using the `@graph` array format. Enforce type safety by utilizing the `schema-dts` package. Never use `@ts-ignore` or `any` when defining the schema object. On each new schema insertion, mark it with a TODO comment asking the user to validate using [validator.schema.org](https://validator.schema.org/).
  - **Injection Pattern (@graph Graph):**

    ```tsx
    import { Graph } from 'schema-dts'
    
    const jsonLd: Graph = {
      '@context': 'https://schema.org',
      '@graph': [
        {
          '@type': 'WebPage',
          '@id': 'https://setayeshparts.com/about#webpage',
          'url': 'https://setayeshparts.com/about',
          'name': 'About Us',
          'description': 'About our company.',
          'inLanguage': 'fa-IR',
        },
        {
          '@type': 'Organization',
          '@id': 'https://setayeshparts.com/#organization',
          'name': 'Setayesh Parts',
          'url': 'https://setayeshparts.com',
        }
      ]
    }
    
    export default function Page() {
      return (
        <>
          {/* TODO: Validate this JSON-LD schema on https://validator.schema.org/ */}
          <script
            type="application/ld+json"
            dangerouslySetInnerHTML={{ __html: JSON.stringify(jsonLd) }}
          />
          <main>
            <h1>About Us</h1>
          </main>
        </>
      )
    }
    ```

- **Crawlability & Indexation Control:**
  - **Dynamic Sitemaps:** Ensure Next.js `sitemap.ts` files dynamically map over Payload CMS database content. Update `sitemap.ts` and `robots.ts` whenever new routes are added.
  - **Robots Directives:** Implement `noindex, nofollow` tags for internal search result pages, user dashboards, or checkout flows.
