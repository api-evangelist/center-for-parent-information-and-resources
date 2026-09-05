# Center for Parent Information and Resources (center-for-parent-information-and-resources)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

The Center for Parent Information and Resources (CPIR) is a federally funded central hub of information and products for the national network of Parent Training and Information Centers (PTIs) and Community Parent Resource Centers (CPRCs). CPIR supports families and youth, with a focus on children and youth with disabilities. This repository captures the APIs, developer tools, and machine-readable API artifacts for CPIR.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/center-for-parent-information-and-resources/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags

- Disability
- Education
- Families
- Federal Government
- Parent Centers
- Parent Training
- Parents
- Special Needs

## Timestamps

- **Created:** 2024-12-03
- **Modified:** 2026-09-05

## APIs

CPIR runs no developer program and publishes no API documentation or OpenAPI of its own. Its own host does,
however, serve a real anonymous read-only JSON API: the WordPress REST API at
`https://www.parentcenterhub.org/wp-json`, which registered 34 namespaces and 717 routes when profiled on
2026-09-05. Four surfaces were documented from the route index the site itself publishes, each verified
against live anonymous responses:

- **CPIR Parent Center Directory API** — the machine-readable form of *Find Your Parent Center*. 784+ Parent
  Centers (PTIs, CPRCs and regional PTACs) across 55 state and territory terms, each with organization name,
  work address, geocoded latitude/longitude, phone, email and public link, in vCard/hCard field naming.
- **CPIR Geography Reference API** — ISO 3166-1 country records, US state centroids and bounding boxes, and
  country boundary geometry as GeoJSON (RFC 7946).
- **CPIR oEmbed API** — a conformant oEmbed 1.0 provider endpoint for any CPIR page.
- **CPIR Site Metadata API** — the discovery root, registered content types and taxonomies, and the Yoast SEO
  head document carrying schema.org JSON-LD.

No credential is required for any of these, and no third party can write to them — CPIR issues no API keys.

**What is not readable:** the WordPress content collections on this host (`/wp/v2/posts`, `/pages`, `/media`,
`/categories`, `/tags`, `/users`, `/search`) all return `401 rest_forbidden` to an anonymous caller. Article
content is available through the site RSS feed instead. There is also no MCP server, no A2A agent card, no
`/.well-known/` document of any kind, no SDK, CLI or GitHub organisation, and no pricing, status page,
changelog or deprecation policy.

The OpenAPI documents in `openapi/` were derived by API Evangelist from CPIR's own published route index
(archived verbatim at `openapi/_original/`); CPIR did not author them.

## Common Properties

- [Website](https://www.parentcenterhub.org)
- [Newsletter](https://www.parentcenterhub.org/buzz/)
- [Events](https://www.parentcenterhub.org/events/)
- [Contact](https://www.parentcenterhub.org/contact/)
- [Privacy Policy](https://www.parentcenterhub.org/privacy-policy/)

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
