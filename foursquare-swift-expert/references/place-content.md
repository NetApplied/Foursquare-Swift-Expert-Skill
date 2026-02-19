# Place Content

Covers:
- Place Details
- Place Tips
- Place Photos

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

## Place Details endpoint

- Method: `GET`
- Path: `/places/{fsq_id}`
- Full URL: `https://places-api.foursquare.com/places/{fsq_id}`
- Path params:
  - `fsq_id` (required): place identifier
- Query params:
  - `fields` (optional): field projection list
  - `session_token` (optional): client session grouping

### Typical response schema

```json
{
  "fsq_id": "5a187743ccad6b307315e6fe",
  "name": "Sample Venue",
  "timezone": "America/Los_Angeles",
  "website": "https://example.com",
  "tel": "+1 555 123 4567",
  "categories": [{ "id": 13034, "name": "Coffee Shop" }],
  "location": {
    "address": "123 Example St",
    "locality": "Oakland",
    "region": "CA",
    "country": "US",
    "formatted_address": "123 Example St, Oakland, CA 94607"
  }
}
```

### Swift models and API function

```swift
import Foundation

struct PlaceDetails: Codable, Identifiable {
    let id: String
    let name: String?
    let timezone: String?
    let website: String?
    let tel: String?
    let categories: [Category]?
    let location: LocationDetails?

    enum CodingKeys: String, CodingKey {
        case id = "fsq_id"
        case name, timezone, website, tel, categories, location
    }
}

struct LocationDetails: Codable {
    let address: String?
    let locality: String?
    let region: String?
    let country: String?
    let formattedAddress: String?

    enum CodingKeys: String, CodingKey {
        case address, locality, region, country
        case formattedAddress = "formatted_address"
    }
}

func fetchPlaceDetails(
    fsqID: String,
    fields: [String]? = nil,
    sessionToken: String? = nil,
    apiKey: String
) async throws -> PlaceDetails {
    var components = URLComponents(string: "https://places-api.foursquare.com/places/\(fsqID)")
    var items: [URLQueryItem] = []
    if let fields, !fields.isEmpty {
        items.append(URLQueryItem(name: "fields", value: fields.joined(separator: ",")))
    }
    if let sessionToken {
        items.append(URLQueryItem(name: "session_token", value: sessionToken))
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
        return try JSONDecoder().decode(PlaceDetails.self, from: data)
    } catch {
        throw FoursquareAPIError.decodingError(error)
    }
}
```

## Place Tips endpoint

- Method: `GET`
- Path: `/places/{fsq_id}/tips`
- Full URL: `https://places-api.foursquare.com/places/{fsq_id}/tips`
- Path params:
  - `fsq_id` (required)
- Query params:
  - `limit` (optional)
  - `sort` (optional)
  - `fields` (optional)

### Typical response schema

```json
{
  "results": [
    {
      "id": "5f0c2f1e5b4f5f0a3f5d7a11",
      "created_at": "2024-01-12T20:45:11.000Z",
      "text": "Great espresso and friendly staff",
      "lang": "en"
    }
  ]
}
```

### Swift models and API function

```swift
import Foundation

struct PlaceTipsResponse: Codable {
    let results: [Tip]
}

struct Tip: Codable, Identifiable {
    let id: String
    let createdAt: String?
    let text: String?
    let lang: String?

    enum CodingKeys: String, CodingKey {
        case id, text, lang
        case createdAt = "created_at"
    }
}

func fetchPlaceTips(
    fsqID: String,
    limit: Int? = nil,
    sort: String? = nil,
    fields: [String]? = nil,
    apiKey: String
) async throws -> [Tip] {
    var components = URLComponents(string: "https://places-api.foursquare.com/places/\(fsqID)/tips")
    var items: [URLQueryItem] = []
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
        return try JSONDecoder().decode(PlaceTipsResponse.self, from: data).results
    } catch {
        throw FoursquareAPIError.decodingError(error)
    }
}
```

## Place Photos endpoint

- Method: `GET`
- Path: `/places/{fsq_id}/photos`
- Full URL: `https://places-api.foursquare.com/places/{fsq_id}/photos`
- Path params:
  - `fsq_id` (required)
- Query params:
  - `limit` (optional)
  - `sort` (optional)
  - `classifications` (optional)

### Typical response schema

```json
{
  "results": [
    {
      "id": "6484f4f8f42f6f21a2f8bb2d",
      "created_at": "2024-01-10T18:33:22.000Z",
      "prefix": "https://fastly.4sqi.net/img/general/",
      "suffix": "/12345_photo.jpg",
      "width": 1440,
      "height": 1080
    }
  ]
}
```

### Swift models and API function

```swift
import Foundation

struct PlacePhotosResponse: Codable {
    let results: [PlacePhoto]
}

struct PlacePhoto: Codable, Identifiable {
    let id: String
    let createdAt: String?
    let prefix: String?
    let suffix: String?
    let width: Int?
    let height: Int?

    enum CodingKeys: String, CodingKey {
        case id, prefix, suffix, width, height
        case createdAt = "created_at"
    }

    func imageURL(size: String = "original") -> URL? {
        guard let prefix, let suffix else { return nil }
        return URL(string: "\(prefix)\(size)\(suffix)")
    }
}

func fetchPlacePhotos(
    fsqID: String,
    limit: Int? = nil,
    sort: String? = nil,
    classifications: [String]? = nil,
    apiKey: String
) async throws -> [PlacePhoto] {
    var components = URLComponents(string: "https://places-api.foursquare.com/places/\(fsqID)/photos")
    var items: [URLQueryItem] = []
    if let limit { items.append(URLQueryItem(name: "limit", value: String(limit))) }
    if let sort { items.append(URLQueryItem(name: "sort", value: sort)) }
    if let classifications, !classifications.isEmpty {
        items.append(URLQueryItem(name: "classifications", value: classifications.joined(separator: ",")))
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
        return try JSONDecoder().decode(PlacePhotosResponse.self, from: data).results
    } catch {
        throw FoursquareAPIError.decodingError(error)
    }
}
```
