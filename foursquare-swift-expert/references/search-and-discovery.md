# Search And Discovery

Covers:
- Autocomplete
- Place Search

## Mandatory headers

```http
accept: application/json
Authorization: YOUR_FSQ_API_KEY
```

## Swift baseline pattern (required)

```swift
import Foundation

struct GeotaggingResponse: Codable {
    let results: [PlaceCandidate]
}

struct PlaceCandidate: Codable, Identifiable {
    let id: String
    let name: String
    let distance: Int?
    let categories: [Category]
    let location: LocationInfo

    enum CodingKeys: String, CodingKey {
        case id = "fsq_id" // Map Foursquare snake_case to Swift camelCase
        case name, distance, categories, location
    }
}

struct Category: Codable {
    let id: Int
    let name: String
    let icon: Icon?
}

struct Icon: Codable {
    let prefix: String
    let suffix: String
}

struct LocationInfo: Codable {
    let address: String?
    let locality: String?
    let region: String?
    let country: String?
}
```

## Error handling (required)

```swift
import Foundation

struct FoursquareErrorResponse: Codable {
    let message: String?
    let errorDetail: String?

    enum CodingKeys: String, CodingKey {
        case message
        case errorDetail = "error_detail"
    }
}

enum FoursquareAPIError: Error, LocalizedError {
    case badRequest(String?)
    case unauthorized(String?)
    case forbidden(String?)
    case notFound(String?)
    case serverError(Int)
    case decodingError(Error)
    case unknown(Int)

    var errorDescription: String? {
        switch self {
        case .badRequest(let detail): return "Bad Request: \(detail ?? \"Invalid parameters\")"
        case .unauthorized: return "Unauthorized: Check your API Key."
        case .forbidden(let detail): return "Forbidden: \(detail ?? \"Access restricted\")"
        case .notFound: return "Resource not found."
        case .serverError(let code): return "Foursquare server error (Status: \(code))"
        case .decodingError(let error): return "Failed to decode response: \(error.localizedDescription)"
        case .unknown(let code): return "An unknown error occurred (Status: \(code))"
        }
    }
}
```

```swift
guard let httpResponse = response as? HTTPURLResponse else {
    throw URLError(.badServerResponse)
}

if !(200...299).contains(httpResponse.statusCode) {
    let errorBody = try? JSONDecoder().decode(FoursquareErrorResponse.self, from: data)
    let detail = errorBody?.errorDetail ?? errorBody?.message

    switch httpResponse.statusCode {
    case 400: throw FoursquareAPIError.badRequest(detail)
    case 401: throw FoursquareAPIError.unauthorized(detail)
    case 403: throw FoursquareAPIError.forbidden(detail)
    case 404: throw FoursquareAPIError.notFound(detail)
    case 500...599: throw FoursquareAPIError.serverError(httpResponse.statusCode)
    default: throw FoursquareAPIError.unknown(httpResponse.statusCode)
    }
}
```

## Autocomplete endpoint

- Method: `GET`
- Path: `/places/autocomplete`
- Full URL: `https://places-api.foursquare.com/places/autocomplete`
- Path params: none
- Query params:
  - `query` (required): partial user text
  - `ll` (optional but recommended): `lat,lng`
  - `radius` (optional): meters
  - `types` (optional): restrict suggestion types
  - `limit` (optional): max suggestions

### Typical response schema

```json
{
  "results": [
    {
      "fsq_id": "4a5f2f39f964a52067bf1fe3",
      "name": "Blue Bottle Coffee",
      "distance": 120,
      "categories": [
        {
          "id": 13034,
          "name": "Coffee Shop",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/coffeeshop_",
            "suffix": ".png"
          }
        }
      ],
      "location": {
        "address": "300 Webster St",
        "locality": "Oakland",
        "region": "CA",
        "country": "US"
      }
    }
  ]
}
```

### Swift models and API function

```swift
import Foundation

struct AutocompleteResponse: Codable {
    let results: [PlaceCandidate]
}

func fetchAutocomplete(
    query: String,
    ll: String? = nil,
    radius: Int? = nil,
    types: [String]? = nil,
    limit: Int? = nil,
    apiKey: String
) async throws -> [PlaceCandidate] {
    var components = URLComponents(string: "https://places-api.foursquare.com/places/autocomplete")
    var items = [URLQueryItem(name: "query", value: query)]
    if let ll { items.append(URLQueryItem(name: "ll", value: ll)) }
    if let radius { items.append(URLQueryItem(name: "radius", value: String(radius))) }
    if let types, !types.isEmpty {
        items.append(URLQueryItem(name: "types", value: types.joined(separator: ",")))
    }
    if let limit { items.append(URLQueryItem(name: "limit", value: String(limit))) }
    components?.queryItems = items

    guard let url = components?.url else { throw URLError(.badURL) }

    var request = URLRequest(url: url)
    request.httpMethod = "GET"
    request.addValue("application/json", forHTTPHeaderField: "accept")
    request.addValue(apiKey, forHTTPHeaderField: "Authorization")

    let (data, response) = try await URLSession.shared.data(for: request)

    guard let httpResponse = response as? HTTPURLResponse else {
        throw URLError(.badServerResponse)
    }

    if !(200...299).contains(httpResponse.statusCode) {
        let errorBody = try? JSONDecoder().decode(FoursquareErrorResponse.self, from: data)
        let detail = errorBody?.errorDetail ?? errorBody?.message

        switch httpResponse.statusCode {
        case 400: throw FoursquareAPIError.badRequest(detail)
        case 401: throw FoursquareAPIError.unauthorized(detail)
        case 403: throw FoursquareAPIError.forbidden(detail)
        case 404: throw FoursquareAPIError.notFound(detail)
        case 500...599: throw FoursquareAPIError.serverError(httpResponse.statusCode)
        default: throw FoursquareAPIError.unknown(httpResponse.statusCode)
        }
    }

    do {
        let decoded = try JSONDecoder().decode(AutocompleteResponse.self, from: data)
        return decoded.results
    } catch {
        throw FoursquareAPIError.decodingError(error)
    }
}
```

