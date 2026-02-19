# Geotagging

Covers:
- Geotagging Candidates

## Mandatory headers

```http
accept: application/json
Authorization: YOUR_FSQ_API_KEY
```

## Geotagging candidates endpoint

- Method: `GET`
- Path: `/places/geotagging`
- Full URL: `https://places-api.foursquare.com/places/geotagging`
- Path params: none
- Query params:
  - `ll` (required): latitude/longitude as `lat,lng`
  - `limit` (optional): max candidate count

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

## Typical response schema

```json
{
  "results": [
    {
      "fsq_id": "4a5f2f39f964a52067bf1fe3",
      "name": "Sample Coffee Shop",
      "distance": 45,
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
        "address": "123 Example St",
        "locality": "Boston",
        "region": "MA",
        "country": "US"
      }
    }
  ]
}
```

## Swift usage example

```swift
import Foundation

func fetchGeotaggingCandidates(
    lat: Double,
    lng: Double,
    limit: Int? = nil,
    apiKey: String
) async throws -> [PlaceCandidate] {
    var components = URLComponents(string: "https://places-api.foursquare.com/places/geotagging")
    var queryItems = [URLQueryItem(name: "ll", value: "\(lat),\(lng)")]
    if let limit {
        queryItems.append(URLQueryItem(name: "limit", value: String(limit)))
    }
    components?.queryItems = queryItems

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
        let decodedResponse = try JSONDecoder().decode(GeotaggingResponse.self, from: data)
        return decodedResponse.results
    } catch {
        throw FoursquareAPIError.decodingError(error)
    }
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
