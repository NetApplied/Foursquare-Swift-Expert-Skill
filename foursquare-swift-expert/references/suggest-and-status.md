# Suggest And Status

Covers:
- Suggest Place
- Suggest Status

## Mandatory headers

```http
accept: application/json
Authorization: YOUR_FSQ_API_KEY
Content-Type: application/json
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

## Suggest Place endpoint

- Method: `POST`
- Path: `/places/suggest`
- Full URL: `https://places-api.foursquare.com/places/suggest`
- Path params: none
- Query params: none

### Typical request schema

```json
{
  "name": "My New Cafe",
  "categories": [13034],
  "location": {
    "address": "123 Main St",
    "locality": "Boston",
    "region": "MA",
    "country": "US",
    "postcode": "02108"
  },
  "geocodes": {
    "main": {
      "latitude": 42.3601,
      "longitude": -71.0589
    }
  }
}
```

### Typical response schema

```json
{
  "status": "submitted",
  "request_id": "f137cd1a-7ad7-4ca9-9298-046e18da3496",
  "message": "Suggestion received",
  "created_at": "2024-02-11T17:31:50.000Z"
}
```

### Swift models and API function

```swift
import Foundation

struct SuggestPlaceRequest: Codable {
    let name: String
    let categories: [Int]
    let location: SuggestLocation
    let geocodes: SuggestGeocodes
}

struct SuggestLocation: Codable {
    let address: String?
    let locality: String?
    let region: String?
    let country: String?
    let postcode: String?
}

struct SuggestGeocodes: Codable {
    let main: LatLng
}

struct LatLng: Codable {
    let latitude: Double
    let longitude: Double
}

struct SuggestPlaceResponse: Codable {
    let status: String
    let requestID: String?
    let message: String?
    let createdAt: String?

    enum CodingKeys: String, CodingKey {
        case status, message
        case requestID = "request_id"
        case createdAt = "created_at"
    }
}

func suggestPlace(
    payload: SuggestPlaceRequest,
    apiKey: String
) async throws -> SuggestPlaceResponse {
    guard let url = URL(string: "https://places-api.foursquare.com/places/suggest") else {
        throw URLError(.badURL)
    }

    var request = URLRequest(url: url)
    request.httpMethod = "POST"
    request.addValue("application/json", forHTTPHeaderField: "accept")
    request.addValue("application/json", forHTTPHeaderField: "Content-Type")
    request.addValue(apiKey, forHTTPHeaderField: "Authorization")
    request.httpBody = try JSONEncoder().encode(payload)

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
        return try JSONDecoder().decode(SuggestPlaceResponse.self, from: data)
    } catch {
        throw FoursquareAPIError.decodingError(error)
    }
}
```

## Suggest Status endpoint

- Method: `GET`
- Path: `/places/suggest/status/{request_id}`
- Full URL: `https://places-api.foursquare.com/places/suggest/status/{request_id}`
- Path params:
  - `request_id` (required): ID returned by suggest request
- Query params: none

### Typical response schema

```json
{
  "request_id": "f137cd1a-7ad7-4ca9-9298-046e18da3496",
  "status": "in_review",
  "updated_at": "2024-02-12T10:05:01.000Z",
  "resolution": null
}
```

### Swift models and API function

```swift
import Foundation

struct SuggestStatusResponse: Codable {
    let requestID: String
    let status: String
    let updatedAt: String?
    let resolution: String?

    enum CodingKeys: String, CodingKey {
        case status, resolution
        case requestID = "request_id"
        case updatedAt = "updated_at"
    }
}

func fetchSuggestStatus(
    requestID: String,
    apiKey: String
) async throws -> SuggestStatusResponse {
    guard let url = URL(string: "https://places-api.foursquare.com/places/suggest/status/\(requestID)") else {
        throw URLError(.badURL)
    }

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
        return try JSONDecoder().decode(SuggestStatusResponse.self, from: data)
    } catch {
        throw FoursquareAPIError.decodingError(error)
    }
}
```
