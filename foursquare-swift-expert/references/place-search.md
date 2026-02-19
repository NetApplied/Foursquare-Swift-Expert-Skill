# Place Search

Source: https://docs.foursquare.com/fsq-developers-places/reference/place-search

`GET /places/search`

Search for places in the FSQ Places database using a location and querying by name, category name, telephone number, taste label, or chain name. For example, search for "coffee" to get back a list of recommended coffee shops. You may pass a location with your request by using one of the following options. If none of the following options are passed, Place Search defaults to geolocation using ip biasing with the optional radius parameter. ll & radius (circular boundary); near (geocodable locality); ne & sw (rectangular boundary);

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
| `query` | `query` | No | `string` | A string to be matched against all content for this place, including but not limited to venue name, category, telephone number, taste, and tips. |
| `ll` | `query` | No | `string` | The latitude/longitude around which to retrieve place information. This must be specified as latitude,longitude (e.g., ll=41.8781,-87.6298). |
| `radius` | `query` | No | `integer` | Sets a radius distance (in meters) used to define an area to bias search results. The maximum allowed radius is 100,000 meters. Radius can be used in combination with ll or ip biased geolocation only. By using radius, global search results will be omitted. If not provided, default radius applied is 22000 meters. |
| `fsq_category_ids` | `query` | No | `string` | Filters the response and returns FSQ Places matching the specified categories. Supports multiple Category IDs, separated by commas. For a complete list of Foursquare Category IDs, refer to the Category Taxonomy page. |
| `fsq_chain_ids` | `query` | No | `string` | Filters the response and returns FSQ Places matching the specified chains. Supports multiple chain IDs, separated by commas. For more information on Foursquare Chain IDs, refer to the Chains page. |
| `exclude_fsq_chain_ids` | `query` | No | `string` | Filters the response and returns FSQ Places not matching any of the specified chains. Supports multiple chain IDs, separated by commas. Cannot be used in conjunction with exclude_all_chains. For more information on Foursquare Chain IDs, refer to the Chains page. |
| `exclude_all_chains` | `query` | No | `boolean` | Filters the response by only returning FSQ Places that are not known to be part of any chain. Cannot be used in conjunction with exclude_chains. |
| `fields` | `query` | No | `string` | Indicate which fields to return in the response, separated by commas. If no fields are specified, all Pro Fields are returned by default. For a complete list of returnable fields, refer to the Places Response Fields page. |
| `min_price` | `query` | No | `integer` | Restricts results to only those places within the specified price range. Valid values range between 1 (most affordable) to 4 (most expensive), inclusive. |
| `max_price` | `query` | No | `integer` | Restricts results to only those places within the specified price range. Valid values range between 1 (most affordable) to 4 (most expensive), inclusive. |
| `open_at` | `query` | No | `string` | Support local day and local time requests through this parameter. To be specified as DOWTHHMM (e.g., 1T2130), where DOW is the day number 1-7 (Monday = 1, Sunday = 7) and time is in 24 hour format. Places that do not have opening hours will not be returned if this parameter is specified. Cannot be specified in conjunction with `open_now`. |
| `open_now` | `query` | No | `boolean` | Restricts results to only those places that are open now. Places that do not have opening hours will not be returned if this parameter is specified. Cannot be specified in conjunction with `open_at`. |
| `tel_format` | `query` | No | `string` | Specifies the format of the returned telephone number. Allowed: NATIONAL, E164 |
| `ne` | `query` | No | `string` | The latitude/longitude representing the north/east points of a rectangle. Must be used with sw parameter to specify a rectangular search box. Global search results will be omitted. |
| `sw` | `query` | No | `string` | The latitude/longitude representing the south/west points of a rectangle. Must be used with ne parameter to specify a rectangular search box. Global search results will be omitted. |
| `near` | `query` | No | `string` | A string naming a locality in the world (e.g., "Chicago, IL"). If the value is not geocodable, returns an error. Global search results will be omitted. |
| `sort` | `query` | No | `string` | Specifies the order in which results are listed. Allowed: RELEVANCE, RATING, DISTANCE, POPULARITY |
| `limit` | `query` | No | `integer` | The number of results to return, up to 50. Defaults to 10. |

