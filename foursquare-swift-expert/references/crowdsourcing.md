# Crowdsourcing

Covers contribution endpoints:
- Suggest Merge
- Suggest Edit
- Suggest Remove
- Flagging

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

## Shared request/response payloads

### Typical contribution response schema

```json
{
  "status": "submitted",
  "request_id": "9dce73a9-c8f3-4e7f-b8a6-c4f5f4e831f0",
  "message": "Contribution received",
  "created_at": "2024-02-10T12:22:11.000Z"
}
```

### Swift shared models

```swift
import Foundation

struct ContributionResponse: Codable {
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

struct SuggestEditPayload: Codable {
    let name: String?
    let address: String?
    let latitude: Double?
    let longitude: Double?
    let categoryIDs: [Int]?

    enum CodingKeys: String, CodingKey {
        case name, address, latitude, longitude
        case categoryIDs = "category_ids"
    }
}

struct SuggestRemovePayload: Codable {
    let reason: String
    let comment: String?
}

struct SuggestMergePayload: Codable {
    let targetFSQID: String
    let duplicateFSQID: String
    let comment: String?

    enum CodingKeys: String, CodingKey {
        case comment
        case targetFSQID = "target_fsq_id"
        case duplicateFSQID = "duplicate_fsq_id"
    }
}

struct FlagPayload: Codable {
    let reason: String
    let comment: String?
}

private func postContribution<T: Encodable>(
    path: String,
    payload: T,
    apiKey: String
) async throws -> ContributionResponse {
    guard let url = URL(string: "https://places-api.foursquare.com\(path)") else {
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
        return try JSONDecoder().decode(ContributionResponse.self, from: data)
    } catch {
        throw FoursquareAPIError.decodingError(error)
    }
}
```

## Suggest Edit endpoint

- Method: `POST`
- Path: `/places/{fsq_id}/suggest-edit`
- Full URL: `https://places-api.foursquare.com/places/{fsq_id}/suggest-edit`
- Path params:
  - `fsq_id` (required)
- Query params: none

```swift
func suggestPlaceEdit(
    fsqID: String,
    payload: SuggestEditPayload,
    apiKey: String
) async throws -> ContributionResponse {
    try await postContribution(path: "/places/\(fsqID)/suggest-edit", payload: payload, apiKey: apiKey)
}
```

## Suggest Remove endpoint

- Method: `POST`
- Path: `/places/{fsq_id}/suggest-remove`
- Full URL: `https://places-api.foursquare.com/places/{fsq_id}/suggest-remove`
- Path params:
  - `fsq_id` (required)
- Query params: none

```swift
func suggestPlaceRemove(
    fsqID: String,
    payload: SuggestRemovePayload,
    apiKey: String
) async throws -> ContributionResponse {
    try await postContribution(path: "/places/\(fsqID)/suggest-remove", payload: payload, apiKey: apiKey)
}
```

## Suggest Merge endpoint

- Method: `POST`
- Path: `/places/suggest-merge`
- Full URL: `https://places-api.foursquare.com/places/suggest-merge`
- Path params: none
- Query params: none

```swift
func suggestPlaceMerge(
    payload: SuggestMergePayload,
    apiKey: String
) async throws -> ContributionResponse {
    try await postContribution(path: "/places/suggest-merge", payload: payload, apiKey: apiKey)
}
```

## Flagging endpoint

- Method: `POST`
- Path: `/places/{fsq_id}/flag`
- Full URL: `https://places-api.foursquare.com/places/{fsq_id}/flag`
- Path params:
  - `fsq_id` (required)
- Query params: none

```swift
func flagPlace(
    fsqID: String,
    payload: FlagPayload,
    apiKey: String
) async throws -> ContributionResponse {
    try await postContribution(path: "/places/\(fsqID)/flag", payload: payload, apiKey: apiKey)
}
```

## Notes

Use the live Foursquare reference to confirm final contribution endpoint availability and payload keys for your account plan before production rollout.
