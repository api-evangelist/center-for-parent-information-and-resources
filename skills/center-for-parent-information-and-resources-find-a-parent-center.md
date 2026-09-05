---
name: find-a-parent-center
description: Find the Parent Training and Information Center (PTI) or Community Parent Resource Center (CPRC) that serves a given US family, using the CPIR national directory API.
api: CPIR Parent Center Directory API
generated: '2026-09-05'
method: generated
source: >-
  Grounded in openapi/center-for-parent-information-and-resources-parent-center-directory-api-openapi.yml,
  which was derived from CPIR's published wp-json route index and verified live on 2026-09-05.
operations:
  - listDirectoryCategories
  - listDirectoryEntries
  - getDirectoryEntry
  - autocompleteDirectoryField
---

# Find a Parent Center

Every US state and territory has at least one federally funded Parent Center that helps families of
children with disabilities navigate special education. CPIR publishes the national directory of them
as anonymous JSON. This skill turns "I live in Iowa and my child has an IEP, who can help me?" into a
concrete center with an address and a phone number.

Base URL: `https://www.parentcenterhub.org/wp-json`
No credential is required. Every call below is a GET.

## Step 1 — Resolve the family's state to a category id

The directory is organised by a state and territory taxonomy, 55 terms as of 2026-09-05.

`listDirectoryCategories` — `GET /cn-api/v1/category?per_page=100&search=Iowa`

Match on the `name` field and keep the `id`. Do not guess an id; there is no published mapping from
a state abbreviation to a category id, so resolve it by name every time.

## Step 2 — List the centers in that state

`listDirectoryEntries` — `GET /cn-api/v1/entry?categories=<id>&per_page=100`

If the family described a city or an organization name instead of a state, use
`autocompleteDirectoryField` instead:

`GET /cn-api/v1/autocomplete/city?search=Des` or `GET /cn-api/v1/autocomplete/organization?search=Parent`

The autocomplete route returns raw directory rows and accepts `order` (asc/desc) and `orderby`
(id, include, name, slug, term_group, description, count).

## Step 3 — Read the center's details

`getDirectoryEntry` — `GET /cn-api/v1/entry/{id}`

Useful fields, in vCard/hCard naming:

- `fn.rendered` — the center's full name
- `org.organization_name.rendered` — the organization name
- `adr[]` — work address, with `street_address`, `locality`, `region`, `postal_code`, and a
  geocoded `latitude` / `longitude` as decimal strings
- `tel[]`, `email[]`, `social[]` — contact channels
- `link` — the center's public page on parentcenterhub.org, which is what you should show a family

## Conventions that will bite you

- **Unescape the text.** Every rendered field is HTML-entity encoded. Entry 35's `fn.rendered` is
  `Access For Special Kids Resource Center (ASK) &#8211; PTI`. Decode before displaying or matching.
- **Page until empty.** `X-WP-Total` and `X-WP-TotalPages` are advertised in
  `Access-Control-Expose-Headers` but are not actually emitted on this collection. Loop `page` at
  `per_page=100` until you get `[]`. The full directory was 784+ entries on 2026-09-05.
- **Respect the crawl delay.** `robots.txt` states `Crawl-delay: 600`. There is no documented rate
  limit and no rate-limit header, so that directive is the only rate CPIR states. Cache the
  directory rather than re-paging it per query.
- **Errors are not RFC 9457.** A failure returns `{"code": "...", "message": "...", "data": {"status": N}}`
  as `application/json`. A `401 rest_forbidden` means the route is closed to anonymous callers, not
  that your credential is wrong — CPIR issues no credentials.
- **Read-only.** The collection answers `Allow: GET` anonymously. Do not attempt to create or
  correct a directory entry through the API; send corrections to
  https://www.parentcenterhub.org/contact-us/ instead.