## Response Codes

| Status | Meaning | Handling |
| --- | --- | --- |
| `200` | success | Decode success payload. |
| `400` | bad request | Decode `FoursquareErrorResponse` and throw `FoursquareAPIError.apiError`. |
| `401` | unauthorized | Decode `FoursquareErrorResponse` and throw `FoursquareAPIError.apiError`. |

## Error Handling

Use shared error utilities from `./shared-error-handling.md` and apply them directly in this endpoint implementation.

```swift
import Foundation

// Reuse these shared definitions from ./shared-error-handling.md:
// - FoursquareAPIError
// - FoursquareErrorResponse
// - handleHTTPErrorIfNeeded(data:httpResponse:)
```

Known non-2xx response codes for this endpoint: `400, 401`.

## Swift Request Example

```swift
import Foundation

func requestPlaceSearch(apiKey: String) async throws -> PlaceSearchResponse {
    // 1. Safe URL Construction
    let urlString = "https://places-api.foursquare.com/places/search"
    guard var components = URLComponents(string: urlString) else {
        throw FoursquareAPIError.invalidURL
    }

    // 2. Define your query items
    let newQueryItems = [
        URLQueryItem(name: "query", value: "coffee"),
        URLQueryItem(name: "ll", value: "40.7484,-73.9857"),
        URLQueryItem(name: "radius", value: "5000"),
        URLQueryItem(name: "fsq_category_ids", value: "13065"),
        URLQueryItem(name: "fsq_chain_ids", value: "556a38d2a7c8957d73d43c37"),
        URLQueryItem(name: "exclude_fsq_chain_ids", value: "556a38d2a7c8957d73d43c37"),
        URLQueryItem(name: "exclude_all_chains", value: "false"),
        URLQueryItem(name: "fields", value: "name,location,categories"),
        URLQueryItem(name: "min_price", value: "1"),
        URLQueryItem(name: "max_price", value: "4"),
        URLQueryItem(name: "open_at", value: "2026-02-19T12:00:00"),
        URLQueryItem(name: "open_now", value: "true"),
        URLQueryItem(name: "tel_format", value: "NATIONAL"),
        URLQueryItem(name: "ne", value: "40.8000,-73.9000"),
        URLQueryItem(name: "sw", value: "40.7000,-74.0500"),
        URLQueryItem(name: "near", value: "New York, NY"),
        URLQueryItem(name: "sort", value: "RELEVANCE"),
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
            return try JSONDecoder().decode(PlaceSearchResponse.self, from: data)
        case 400:
            let apiError = try? JSONDecoder().decode(FoursquareErrorResponse.self, from: data)
            throw FoursquareAPIError.apiError(statusCode: 400, error: apiError)
        case 401:
            let apiError = try? JSONDecoder().decode(FoursquareErrorResponse.self, from: data)
            throw FoursquareAPIError.apiError(statusCode: 401, error: apiError)
        default:
            throw FoursquareAPIError.unexpectedStatusCode(httpResponse.statusCode)
    }
}
```


## Swift Data Models

Use shared core models from `./shared-models.md` (`Place`, `Category`, `Location`, `Coordinate`, `GeoCircle`, `GeoBounds`) and keep only endpoint-specific wrappers here.

```swift
import Foundation

struct PlaceSearchResponse: Codable, Identifiable, Hashable {
    let results: [Place]
    let context: PlaceSearchContext?

    var id: Int { hashValue }

    enum CodingKeys: String, CodingKey {
        case results = "results"
        case context = "context"
    }
}

struct PlaceSearchContext: Codable, Identifiable, Hashable {
    let geoBounds: GeoBounds?

    var id: Int { hashValue }

    enum CodingKeys: String, CodingKey {
        case geoBounds = "geo_bounds"
    }
}
```
