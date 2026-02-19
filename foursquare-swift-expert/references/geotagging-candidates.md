# Find Geotagging Candidates

Source: https://docs.foursquare.com/fsq-developers-places/reference/geotagging-candidates

`GET /geotagging/candidates`

Utilize Foursquare's Snap to Place technology to detect where your user's device is and what is around them. This endpoint will intentionally return lower quality results not found in Place Search. It is not intended to replace Place Search as the primary way to search for top, recommended POIs.

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
| `fields` | `query` | No | `string` | Indicate which fields to return in the response, separated by commas. If no fields are specified, all Pro Fields are returned by default. For a complete list of returnable fields, refer to the Places Response Fields page. |
| `ll` | `query` | No | `string` | The latitude/longitude around which to retrieve place information. This must be specified as latitude,longitude (e.g., ll=41.8781,-87.6298). If you do not specify ll, the server will attempt to retrieve the IP address from the request, and geolocate that IP address. |
| `hacc` | `query` | No | `number` | The estimated horizontal accuracy radius in meters of the user’s location at the 68th percentile confidence level as returned by the user’s cell phone OS. |
| `altitude` | `query` | No | `number` | The altitude of the user’s location in meters above the World Geodetic System 1984 (WGS84) reference ellipsoid as returned by the user’s cell phone OS. |
| `query` | `query` | No | `string` | A string to be matched against place name for candidates. |
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

func requestGeotaggingCandidates(apiKey: String) async throws -> GeotaggingCandidatesResponse {
    // 1. Safe URL Construction
    let urlString = "https://places-api.foursquare.com/geotagging/candidates"
    guard var components = URLComponents(string: urlString) else {
        throw FoursquareAPIError.invalidURL
    }

    // 2. Define your query items
    let newQueryItems = [
        URLQueryItem(name: "fields", value: "name,location,categories"),
        URLQueryItem(name: "ll", value: "40.7484,-73.9857"),
        URLQueryItem(name: "hacc", value: "12.5"),
        URLQueryItem(name: "altitude", value: "120.0"),
        URLQueryItem(name: "query", value: "coffee"),
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
            return try JSONDecoder().decode(GeotaggingCandidatesResponse.self, from: data)

        default:
            throw FoursquareAPIError.unexpectedStatusCode(httpResponse.statusCode)
    }
}
```


## Swift Data Models

Use shared core models from `./shared-models.md` (`Place`, `Category`, `Location`) and keep only endpoint-specific wrappers here.

```swift
import Foundation

struct GeotaggingCandidatesResponse: Codable, Identifiable, Hashable {
    let candidates: [Place]

    var id: Int { hashValue }

    enum CodingKeys: String, CodingKey {
        case candidates = "candidates"
    }
}
```
