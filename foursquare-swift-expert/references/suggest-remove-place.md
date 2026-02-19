# Remove a Place

Documentation source: https://docs.foursquare.com/fsq-developers-places/reference/place-suggest-remove

## Endpoint

- Method: `POST`
- Path: `/places/{fsq_place_id}/suggest/remove`
- Operation ID: `place-suggest-remove`

## Path parameters

| Name | Type | Required | Description | Notes |
| --- | --- | --- | --- | --- |
| fsq_place_id | string | Yes | A unique string identifier for a FSQ Place (formerly known as Venue ID). E.g., Foursquare HQ's fsq_place_id = 5a187743ccad6b307315e6fe. |  |

## Query parameters

| Name | Type | Required | Description | Notes |
| --- | --- | --- | --- | --- |
| dry_run | boolean | No | If true, return the expected result without actually submitting the suggestion. Useful for testing. **Note this defaults to *false* in all cases EXCEPT when calling through this docs page.** | Default: True |
| reason | string | Yes | Reason for removal. Possible values are:<ul><li>closed</li><li>doesnt_exist</li><li>inappropriate</li><li>not_closed</li><li>private</li></ul> | Enum: CLOSED, DOESNT_EXIST, INAPPROPRIATE, NOT_CLOSED, OTHER, PRIVATE |
| comment | string | No | A comment describing the removal request. |  |

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
          "suggested_edits": {
            "type": "array",
            "properties": {
              "traversable_again": {
                "type": "boolean"
              }
            },
            "items": {
              "type": "object",
              "properties": {
                "id": {
                  "type": "string"
                },
                "fsq_place_id": {
                  "type": "string"
                },
                "suggested_edit_type": {
                  "type": "string"
                },
                "created_at": {
                  "type": "string",
                  "format": "date-time"
                },
                "resolved_time": {
                  "type": "string",
                  "format": "date-time"
                },
                "rolled_back": {
                  "type": "object"
                },
                "status": {
                  "type": "string"
                },
                "created_fsq_place_id": {
                  "type": "string"
                },
                "matched_fsq_place_id": {
                  "type": "string"
                }
              }
            }
          },
          "errors": {
            "type": "array",
            "properties": {
              "traversable_again": {
                "type": "boolean"
              }
            },
            "items": {
              "type": "string"
            }
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

struct PlaceSuggestRemoveRequest: Codable, Identifiable, Hashable {
    let fsqPlaceId: String
    let dryRun: Bool?
    let reason: String
    let comment: String?

    var id: String { fsqPlaceId }

    enum CodingKeys: String, CodingKey {
        case fsqPlaceId = "fsq_place_id"
        case dryRun = "dry_run"
        case reason = "reason"
        case comment = "comment"
    }
}

struct PlaceSuggestRemoveResponse: Codable, Identifiable, Hashable {
    let suggestedEdits: [JSONValue]?
    let errors: [String]?

    var id: String { "place-suggest-remove-response" }

    enum CodingKeys: String, CodingKey {
        case suggestedEdits = "suggested_edits"
        case errors = "errors"
    }
}

final class PlaceSuggestRemoveClient {
    private let apiKey: String
    private let apiVersion: String
    private let baseURL = URL(string: "https://places-api.foursquare.com")!
    private let session: URLSession

    init(apiKey: String, apiVersion: String = "2025-06-17", session: URLSession = .shared) {
        self.apiKey = apiKey
        self.apiVersion = apiVersion
        self.session = session
    }

    func placeSuggestRemove(parameters: PlaceSuggestRemoveRequest) async throws -> PlaceSuggestRemoveResponse {
        let endpointPath = "/places/\(parameters.fsqPlaceId)/suggest/remove"
        guard var components = URLComponents(url: baseURL.appendingPathComponent(endpointPath), resolvingAgainstBaseURL: false) else {
            throw FoursquareAPIError.invalidURL
        }

        var queryItems: [URLQueryItem] = []
        if let dryRun { queryItems.append(URLQueryItem(name: "dry_run", value: String(dryRun))) }
        queryItems.append(URLQueryItem(name: "reason", value: reason))
        if let comment { queryItems.append(URLQueryItem(name: "comment", value: comment)) }
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
            return try JSONDecoder().decode(PlaceSuggestRemoveResponse.self, from: data)
        } catch {
            throw FoursquareAPIError.decodingFailure(error)
        }
    }
}
```
