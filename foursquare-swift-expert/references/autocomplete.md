# Autocomplete

Source: https://docs.foursquare.com/fsq-developers-places/reference/autocomplete

`GET /autocomplete`

Returns a list of top places, geos, and/or searches partially matching the provided keyword and location inputs.

## Mandatory Headers

| Header | Required | Value | Notes |
| --- | --- | --- | --- |
| `Authorization` | Yes | `Bearer <FSQ_API_KEY>` | Bearer Token to authorize requests. |
| `X-Places-Api-Version` | Yes | `2025-06-17` | The version of the API to use. |

## Path Parameters

_None._

## Query Parameters

| Name | In | Required | Type | Description |
| --- | --- | --- | --- | --- |
| `query` | `query` | Yes | `string` | A search term to be applied against titles. Must be at least 3 characters long. |
| `ll` | `query` | No | `string` | The latitude/longitude around which you wish to retrieve place information. Specified as latitude,longitude (e.g., ll=41.8781,-87.6298). If you do not specify ll, the server will attempt to retrieve the IP address from the request, and geolocate that IP address. |
| `radius` | `query` | No | `integer` | Defines the distance (in meters) within which to return place results. Setting a radius biases the results to the indicated area, but may not fully restrict results to that specified area. If not provided, default radius is set to 5000 meters. |
| `types` | `query` | No | `string` | The types of results to return; any combination of place, search, and/or geo.If no types are specified, all types will be returned. |
| `bias` | `query` | No | `string` | Bias the autocomplete results by a specific type; one of place, search, or geo. |
| `session_token` | `query` | No | `string` | A user-generated token to to group the user's query and the user's selected result into a discrete session for billing purposes. Learn more about [session tokens](https://docs.foursquare.com/reference/session-tokens). *If the session_token parameter is omitted, the session is charged per keystroke/request.* |
| `limit` | `query` | No | `integer` | The number of results to return, up to 50. Defaults to 10. |

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

func requestAutocomplete(apiKey: String) async throws -> AutocompleteResponse {
    // 1. Safe URL Construction
    let urlString = "https://places-api.foursquare.com/autocomplete"
    guard var components = URLComponents(string: urlString) else {
        throw FoursquareAPIError.invalidURL
    }

    // 2. Define your query items
    let newQueryItems = [
        URLQueryItem(name: "query", value: "coffee"),
        URLQueryItem(name: "ll", value: "40.7484,-73.9857"),
        URLQueryItem(name: "radius", value: "5000"),
        URLQueryItem(name: "types", value: "place,category,brand"),
        URLQueryItem(name: "bias", value: "proximity"),
        URLQueryItem(name: "session_token", value: "session-token-123"),
        URLQueryItem(name: "limit", value: "10")
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
            return try JSONDecoder().decode(AutocompleteResponse.self, from: data)

        default:
            throw FoursquareAPIError.unexpectedStatusCode(httpResponse.statusCode)
    }
}
```


## Swift Data Models

Use shared core models from `./shared-models.md` (`Place`, `Category`, `Location`) and keep only endpoint-specific wrappers here.

```swift
import Foundation

struct AutocompleteResponse: Codable, Identifiable, Hashable {
    let results: [AutocompleteResult]

    var id: Int { hashValue }

    enum CodingKeys: String, CodingKey {
        case results = "results"
    }
}

struct AutocompleteResult: Codable, Identifiable, Hashable {
    let type: String
    let text: AutocompleteText
    let link: String
    let place: Place

    var id: String { link }

    enum CodingKeys: String, CodingKey {
        case type = "type"
        case text = "text"
        case link = "link"
        case place = "place"
    }
}

struct AutocompleteText: Codable, Identifiable, Hashable {
    let primary: String
    let secondary: String
    let highlight: [AutocompleteHighlight]

    var id: String { primary + "|" + secondary }

    enum CodingKeys: String, CodingKey {
        case primary = "primary"
        case secondary = "secondary"
        case highlight = "highlight"
    }
}

struct AutocompleteHighlight: Codable, Identifiable, Hashable {
    let start: Int
    let length: Int

    var id: String { "\(start)-\(length)" }

    enum CodingKeys: String, CodingKey {
        case start = "start"
        case length = "length"
    }
}
```
