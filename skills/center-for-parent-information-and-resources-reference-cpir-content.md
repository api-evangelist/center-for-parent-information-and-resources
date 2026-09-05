---
name: reference-cpir-content
description: Cite or embed a CPIR page correctly using its oEmbed endpoint and schema.org head document, and know why the WordPress content collections cannot be used.
api: CPIR oEmbed API
generated: '2026-09-05'
method: generated
source: >-
  Grounded in openapi/center-for-parent-information-and-resources-oembed-api-openapi.yml and
  openapi/center-for-parent-information-and-resources-site-metadata-api-openapi.yml, derived from
  CPIR's published wp-json route index and verified live on 2026-09-05.
operations:
  - getOEmbedData
  - getSeoHead
  - getApiRoot
---

# Reference CPIR content

CPIR publishes a large body of plain-language special-education material. An agent citing it should
resolve titles and attribution from CPIR's own machine surface rather than scraping the page.

Base URL: `https://www.parentcenterhub.org/wp-json`

## Read this first: the content collections are closed

`/wp/v2/posts`, `/pages`, `/media`, `/categories`, `/tags`, `/users` and `/search` all return
`401 rest_forbidden` to an anonymous caller on this host — verified 2026-09-05. A security plugin
has closed anonymous REST content reads. **Do not plan an integration around them.** For article
content, use the RSS feed at `https://www.parentcenterhub.org/feed/`.

## Step 1 — Resolve a page to citable metadata

`getOEmbedData` — `GET /oembed/1.0/embed?url=<encoded CPIR url>`

Returns a conformant oEmbed 1.0 document: `version`, `provider_name`
("Center for Parent Information and Resources"), `provider_url`, `title`, `author_name`, and
embeddable `html`. Use `title` and `provider_name` for the citation and `html` if you are embedding.

`maxwidth` defaults to 600.

## Step 2 — Get structured data when you need more

`getSeoHead` — `GET /yoast/v1/get_head?url=<CPIR url>`

Returns `{ "html": "..." }` containing the rendered head block, which includes an
`application/ld+json` script with `"@context": "https://schema.org"` and an `@graph` of `WebPage`,
`BreadcrumbList` and `WebSite` nodes. Parse the JSON-LD out of that string — it gives you canonical
URL, breadcrumbs and site identity without touching the gated content routes.

## Step 3 — Confirm the surface has not changed

`getApiRoot` — `GET /` (that is, `https://www.parentcenterhub.org/wp-json/`)

Returns the full index: site name, 34 namespaces and 717 routes as of 2026-09-05, each with its
argument schema. CPIR publishes no changelog and no deprecation policy, and namespaces here come and
go with plugin updates, so re-read this index before relying on a route you have not called
recently.

## Conventions

- Errors are `{"code","message","data":{"status"}}` as `application/json`, not RFC 9457.
- No credential is needed for any of the above. `/oembed/1.0/proxy` is the exception — it requires
  an authenticated session and returns `401 rest_forbidden` anonymously.
- Attribute CPIR by name and link to the page. The material is federally funded public-good content;
  citing it properly is the expected use.
