---
name: foursquare-swift-expert
description: Comprehensive Swift-ready documentation and implementation library for the Foursquare Places API.
---

# Foursquare Swift Expert

Use this skill to implement Foursquare Places API integrations in Swift.

## Core Workflow

1. Read the primary reference: `Foursquare Places API Overview`.
2. Route endpoint-specific work to files in `./references/`.
3. Keep request code on `async/await` + `URLSession`.
4. Model responses with `Codable`, `Identifiable`, and `Hashable`.
5. Use explicit `CodingKeys` to map snake_case JSON to camelCase Swift properties.
6. Apply the shared non-2xx error-handling pattern that decodes `error_detail`.

## Swift Standards

- Use `async/await` for network calls.
- Use `URLSession` for request execution.
- Build URLs with `URLComponents` and append query items via `(components.queryItems ?? []) + newQueryItems`.
- Set `Authorization` and `X-Places-Api-Version` headers on every request.
- Decode success payloads with `JSONDecoder`.
- For non-2xx responses, decode `FoursquareErrorResponse` from `error_detail` and throw `FoursquareAPIError`.

## Endpoint References

- `./references/autocomplete.md`
- `./references/place-search.md`
- `./references/place-details.md`
- `./references/place-flag.md`
- `./references/geotagging-candidates.md`
- `./references/shared-models.md`
- `./references/shared-error-handling.md`
