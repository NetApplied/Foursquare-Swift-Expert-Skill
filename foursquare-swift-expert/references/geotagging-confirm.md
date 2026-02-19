# Confirm Geotagging Candidate Selection

Documentation source: https://docs.foursquare.com/fsq-developers-places/reference/geotagging-confirm

## Endpoint

- Method: `POST`
- Path: `/geotagging/confirm`
- Operation ID: `geotagging-confirm`

## Path parameters

None.

## Query parameters

| Name | Type | Required | Description | Notes |
| --- | --- | --- | --- | --- |
| request_id | string | No | The request ID pulled from the header of the /geotagging/candidates request that generated the list of candidates shown to the user. The header key is X-Fsq-Request-Id. |  |
| fsq_place_id | string | No | The FSQ Place ID of the place which was selected by the user from the candidates list |  |
| confirm_context | string | No | Specify the use case for specified request. Options are: CurrentLocation, Nearby, Destination, Search | Enum: CurrentLocation, Nearby, Destination, Search |
| delayed | boolean | No |  |  |

## Mandatory headers

| Header | Required | Value | Description |
| --- | --- | --- | --- |
| Authorization | Yes | `Bearer <FSQ_API_KEY>` | Bearer Token to authorize requests. |
| X-Places-Api-Version | Yes | `2025-06-17` | The version of the API to use. |

## Response codes

| Status | Description |
| --- | --- |
| 200 | success |

## Full JSON response schemas

### `200` schema

```json
{
  "description": "success",
  "content": {
    "application/json": {
      "schema": {
        "type": "object",
        "properties": {
          "response": {
            "type": "string"
          }
        }
      }
    }
  }
}
```

## Referenced component schemas

No referenced component schemas for this endpoint response.

## Error handling

Always gate response handling with this exact pattern and decode `error_detail` when present:

```swift
if !(200...299).contains(httpResponse.statusCode) {
    let decodedError = try? JSONDecoder().decode(FoursquareErrorResponse.self, from: data)
    throw FoursquareAPIError.httpStatus(code: httpResponse.statusCode, detail: decodedError?.errorDetail)
}
```

## Swift example

