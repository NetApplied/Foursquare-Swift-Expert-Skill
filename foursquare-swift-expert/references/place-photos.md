# Get Place Photos

Documentation source: https://docs.foursquare.com/fsq-developers-places/reference/place-photos

## Endpoint

- Method: `GET`
- Path: `/places/{fsq_place_id}/photos`
- Operation ID: `place-photos`

## Path parameters

| Name | Type | Required | Description | Notes |
| --- | --- | --- | --- | --- |
| fsq_place_id | string | Yes | A unique string identifier for a FSQ Place (formerly known as Venue ID). E.g., Foursquare HQ's fsq_place_id = 5a187743ccad6b307315e6fe |  |

## Query parameters

| Name | Type | Required | Description | Notes |
| --- | --- | --- | --- | --- |
| limit | integer | No | The specified number of photos per page. Returns 10 photos by default, up to a maximum number of 50. | Max: 50; Format: int32 |
| sort | string | No | Specifies the order in which results are listed. Possible values are:<ul><li>popular (default) - sorts results based on their popularity among Foursquare users</li><li>newest - sorts results from most recently added to least recently added</li></ul> | Enum: POPULAR, NEWEST |
| classifications | string | No | Restricts the results to photos matching the specified classifications, separated by a comma. Possible values are:<ul><li>exhibit</li><li>facilities</li><li>food_or_drink</li><li>indoor_facilities_and_classrooms</li><li>indoor_general</li><li>indoor_or_ambience</li><li>logos</li><li>menu</li><li>monuments_and_landmark_buildings</li><li>outdoor</li><li>outdoor_building_and_grounds</li><li>outdoor_building_exterior</li><li>outdoor_grounds</li><li>outdoor_or_storefront</li><li>outdoor_scenery</li><li>product</li></ul> |  |

## Mandatory headers

| Header | Required | Value | Description |
| --- | --- | --- | --- |
| Authorization | Yes | `Bearer <FSQ_API_KEY>` | Bearer Token to authorize requests. |
| X-Places-Api-Version | Yes | `2025-06-17` | The version of the API to use. |

## Response codes

| Status | Description |
| --- | --- |
| 200 | success |
| 404 | invalid venue specified |

## Full JSON response schemas

### `200` schema

```json
{
  "description": "success",
  "content": {
    "application/json": {
      "schema": {
        "type": "array",
        "items": {
          "type": "object",
          "properties": {
            "id": {
              "type": "string"
            },
            "created_at": {
              "type": "string",
              "format": "date-time"
            },
            "prefix": {
              "type": "string"
            },
            "suffix": {
              "type": "string"
            },
            "width": {
              "type": "integer",
              "format": "int32"
            },
            "height": {
              "type": "integer",
              "format": "int32"
            },
            "classifications": {
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
            "tip": {
              "type": "object",
              "properties": {
                "id": {
                  "type": "string"
                },
                "created_at": {
                  "type": "string",
                  "format": "date-time"
                },
                "text": {
                  "type": "string"
                },
                "url": {
                  "type": "string"
                },
                "photo": {},
                "lang": {
                  "type": "string"
                },
                "agree_count": {
                  "type": "integer",
                  "format": "int32"
                },
                "disagree_count": {
                  "type": "integer",
                  "format": "int32"
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

### `404` schema

```json
{
  "description": "invalid venue specified"
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

struct PlacePhotosRequest: Codable, Identifiable, Hashable {
    let fsqPlaceId: String
    let limit: Int?
    let sort: String?
    let classifications: String?

    var id: String { fsqPlaceId }

    enum CodingKeys: String, CodingKey {
        case fsqPlaceId = "fsq_place_id"
        case limit = "limit"
        case sort = "sort"
        case classifications = "classifications"
    }
}

struct PlacePhotosResponse: Codable, Identifiable, Hashable {
    let payload: JSONValue?

    var id: String { "place-photos-response" }

    enum CodingKeys: String, CodingKey {
        case payload = "payload"
    }
}

final class PlacePhotosClient {
    private let apiKey: String
    private let apiVersion: String
    private let baseURL = URL(string: "https://places-api.foursquare.com")!
    private let session: URLSession

    init(apiKey: String, apiVersion: String = "2025-06-17", session: URLSession = .shared) {
        self.apiKey = apiKey
        self.apiVersion = apiVersion
        self.session = session
    }

    func placePhotos(parameters: PlacePhotosRequest) async throws -> PlacePhotosResponse {
        let endpointPath = "/places/\(parameters.fsqPlaceId)/photos"
        guard var components = URLComponents(url: baseURL.appendingPathComponent(endpointPath), resolvingAgainstBaseURL: false) else {
            throw FoursquareAPIError.invalidURL
        }

        var queryItems: [URLQueryItem] = []
        if let limit { queryItems.append(URLQueryItem(name: "limit", value: String(limit))) }
        if let sort { queryItems.append(URLQueryItem(name: "sort", value: sort)) }
        if let classifications { queryItems.append(URLQueryItem(name: "classifications", value: classifications)) }
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
                case 404:
                    throw .httpStatus(code: 404, detail: decodedError?.errorDetail)
                default:
                    throw .httpStatus(code: httpResponse.statusCode, detail: decodedError?.errorDetail)
                }
        }

        do {
            return try JSONDecoder().decode(PlacePhotosResponse.self, from: data)
        } catch {
            throw FoursquareAPIError.decodingFailure(error)
        }
    }
}
```
