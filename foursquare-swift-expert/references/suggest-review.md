# Get Places With Pending Suggested Edits

Documentation source: https://docs.foursquare.com/fsq-developers-places/reference/place-top-venue-woes

## Endpoint

- Method: `GET`
- Path: `/places/suggest/review`
- Operation ID: `place-top-venue-woes`

## Path parameters

None.

## Query parameters

| Name | Type | Required | Description | Notes |
| --- | --- | --- | --- | --- |
| near | string | No | The name of the location to search near |  |
| ne | string | No | The north-east corner of the bounding box to search within |  |
| sw | string | No | The south-west corner of the bounding box to search within |  |

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
          "venues": {
            "type": "array",
            "properties": {
              "traversable_again": {
                "type": "boolean"
              }
            },
            "items": {
              "type": "object",
              "properties": {
                "fsq_place_id": {
                  "type": "string"
                },
                "venue_name": {
                  "type": "string"
                },
                "location": {
                  "type": "object",
                  "properties": {
                    "address": {
                      "type": "string"
                    },
                    "locality": {
                      "type": "string"
                    },
                    "region": {
                      "type": "string"
                    },
                    "postcode": {
                      "type": "string"
                    },
                    "admin_region": {
                      "type": "string"
                    },
                    "post_town": {
                      "type": "string"
                    },
                    "po_box": {
                      "type": "string"
                    },
                    "country": {
                      "type": "string"
                    },
                    "formatted_address": {
                      "type": "string"
                    }
                  }
                },
                "woe_types": {
                  "uniqueItems": true,
                  "type": "array",
                  "properties": {
                    "traversable_again": {
                      "type": "boolean"
                    }
                  },
                  "items": {
                    "type": "string"
                  }
                },
                "woe_count": {
                  "type": "integer",
                  "format": "int32"
                },
                "woes": {
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
                }
              }
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

struct PlaceTopVenueWoesRequest: Codable, Identifiable, Hashable {
    let near: String?
    let ne: String?
    let sw: String?

    var id: String { "place-top-venue-woes-request" }

    enum CodingKeys: String, CodingKey {
        case near = "near"
        case ne = "ne"
        case sw = "sw"
    }
}

struct PlaceTopVenueWoesResponse: Codable, Identifiable, Hashable {
    let venues: [JSONValue]?

    var id: String { "place-top-venue-woes-response" }

    enum CodingKeys: String, CodingKey {
        case venues = "venues"
    }
}

final class PlaceTopVenueWoesClient {
    private let apiKey: String
    private let apiVersion: String
    private let baseURL = URL(string: "https://places-api.foursquare.com")!
    private let session: URLSession

    init(apiKey: String, apiVersion: String = "2025-06-17", session: URLSession = .shared) {
        self.apiKey = apiKey
        self.apiVersion = apiVersion
        self.session = session
    }

    func placeTopVenueWoes(parameters: PlaceTopVenueWoesRequest) async throws -> PlaceTopVenueWoesResponse {
        let endpointPath = "/places/suggest/review"
        guard var components = URLComponents(url: baseURL.appendingPathComponent(endpointPath), resolvingAgainstBaseURL: false) else {
            throw FoursquareAPIError.invalidURL
        }

        var queryItems: [URLQueryItem] = []
        if let near { queryItems.append(URLQueryItem(name: "near", value: near)) }
        if let ne { queryItems.append(URLQueryItem(name: "ne", value: ne)) }
        if let sw { queryItems.append(URLQueryItem(name: "sw", value: sw)) }
        components.queryItems = queryItems.isEmpty ? nil : queryItems

        guard let url = components.url else {
            throw FoursquareAPIError.invalidURL
        }

        var request = URLRequest(url: url)
        request.httpMethod = "GET"
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
            return try JSONDecoder().decode(PlaceTopVenueWoesResponse.self, from: data)
        } catch {
            throw FoursquareAPIError.decodingFailure(error)
        }
    }
}
```
