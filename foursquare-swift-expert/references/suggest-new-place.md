# Suggest a New Place

Documentation source: https://docs.foursquare.com/fsq-developers-places/reference/places-suggest-place

## Endpoint

- Method: `POST`
- Path: `/places/suggest/place`
- Operation ID: `places-suggest-place`

## Path parameters

None.

## Query parameters

| Name | Type | Required | Description | Notes |
| --- | --- | --- | --- | --- |
| name | string | Yes | The proposed value for the name of the new place. |  |
| categories | string | No | Categories to search for. Supports multiple Category IDs, separated by commas. For a complete list of Foursquare Category IDs, refer to the <a href="https://docs.foursquare.com/data-products/docs/categories" target="blank">Category Taxonomy</a> page. |  |
| address | string | No | The proposed value for the address of the new place. |  |
| locality | string | No | The proposed value for the name of the locality (city) where this new place is. |  |
| region | string | No | The proposed value for the nearest state or province to the new place. |  |
| postcode | string | No | The value for the zip or postal code for the new place. |  |
| country_code | string | No | The 2-digit country code where the place is located (e.g. US). |  |
| latitude | number | No | The proposed value for the latitude at which the new place should be located (e.g., 41.8781). | Format: double |
| longitude | number | No | The proposed value for the longitude at which the new place should be located (e.g., -87.6298). | Format: double |
| chains | string | No | The proposed chain ids to be assosciated with this new place |  |
| parent_id | string | No | The proposed value if the new place is a subvenue of a larger place (such as a coffee shop within a Target), set this attribute to the ID of the parent place. |  |
| is_private_place | boolean | No | If true, the new place will be marked as private. |  |
| tel | string | No | The proposed new value for the phone number of the place. |  |
| website | string | No | The proposed value for the url of the homepage of the new place. |  |
| email | string | No | The proposed value for the email address for this new place |  |
| facebook_url | string | No | The proposed value for the url for this new place's Facebook Page. |  |
| instagram | string | No | The proposed value for the instagram handle for this new place |  |
| twitter | string | No | The proposed value for the twitter handle of the new place. |  |
| hours | string | No | The proposed value for the hours for the new venue, as a semi-colon separated list of open segments and named segments (e.g., brunch or happy hour). Open segments are formatted as day,start,end. Named segments additionally have a label, formatted as day,start,end,label. Days are formatted as integers with Monday = 1,...,Sunday = 7. Start and End are formatted as [+]HHMM format. Use 24 hour format (no colon), prefix with 0 for HH or MM less than 10. Use '+' prefix, i.e., +0230 to represent 2:30 am past midnight into the following day. To indicate that a venue is open 24/7, send this value with the hours attribute: 1,0000,2400;2,0000,2400;3,0000,2400;4,0000,2400;5,0000,2400;6,0000,2400;7,0000,2400 |  |
| attributes | string | No | Comma seperated list that represents the attributes that the new place has. Possible values are: {atm, reservation, offers_delivery, parking, outdoor_seating, restroom, credit_cards, wifi}. |  |
| dry_run | boolean | No | If true, return the expected result without actually submitting the suggestion. Useful for testing. **Note this defaults to *false* in all cases EXCEPT when calling through this docs page.** | Default: True |

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
          "new_place_suggestion": {
            "type": "object",
            "properties": {
              "id": {
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
              "status": {
                "type": "string"
              }
            }
          },
          "matched_fsq_place": {
            "type": "object",
            "properties": {
              "fsq_place_id": {
                "type": "string"
              },
              "name": {
                "type": "string"
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

struct PlacesSuggestPlaceRequest: Codable, Identifiable, Hashable {
    let name: String
    let categories: String?
    let address: String?
    let locality: String?
    let region: String?
    let postcode: String?
    let countryCode: String?
    let latitude: Double?
    let longitude: Double?
    let chains: String?
    let parentId: String?
    let isPrivatePlace: Bool?
    let tel: String?
    let website: String?
    let email: String?
    let facebookUrl: String?
    let instagram: String?
    let twitter: String?
    let hours: String?
    let attributes: String?
    let dryRun: Bool?

    var id: String { "places-suggest-place-request" }

    enum CodingKeys: String, CodingKey {
        case name = "name"
        case categories = "categories"
        case address = "address"
        case locality = "locality"
        case region = "region"
        case postcode = "postcode"
        case countryCode = "country_code"
        case latitude = "latitude"
        case longitude = "longitude"
        case chains = "chains"
        case parentId = "parent_id"
        case isPrivatePlace = "is_private_place"
        case tel = "tel"
        case website = "website"
        case email = "email"
        case facebookUrl = "facebook_url"
        case instagram = "instagram"
        case twitter = "twitter"
        case hours = "hours"
        case attributes = "attributes"
        case dryRun = "dry_run"
    }
}

struct PlacesSuggestPlaceResponse: Codable, Identifiable, Hashable {
    let newPlaceSuggestion: JSONValue?
    let matchedFsqPlace: JSONValue?
    let errors: [String]?

    var id: String { "places-suggest-place-response" }

    enum CodingKeys: String, CodingKey {
        case newPlaceSuggestion = "new_place_suggestion"
        case matchedFsqPlace = "matched_fsq_place"
        case errors = "errors"
    }
}

final class PlacesSuggestPlaceClient {
    private let apiKey: String
    private let apiVersion: String
    private let baseURL = URL(string: "https://places-api.foursquare.com")!
    private let session: URLSession

    init(apiKey: String, apiVersion: String = "2025-06-17", session: URLSession = .shared) {
        self.apiKey = apiKey
        self.apiVersion = apiVersion
        self.session = session
    }

    func placesSuggestPlace(parameters: PlacesSuggestPlaceRequest) async throws -> PlacesSuggestPlaceResponse {
        let endpointPath = "/places/suggest/place"
        guard var components = URLComponents(url: baseURL.appendingPathComponent(endpointPath), resolvingAgainstBaseURL: false) else {
            throw FoursquareAPIError.invalidURL
        }

        var queryItems: [URLQueryItem] = []
        queryItems.append(URLQueryItem(name: "name", value: name))
        if let categories { queryItems.append(URLQueryItem(name: "categories", value: categories)) }
        if let address { queryItems.append(URLQueryItem(name: "address", value: address)) }
        if let locality { queryItems.append(URLQueryItem(name: "locality", value: locality)) }
        if let region { queryItems.append(URLQueryItem(name: "region", value: region)) }
        if let postcode { queryItems.append(URLQueryItem(name: "postcode", value: postcode)) }
        if let countryCode { queryItems.append(URLQueryItem(name: "country_code", value: countryCode)) }
        if let latitude { queryItems.append(URLQueryItem(name: "latitude", value: String(latitude))) }
        if let longitude { queryItems.append(URLQueryItem(name: "longitude", value: String(longitude))) }
        if let chains { queryItems.append(URLQueryItem(name: "chains", value: chains)) }
        if let parentId { queryItems.append(URLQueryItem(name: "parent_id", value: parentId)) }
        if let isPrivatePlace { queryItems.append(URLQueryItem(name: "is_private_place", value: String(isPrivatePlace))) }
        if let tel { queryItems.append(URLQueryItem(name: "tel", value: tel)) }
        if let website { queryItems.append(URLQueryItem(name: "website", value: website)) }
        if let email { queryItems.append(URLQueryItem(name: "email", value: email)) }
        if let facebookUrl { queryItems.append(URLQueryItem(name: "facebook_url", value: facebookUrl)) }
        if let instagram { queryItems.append(URLQueryItem(name: "instagram", value: instagram)) }
        if let twitter { queryItems.append(URLQueryItem(name: "twitter", value: twitter)) }
        if let hours { queryItems.append(URLQueryItem(name: "hours", value: hours)) }
        if let attributes { queryItems.append(URLQueryItem(name: "attributes", value: attributes)) }
        if let dryRun { queryItems.append(URLQueryItem(name: "dry_run", value: String(dryRun))) }
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
            return try JSONDecoder().decode(PlacesSuggestPlaceResponse.self, from: data)
        } catch {
            throw FoursquareAPIError.decodingFailure(error)
        }
    }
}
```
