# Flag a Place

Source: https://docs.foursquare.com/fsq-developers-places/reference/place-flag

`POST /places/{fsq_place_id}/suggest/flag`

Flag a field(s) on a Place as incorrect. Does not require you to provide the correct value.

## Mandatory Headers

| Header | Required | Value | Notes |
| --- | --- | --- | --- |
| `Authorization` | Yes | `Bearer <FSQ_API_KEY>` | Bearer Token to authorize requests. |
| `X-Places-Api-Version` | Yes | `2025-06-17` | The version of the API to use. |

## Path Parameters

| Name | In | Required | Type | Description |
| --- | --- | --- | --- | --- |
| `fsq_place_id` | `path` | Yes | `string` | A unique string identifier for a FSQ Place (formerly known as Venue ID). E.g., Foursquare HQ's fsq_place_id = 5a187743ccad6b307315e6fe. |

## Query Parameters

| Name | In | Required | Type | Description |
| --- | --- | --- | --- | --- |
| `dry_run` | `query` | No | `boolean` | If true, return the expected result without actually submitting the suggestion. Useful for testing. **Note this defaults to *false* in all cases EXCEPT when calling through this docs page.** Default: True |
| `fields` | `query` | Yes | `string` | List of fields being flagged as incorrect. Possible values are:address; latitude; longitude; locality; name; tel; postcode; region; state; website; placeActions; |
| `comment` | `query` | No | `string` | A comment describing the issue being flagged. |

## Response Codes

| Status | Meaning | Handling |
| --- | --- | --- |
| `200` | success | Decode success payload. |

## Error Handling

Use shared error utilities from `./shared-error-handling.md` and apply them directly in this endpoint implementation.

```swift
import Foundation

// Reuse these shared definitions from ./shared-error-handling.md:
// - FoursquareAPIError
// - FoursquareErrorResponse
// - handleHTTPErrorIfNeeded(data:httpResponse:)
```

Known non-2xx response codes for this endpoint: `default`.

## Swift Request Example

```swift
import Foundation

func requestPlaceFlag(apiKey: String) async throws -> PlaceFlagResponse {
    // 1. Safe URL Construction
    let urlString = "https://places-api.foursquare.com/places/4ca61465a6e08cfa9d087794/suggest/flag"
    guard var components = URLComponents(string: urlString) else {
        throw FoursquareAPIError.invalidURL
    }

    // 2. Define your query items
    let newQueryItems = [
        URLQueryItem(name: "dry_run", value: "false"),
        URLQueryItem(name: "fields", value: "name,location,categories"),
        URLQueryItem(name: "comment", value: "Incorrect place details")
    ]

    // 3. The best way to append: nil-coalesce to an empty array first
    components.queryItems = (components.queryItems ?? []) + newQueryItems

    // 4. Create and configure the request
    guard let url = components.url else {
        throw FoursquareAPIError.invalidURL
    }

    var request = URLRequest(url: url)
    request.httpMethod = "POST"
    request.setValue("Bearer \(apiKey)", forHTTPHeaderField: "Authorization")
    request.setValue("2025-06-17", forHTTPHeaderField: "X-Places-Api-Version")

    let (data, response) = try await URLSession.shared.data(for: request)
    guard let httpResponse = response as? HTTPURLResponse else {
        throw FoursquareAPIError.invalidResponse
    }

    if !(200...299).contains(httpResponse.statusCode) {
        let apiError = try? JSONDecoder().decode(FoursquareErrorResponse.self, from: data)
        throw FoursquareAPIError.apiError(statusCode: httpResponse.statusCode, error: apiError)
    }

    switch httpResponse.statusCode {
        case 200:
            return try JSONDecoder().decode(PlaceFlagResponse.self, from: data)

        default:
            throw FoursquareAPIError.unexpectedStatusCode(httpResponse.statusCode)
    }
}
```


## Swift Data Models

```swift
import Foundation

struct PlaceFlagResponse: Codable, Identifiable, Hashable {
    let suggestedEdits: [PlaceFlagSuggestedEdit]
    let errors: [String]

    var id: Int { hashValue }

    enum CodingKeys: String, CodingKey {
        case suggestedEdits = "suggested_edits"
        case errors = "errors"
    }
}

struct PlaceFlagSuggestedEdit: Codable, Identifiable, Hashable {
    let id: String
    let fsqPlaceID: String
    let suggestedEditType: String
    let createdAt: String
    let status: String

    enum CodingKeys: String, CodingKey {
        case id = "id"
        case fsqPlaceID = "fsq_place_id"
        case suggestedEditType = "suggested_edit_type"
        case createdAt = "created_at"
        case status = "status"
    }
}
```