## Place Search endpoint

- Method: `GET`
- Path: `/places/search`
- Full URL: `https://places-api.foursquare.com/places/search`
- Path params: none
- Query params:
  - `query` (optional): free text
  - One location strategy is required:
    - `ll` (+ optional `radius`), or
    - `near`, or
    - `ne` and `sw`
  - `categories` (optional): comma-separated category IDs
  - `limit` (optional): max results
  - `sort` (optional)
  - `fields` (optional): response field projection

### Typical response schema

```json
{
  "results": [
    {
      "fsq_id": "4a5f2f39f964a52067bf1fe3",
      "name": "Sample Coffee Shop",
      "distance": 120,
      "categories": [{ "id": 13034, "name": "Coffee Shop" }],
      "location": {
        "address": "123 Example St",
        "locality": "New York",
        "region": "NY",
        "country": "US"
      },
      "geocodes": {
        "main": { "latitude": 40.7243, "longitude": -74.0018 }
      }
    }
  ]
}
```

### Swift models and API function

```swift
import Foundation

struct PlaceSearchResponse: Codable {
    let results: [PlaceCandidate]
}

func fetchPlaceSearch(
    query: String? = nil,
    ll: String? = nil,
    near: String? = nil,
    ne: String? = nil,
    sw: String? = nil,
    radius: Int? = nil,
    categories: [Int]? = nil,
    limit: Int? = nil,
    sort: String? = nil,
    fields: [String]? = nil,
    apiKey: String
) async throws -> [PlaceCandidate] {
    var components = URLComponents(string: "https://places-api.foursquare.com/places/search")
    var items: [URLQueryItem] = []

    if let query { items.append(URLQueryItem(name: "query", value: query)) }
    if let ll { items.append(URLQueryItem(name: "ll", value: ll)) }
    if let near { items.append(URLQueryItem(name: "near", value: near)) }
    if let ne { items.append(URLQueryItem(name: "ne", value: ne)) }
    if let sw { items.append(URLQueryItem(name: "sw", value: sw)) }
    if let radius { items.append(URLQueryItem(name: "radius", value: String(radius))) }
    if let categories, !categories.isEmpty {
        items.append(URLQueryItem(name: "categories", value: categories.map(String.init).joined(separator: ",")))
    }
    if let limit { items.append(URLQueryItem(name: "limit", value: String(limit))) }
    if let sort { items.append(URLQueryItem(name: "sort", value: sort)) }
    if let fields, !fields.isEmpty {
        items.append(URLQueryItem(name: "fields", value: fields.joined(separator: ",")))
    }
    components?.queryItems = items.isEmpty ? nil : items

    guard let url = components?.url else { throw URLError(.badURL) }

    var request = URLRequest(url: url)
    request.httpMethod = "GET"
    request.addValue("application/json", forHTTPHeaderField: "accept")
    request.addValue(apiKey, forHTTPHeaderField: "Authorization")

    let (data, response) = try await URLSession.shared.data(for: request)

    guard let httpResponse = response as? HTTPURLResponse else {
        throw URLError(.badServerResponse)
    }

    if !(200...299).contains(httpResponse.statusCode) {
        let errorBody = try? JSONDecoder().decode(FoursquareErrorResponse.self, from: data)
        let detail = errorBody?.errorDetail ?? errorBody?.message

        switch httpResponse.statusCode {
        case 400: throw FoursquareAPIError.badRequest(detail)
        case 401: throw FoursquareAPIError.unauthorized(detail)
        case 403: throw FoursquareAPIError.forbidden(detail)
        case 404: throw FoursquareAPIError.notFound(detail)
        case 500...599: throw FoursquareAPIError.serverError(httpResponse.statusCode)
        default: throw FoursquareAPIError.unknown(httpResponse.statusCode)
        }
    }

    do {
        let decoded = try JSONDecoder().decode(PlaceSearchResponse.self, from: data)
        return decoded.results
    } catch {
        throw FoursquareAPIError.decodingError(error)
    }
}
```
