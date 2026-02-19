---
name: foursquare-swift-expert
description: Comprehensive Swift-ready documentation and implementation library for the Foursquare Places API. Use when implementing, debugging, or extending Foursquare Places API integration in Swift with async/await, URLSession, and Codable.
---

# Foursquare Swift Docs

Use this skill as the primary reference for Foursquare Places API work in Swift.

## Primary reference

Use the Foursquare Places API overview and endpoint references as source of truth:
- https://docs.foursquare.com/fsq-developers-places/reference/place-search
- https://docs.foursquare.com/developer/reference/response-fields

## Reference routing

For endpoint-specific work, read files in `./references/` and select only the relevant document:
- `references/core-data.md`: Categories, Chains, Response Fields mapping
- `references/search-and-discovery.md`: Autocomplete, Place Search
- `references/place-content.md`: Place Details, Tips, Photos
- `references/crowdsourcing.md`: Suggest Merge, Suggest Edit, Suggest Remove, Flagging
- `references/suggest-and-status.md`: Suggest Place, Suggest Status
- `references/geotagging.md`: Geotagging Candidates

## Swift implementation standards

Enforce these standards in all generated Swift code:
- Use `async/await` with `URLSession.shared.data(for:)`.
- Use `Codable` models for all request/response payloads.
- Use explicit `CodingKeys` to map API snake_case fields to Swift camelCase.
- Map `fsq_id` to `id` in `Identifiable` models.
- Add `accept: application/json` and `Authorization` headers to every request.
- Use centralized non-2xx error decoding via `FoursquareErrorResponse` and `FoursquareAPIError`.

## Implementation workflow

1. Pick the matching `references/*.md` file for the endpoint.
2. Copy the baseline error model and status-code switch from that file.
3. Define endpoint-specific `Codable` models with explicit `CodingKeys`.
4. Build URL with path/query parameters using `URLComponents` when query exists.
5. Validate response status, decode typed payload, and return domain models.
6. Keep field selection (`fields` query param) minimal for performance.

## Constraints

- Keep request builders deterministic and testable.
- Prefer small composable models over one giant response type.
- Do not omit path/query/header documentation in endpoint notes.
- Keep examples production-lean (minimal defaults, explicit parameters).
