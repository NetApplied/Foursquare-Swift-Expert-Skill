# Get Place Details

Source: https://docs.foursquare.com/fsq-developers-places/reference/place-details

`GET /places/{fsq_place_id}`

Retrieve comprehensive information and metadata for a FSQ Place using the fsq_place_id.

## Mandatory Headers

| Header | Required | Value | Notes |
| --- | --- | --- | --- |
| `Authorization` | Yes | `Bearer <FSQ_API_KEY>` | Bearer Token to authorize requests. |
| `X-Places-Api-Version` | Yes | `2025-06-17` | The version of the API to use. |

## Path Parameters

| Name | In | Required | Type | Description |
| --- | --- | --- | --- | --- |
| `fsq_place_id` | `path` | Yes | `string` | A unique string identifier for a FSQ Place (formerly known as Venue ID). E.g., Foursquare HQ's fsq_place_id = 5a187743ccad6b307315e6fe |

## Query Parameters

| Name | In | Required | Type | Description |
| --- | --- | --- | --- | --- |
| `fields` | `query` | No | `string` | Indicate which fields to return in the response, separated by commas. If no fields are specified, all Pro Fields are returned by default. For a complete list of returnable fields, refer to the Places Response Fields page. |

## Response Codes

| Status | Meaning | Handling |
| --- | --- | --- |
| `200` | success | Decode success payload. |
| `404` | invalid venue specified | Decode `FoursquareErrorResponse` and throw `FoursquareAPIError.apiError`. |

## Error Handling

Use shared error utilities from `./shared-error-handling.md` and apply them directly in this endpoint implementation.

```swift
import Foundation

// Reuse these shared definitions from ./shared-error-handling.md:
// - FoursquareAPIError
// - FoursquareErrorResponse
// - handleHTTPErrorIfNeeded(data:httpResponse:)
```

Known non-2xx response codes for this endpoint: `404`.

## Swift Request Example

```swift
import Foundation

func requestPlaceDetails(apiKey: String) async throws -> PlaceDetailsResponse {
    // 1. Safe URL Construction
    let urlString = "https://places-api.foursquare.com/places/5a187743ccad6b307315e6fe"
    guard var components = URLComponents(string: urlString) else {
        throw FoursquareAPIError.invalidURL
    }

    // 2. Define your query items
    let newQueryItems = [
        URLQueryItem(name: "fields", value: "name,location,categories")
    ]

    // 3. The best way to append: nil-coalesce to an empty array first
    components.queryItems = (components.queryItems ?? []) + newQueryItems

    // 4. Create and configure the request
    guard let url = components.url else {
        throw FoursquareAPIError.invalidURL
    }

    var request = URLRequest(url: url)
    request.httpMethod = "GET"
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
            return try JSONDecoder().decode(PlaceDetailsResponse.self, from: data)
        case 404:
            let apiError = try? JSONDecoder().decode(FoursquareErrorResponse.self, from: data)
            throw FoursquareAPIError.apiError(statusCode: 404, error: apiError)
        default:
            throw FoursquareAPIError.unexpectedStatusCode(httpResponse.statusCode)
    }
}
```


## Swift Data Models

Use shared core models from `./shared-models.md` (`Place`, `Category`, `Location`) for this endpoint.

```swift
import Foundation

typealias PlaceDetailsResponse = Place
```
