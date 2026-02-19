# Core Data

Covers Foursquare Places API reference data endpoints:
- Categories
- Chains
- Response Field mappings

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

## Categories endpoint

- Method: `GET`
- Path: `/places/categories`
- Full URL: `https://places-api.foursquare.com/places/categories`
- Path params: none
- Query params:
  - `locale` (optional): locale/language code for category labels

### Typical response schema

```json
{
  "results": [
    {
      "id": 13000,
      "name": "Dining and Drinking",
      "short_name": "Dining",
      "plural_name": "Dining and Drinking Places",
      "icon": {
        "prefix": "https://ss3.4sqi.net/img/categories_v2/food/default_",
        "suffix": ".png"
      },
      "parent": null,
      "categories": [
        {
          "id": 13065,
          "name": "Restaurant",
          "short_name": "Restaurant",
          "plural_name": "Restaurants",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/default_",
            "suffix": ".png"
          },
          "parent": 13000,
          "categories": []
        }
      ]
    }
  ]
}
```

### Swift models and API function

```swift
import Foundation

struct CategoriesResponse: Codable {
    let results: [CategoryNode]
}

struct CategoryNode: Codable {
    let id: Int
    let name: String
    let shortName: String?
    let pluralName: String?
    let icon: Icon?
    let parent: Int?
    let categories: [CategoryNode]?

    enum CodingKeys: String, CodingKey {
        case id, name, icon, parent, categories
        case shortName = "short_name"
        case pluralName = "plural_name"
    }
}

func fetchCategories(locale: String? = nil, apiKey: String) async throws -> [CategoryNode] {
    var components = URLComponents(string: "https://places-api.foursquare.com/places/categories")
    if let locale {
        components?.queryItems = [URLQueryItem(name: "locale", value: locale)]
    }

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
        let decoded = try JSONDecoder().decode(CategoriesResponse.self, from: data)
        return decoded.results
    } catch {
        throw FoursquareAPIError.decodingError(error)
    }
}
```

## Chains endpoint

- Method: `GET`
- Path: `/places/chains`
- Full URL: `https://places-api.foursquare.com/places/chains`
- Path params: none
- Query params:
  - `query` (required): free text chain name
  - `limit` (optional): number of results

### Typical response schema

```json
{
  "results": [
    {
      "fsq_id": "c3f5c4fd-617e-4f74-bb3f-2f58f9fa7654",
      "name": "Blue Bottle Coffee"
    }
  ]
}
```

### Swift models and API function

```swift
import Foundation

struct ChainsResponse: Codable {
    let results: [Chain]
}

struct Chain: Codable, Identifiable {
    let id: String
    let name: String

    enum CodingKeys: String, CodingKey {
        case id = "fsq_id"
        case name
    }
}

func searchChains(query: String, limit: Int? = nil, apiKey: String) async throws -> [Chain] {
    var components = URLComponents(string: "https://places-api.foursquare.com/places/chains")
    var items = [URLQueryItem(name: "query", value: query)]
    if let limit {
        items.append(URLQueryItem(name: "limit", value: String(limit)))
    }
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
        let decoded = try JSONDecoder().decode(ChainsResponse.self, from: data)
        return decoded.results
    } catch {
        throw FoursquareAPIError.decodingError(error)
    }
}
```

## Response field mapping guidance

Use field selection to reduce payload size and map snake_case fields with `CodingKeys`.

Common mappings:
- `fsq_id` -> `id`
- `created_at` -> `createdAt`
- `short_name` -> `shortName`
- `plural_name` -> `pluralName`
- `formatted_address` -> `formattedAddress`
- `error_detail` -> `errorDetail`