```swift
import Foundation

enum JSONValue: Codable, Hashable, Identifiable {
    case string(String)
    case int(Int)
    case double(Double)
    case bool(Bool)
    case object([String: JSONValue])
    case array([JSONValue])
    case null

    var id: String { queryEncodedString }

    var queryEncodedString: String {
        switch self {
        case .string(let value): return value
        case .int(let value): return String(value)
        case .double(let value): return String(value)
        case .bool(let value): return String(value)
        case .object(let value):
            let data = try? JSONEncoder().encode(value)
            return data.flatMap { String(data: $0, encoding: .utf8) } ?? "{}"
        case .array(let value): return value.map { $0.queryEncodedString }.joined(separator: ",")
        case .null: return "null"
        }
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.singleValueContainer()
        if container.decodeNil() {
            self = .null
        } else if let value = try? container.decode(String.self) {
            self = .string(value)
        } else if let value = try? container.decode(Int.self) {
            self = .int(value)
        } else if let value = try? container.decode(Double.self) {
            self = .double(value)
        } else if let value = try? container.decode(Bool.self) {
            self = .bool(value)
        } else if let value = try? container.decode([String: JSONValue].self) {
            self = .object(value)
        } else if let value = try? container.decode([JSONValue].self) {
            self = .array(value)
        } else {
            throw DecodingError.typeMismatch(JSONValue.self, DecodingError.Context(codingPath: decoder.codingPath, debugDescription: "Unsupported JSON value"))
        }
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.singleValueContainer()
        switch self {
        case .string(let value): try container.encode(value)
        case .int(let value): try container.encode(value)
        case .double(let value): try container.encode(value)
        case .bool(let value): try container.encode(value)
        case .object(let value): try container.encode(value)
        case .array(let value): try container.encode(value)
        case .null: try container.encodeNil()
        }
    }
}

struct FoursquareErrorResponse: Codable, Identifiable, Hashable {
    let error: String?
    let message: String?
    let errorDetail: String?

    var id: String { errorDetail ?? message ?? error ?? "foursquare-error" }

    enum CodingKeys: String, CodingKey {
        case error = "error"
        case message = "message"
        case detail = "detail"
        case errorDetail = "error_detail"
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        let rawError = try container.decodeIfPresent(String.self, forKey: .error)
        let rawMessage = try container.decodeIfPresent(String.self, forKey: .message)
        let rawDetail = try container.decodeIfPresent(String.self, forKey: .detail)
        let rawErrorDetail = try container.decodeIfPresent(String.self, forKey: .errorDetail)
        self.error = rawError
        self.message = rawMessage
        self.errorDetail = rawErrorDetail ?? rawDetail ?? rawMessage ?? rawError
    }
}

enum FoursquareAPIError: Error {
    case invalidURL
    case invalidResponse
    case httpStatus(code: Int, detail: String?)
    case decodingFailure(Error)
}

struct GeotaggingConfirmRequest: Codable, Identifiable, Hashable {
    let requestId: String?
    let fsqPlaceId: String?
    let confirmContext: String?
    let delayed: Bool?

    var id: String { "geotagging-confirm-request" }

    enum CodingKeys: String, CodingKey {
        case requestId = "request_id"
        case fsqPlaceId = "fsq_place_id"
        case confirmContext = "confirm_context"
        case delayed = "delayed"
    }
}

struct GeotaggingConfirmResponse: Codable, Identifiable, Hashable {
    let response: String?

    var id: String { "geotagging-confirm-response" }

    enum CodingKeys: String, CodingKey {
        case response = "response"
    }
}

final class GeotaggingConfirmClient {
    private let apiKey: String
    private let apiVersion: String
    private let baseURL = URL(string: "https://places-api.foursquare.com")!
    private let session: URLSession

    init(apiKey: String, apiVersion: String = "2025-06-17", session: URLSession = .shared) {
        self.apiKey = apiKey
        self.apiVersion = apiVersion
        self.session = session
    }

    func geotaggingConfirm(parameters: GeotaggingConfirmRequest) async throws -> GeotaggingConfirmResponse {
        let endpointPath = "/geotagging/confirm"
        guard var components = URLComponents(url: baseURL.appendingPathComponent(endpointPath), resolvingAgainstBaseURL: false) else {
            throw FoursquareAPIError.invalidURL
        }

        var queryItems: [URLQueryItem] = []
        if let requestId { queryItems.append(URLQueryItem(name: "request_id", value: requestId)) }
        if let fsqPlaceId { queryItems.append(URLQueryItem(name: "fsq_place_id", value: fsqPlaceId)) }
        if let confirmContext { queryItems.append(URLQueryItem(name: "confirm_context", value: confirmContext)) }
        if let delayed { queryItems.append(URLQueryItem(name: "delayed", value: String(delayed))) }
        components.queryItems = queryItems.isEmpty ? nil : queryItems

        guard let url = components.url else {
            throw FoursquareAPIError.invalidURL
        }

        var request = URLRequest(url: url)
        request.httpMethod = "POST"
        request.setValue("Bearer \(apiKey)", forHTTPHeaderField: "Authorization")
        request.setValue(apiVersion, forHTTPHeaderField: "X-Places-Api-Version")

        let (data, response) = try await session.data(for: request)
        guard let httpResponse = response as? HTTPURLResponse else {
            throw FoursquareAPIError.invalidResponse
        }

        if !(200...299).contains(httpResponse.statusCode) {
            let decodedError = try? JSONDecoder().decode(FoursquareErrorResponse.self, from: data)
                switch httpResponse.statusCode {
                default:
                    throw .httpStatus(code: httpResponse.statusCode, detail: decodedError?.errorDetail)
                }
        }

        do {
            return try JSONDecoder().decode(GeotaggingConfirmResponse.self, from: data)
        } catch {
            throw FoursquareAPIError.decodingFailure(error)
        }
    }
}
```
