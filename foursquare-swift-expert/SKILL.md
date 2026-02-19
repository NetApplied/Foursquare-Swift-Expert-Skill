---
name: foursquare-swift-expert
description: Comprehensive Swift-ready documentation and implementation library for the Foursquare Places API.
---

# Foursquare Swift Expert

Use this skill as the primary knowledge base for implementing Foursquare Places API integrations in Swift.

## Primary reference

Use the Foursquare Places API overview and then follow endpoint-specific guidance in `./references/`.

## Workflow

1. Identify the endpoint from the user request.
2. Open the matching file in `./references/`.
3. Apply the documented path/query parameters, required headers, response schemas, and response-code handling.
4. Implement Swift requests with `async/await` and `URLSession`.
5. Implement Swift models using `Codable`, `Identifiable`, and `Hashable` with explicit `CodingKeys` mapping from snake_case JSON fields to camelCase Swift properties.
6. Use endpoint error handling that decodes `FoursquareErrorResponse` and parses `error_detail` when available.

## Swift standards

- Use `async/await` for all network calls.
- Use `URLSession` for request execution.
- Define models with `Codable`, `Identifiable`, and `Hashable`.
- Define explicit `CodingKeys` in each model to map snake_case keys to camelCase properties.
- Use this response gate in endpoint implementations:

```swift
if !(200...299).contains(httpResponse.statusCode) {
    let decodedError = try? JSONDecoder().decode(FoursquareErrorResponse.self, from: data)
    throw FoursquareAPIError.httpStatus(code: httpResponse.statusCode, detail: decodedError?.errorDetail)
}
```

## Reference files

- `references/common-data.md`
- `references/autocomplete.md`
- `references/place-search.md`
- `references/place-details.md`
- `references/place-tips.md`
- `references/place-photos.md`
- `references/place-flag.md`
- `references/suggest-merge-places.md`
- `references/suggest-edit-place.md`
- `references/suggest-remove-place.md`
- `references/suggest-new-place.md`
- `references/suggest-status.md`
- `references/suggest-review.md`
- `references/geotagging-candidates.md`
- `references/geotagging-confirm.md`
