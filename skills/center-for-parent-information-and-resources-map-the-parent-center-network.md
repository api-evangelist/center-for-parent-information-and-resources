---
name: map-the-parent-center-network
description: Build a geographic view of the US Parent Center network by joining CPIR directory entries to state centroids and bounding boxes from the same API.
api: CPIR Parent Center Directory API
generated: '2026-09-05'
method: generated
source: >-
  Grounded in openapi/center-for-parent-information-and-resources-parent-center-directory-api-openapi.yml
  and openapi/center-for-parent-information-and-resources-geography-reference-api-openapi.yml, both
  derived from CPIR's published wp-json route index and verified live on 2026-09-05.
operations:
  - listDirectoryEntries
  - listCountryRegions
  - getCountryRegion
  - getCountryGeoJson
---

# Map the Parent Center network

CPIR geocodes every Parent Center and serves US state geography from the same host, so a national
map of the network can be built entirely from one base URL with no credential.

Base URL: `https://www.parentcenterhub.org/wp-json`

## Step 1 — Pull the whole directory once

`listDirectoryEntries` — `GET /cn-api/v1/entry?per_page=100&page=N`

Loop `page` until an empty array. 784+ entries on 2026-09-05. Each entry's `adr[0]` carries
`latitude` and `longitude` as decimal **strings** — cast them before use — plus `region`, a
two-letter subdivision code such as `IA`.

Cache this. `robots.txt` asks for a 600-second crawl delay and there is no rate-limit header to
negotiate against, so pull the directory once and re-use it.

## Step 2 — Pull the state geography

`listCountryRegions` — `GET /cn-api/v1/countries/us/regions`

Returns an object keyed by subdivision code. Each value has `name`, `alt_names`, and a `geo` block
with `latitude`, `longitude`, `min_latitude`, `min_longitude`, `max_latitude`, `max_longitude` —
a centroid and a bounding box, which is exactly what you need to frame a per-state view.

For one state only, use `getCountryRegion` — `GET /cn-api/v1/countries/us/region/IA`.

## Step 3 — Join, and know that the join is by value

The entry's `adr[].region` string matches a key in the regions object. **There is no foreign key
and no link relation between the two namespaces** — you are joining `"IA"` to `"IA"` yourself.
Normalise case and trim before matching, and count the entries that fail to join rather than
silently dropping them: an unjoinable entry usually means a missing or non-standard `region` value
in the directory record, which is a real data-quality finding worth reporting back to CPIR.

## Step 4 — Add the national outline

`getCountryGeoJson` — `GET /cn-api/v1/countries/us/geojson`

Returns a GeoJSON FeatureCollection (RFC 7946) with a MultiPolygon geometry, ~493 KB as observed on
2026-09-05. Fetch it once and cache it; it does not change.

## Cautions

- Entries are contact records for organizations, and several carry a named staff contact. Treat the
  `email` and `tel` arrays as publication-limited: they are published for families to reach a center,
  not for bulk outreach. Do not build a mailing list from this directory.
- Text fields are HTML-entity encoded inside `{"rendered": ...}`.
- The directory is read-only to third parties. Report a wrong address to
  https://www.parentcenterhub.org/contact-us/ rather than trying to PATCH it.
