# Edit a Place

Documentation source: https://docs.foursquare.com/fsq-developers-places/reference/place-suggest-edit

## Endpoint

- Method: `POST`
- Path: `/places/{fsq_place_id}/suggest/edit`
- Operation ID: `place-suggest-edit`

## Path parameters

| Name | Type | Required | Description | Notes |
| --- | --- | --- | --- | --- |
| fsq_place_id | string | Yes | A unique string identifier for a FSQ Place (formerly known as Venue ID). E.g., Foursquare HQ's fsq_id = 5a187743ccad6b307315e6fe. |  |

## Query parameters

| Name | Type | Required | Description | Notes |
| --- | --- | --- | --- | --- |
| dry_run | boolean | No | If true, return the expected result without actually submitting the suggestion. Useful for testing. **Note this defaults to *false* in all cases EXCEPT when calling through this docs page.** | Default: True |
| latitude | number | No | The proposed new value for the latitude at which the place should be located (e.g., 41.8781). | Format: double |
| longitude | number | No | The proposed new value for the longitude at which the place should be located (e.g., -87.6298). | Format: double |
| menu | string | No | The proposed new value for the url where the menu of the place can be found. |  |
| facebook_url | string | No | The proposed new value for the url for this place's Facebook Page. |  |
| parent_id | string | No | The proposed new value if the place is a subvenue of a larger place (such as a coffee shop within a Target), set this attribute to the ID of the parent place. Set to "" to remove parent. |  |
| is_private_place | boolean | No | The proposed new value for whether the place is a private place. If true, the place will be marked as private. Otherwise, it will be marked as public. |  |
| hours | string | No | The proposed new value for the hours for the venue, as a semi-colon separated list of open segments and named segments (e.g., brunch or happy hour). Open segments are formatted as day,start,end. Named segments additionally have a label, formatted as day,start,end,label. Days are formatted as integers with Monday = 1,...,Sunday = 7. Start and End are formatted as [+]HHMM format. Use 24 hour format (no colon), prefix with 0 for HH or MM less than 10. Use '+' prefix, i.e., +0230 to represent 2:30 am past midnight into the following day. To indicate that a venue is open 24/7, send this value with the hours attribute: 1,0000,2400;2,0000,2400;3,0000,2400;4,0000,2400;5,0000,2400;6,0000,2400;7,0000,2400 |  |
| name | string | No | The proposed new value for the name of the place. |  |
| description | string | No | The proposed new value for the freeform description of the place, up to 300 characters. |  |
| tel | string | No | The proposed new value for the phone number of the place. |  |
| instagram | string | No | The proposed new value for the instagram handle of the place. |  |
| twitter | string | No | The proposed new value for the twitter handle of the place. |  |
| website | string | No | The proposed new value for the url of the homepage of the place. |  |
| address | string | No | The proposed new value for the address of the place. |  |
| locality | string | No | The proposed new value for the name of the locality (city) where this place is. |  |
| region | string | No | The proposed new value for the nearest state or province to the place. |  |
| postcode | string | No | The proposed new value for the zip or postal code for the place. |  |
| country_code | string | No | The proposed new 2-digit country code where the place is located (e.g. US). |  |
| add_attributes | string | No | A comma-separated list of attribute keys to add or enable for the venue. Possible values are: {atm, reservations, offers_delivery, parking, outdoor_seating, restroom, credit_cards, wifi}. |  |
| remove_attributes | string | No | A comma-separated list of attribute keys to remove or disable from the venue. Possible values are: {atm, reservations, offers_delivery, parking, outdoor_seating, restroom, credit_cards, wifi}. |  |
| add_fsq_category_ids | string | No | Add category IDs. Supports multiple Category IDs, separated by commas. For a complete list of Foursquare Category IDs, refer to the <a href="https://docs.foursquare.com/data-products/docs/categories" target="blank">Category Taxonomy</a> page. [This endpoint prefers the 5-integer style id, but can accept the BSON style id] |  |
| remove_fsq_category_ids | string | No | Remove category IDs. Supports multiple Category IDs, separated by commas. For a complete list of Foursquare Category IDs, refer to the <a href="https://docs.foursquare.com/data-products/docs/categories" target="blank">Category Taxonomy</a> page. [This endpoint prefers the 5-integer style id, but can accept the BSON style id] |  |
| primary_fsq_category_id | string | No | Change the primary category ID For a complete list of Foursquare Category IDs, refer to the <a href="https://docs.foursquare.com/data-products/docs/categories" target="blank">Category Taxonomy</a> page. [This endpoint prefers the 5-integer style id, but can accept the BSON style id] |  |
| add_fsq_chain_ids | string | No | Add chain IDs. |  |
| remove_fsq_chain_ids | string | No | Remove Foursquare chain IDs. |  |
| primary_fsq_chain_id | string | No | Change the primary chain ID. |  |
| unset_fields | string | No | Fields to unset. Supports multiple fields, separated by commas. Possible values are: <ul><li>menu (default)</li><li>facebook_url</li><li>description</li><li>address</li><li>tel</li><li>twitter</li><li>website</li><li>fsq_chain_ids</li><li>hours</li></ul> |  |

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

struct PlaceSuggestEditRequest: Codable, Identifiable, Hashable {
    let fsqPlaceId: String
    let dryRun: Bool?
    let latitude: Double?
    let longitude: Double?
    let menu: String?
    let facebookUrl: String?
    let parentId: String?
    let isPrivatePlace: Bool?
    let hours: String?
    let name: String?
    let description: String?
    let tel: String?
    let instagram: String?
    let twitter: String?
    let website: String?
    let address: String?
    let locality: String?
    let region: String?
    let postcode: String?
    let countryCode: String?
    let addAttributes: String?
    let removeAttributes: String?
    let addFsqCategoryIds: String?
    let removeFsqCategoryIds: String?
    let primaryFsqCategoryId: String?
    let addFsqChainIds: String?
    let removeFsqChainIds: String?
    let primaryFsqChainId: String?
    let unsetFields: String?

    var id: String { fsqPlaceId }

    enum CodingKeys: String, CodingKey {
        case fsqPlaceId = "fsq_place_id"
        case dryRun = "dry_run"
        case latitude = "latitude"
        case longitude = "longitude"
        case menu = "menu"
        case facebookUrl = "facebook_url"
        case parentId = "parent_id"
        case isPrivatePlace = "is_private_place"
        case hours = "hours"
        case name = "name"
        case description = "description"
        case tel = "tel"
        case instagram = "instagram"
        case twitter = "twitter"
        case website = "website"
        case address = "address"
        case locality = "locality"
        case region = "region"
        case postcode = "postcode"
        case countryCode = "country_code"
        case addAttributes = "add_attributes"
        case removeAttributes = "remove_attributes"
        case addFsqCategoryIds = "add_fsq_category_ids"
        case removeFsqCategoryIds = "remove_fsq_category_ids"
        case primaryFsqCategoryId = "primary_fsq_category_id"
        case addFsqChainIds = "add_fsq_chain_ids"
        case removeFsqChainIds = "remove_fsq_chain_ids"
        case primaryFsqChainId = "primary_fsq_chain_id"
        case unsetFields = "unset_fields"
    }
}

struct PlaceSuggestEditResponse: Codable, Identifiable, Hashable {
    let suggestedEdits: [JSONValue]?
    let errors: [String]?

    var id: String { "place-suggest-edit-response" }

    enum CodingKeys: String, CodingKey {
        case suggestedEdits = "suggested_edits"
        case errors = "errors"
    }
}

final class PlaceSuggestEditClient {
    private let apiKey: String
    private let apiVersion: String
    private let baseURL = URL(string: "https://places-api.foursquare.com")!
    private let session: URLSession

    init(apiKey: String, apiVersion: String = "2025-06-17", session: URLSession = .shared) {
        self.apiKey = apiKey
        self.apiVersion = apiVersion
        self.session = session
    }

    func placeSuggestEdit(parameters: PlaceSuggestEditRequest) async throws -> PlaceSuggestEditResponse {
        let endpointPath = "/places/\(parameters.fsqPlaceId)/suggest/edit"
        guard var components = URLComponents(url: baseURL.appendingPathComponent(endpointPath), resolvingAgainstBaseURL: false) else {
            throw FoursquareAPIError.invalidURL
        }

        var queryItems: [URLQueryItem] = []
        if let dryRun { queryItems.append(URLQueryItem(name: "dry_run", value: String(dryRun))) }
        if let latitude { queryItems.append(URLQueryItem(name: "latitude", value: String(latitude))) }
        if let longitude { queryItems.append(URLQueryItem(name: "longitude", value: String(longitude))) }
        if let menu { queryItems.append(URLQueryItem(name: "menu", value: menu)) }
        if let facebookUrl { queryItems.append(URLQueryItem(name: "facebook_url", value: facebookUrl)) }
        if let parentId { queryItems.append(URLQueryItem(name: "parent_id", value: parentId)) }
        if let isPrivatePlace { queryItems.append(URLQueryItem(name: "is_private_place", value: String(isPrivatePlace))) }
        if let hours { queryItems.append(URLQueryItem(name: "hours", value: hours)) }
        if let name { queryItems.append(URLQueryItem(name: "name", value: name)) }
        if let description { queryItems.append(URLQueryItem(name: "description", value: description)) }
        if let tel { queryItems.append(URLQueryItem(name: "tel", value: tel)) }
        if let instagram { queryItems.append(URLQueryItem(name: "instagram", value: instagram)) }
        if let twitter { queryItems.append(URLQueryItem(name: "twitter", value: twitter)) }
        if let website { queryItems.append(URLQueryItem(name: "website", value: website)) }
        if let address { queryItems.append(URLQueryItem(name: "address", value: address)) }
        if let locality { queryItems.append(URLQueryItem(name: "locality", value: locality)) }
        if let region { queryItems.append(URLQueryItem(name: "region", value: region)) }
        if let postcode { queryItems.append(URLQueryItem(name: "postcode", value: postcode)) }
        if let countryCode { queryItems.append(URLQueryItem(name: "country_code", value: countryCode)) }
        if let addAttributes { queryItems.append(URLQueryItem(name: "add_attributes", value: addAttributes)) }
        if let removeAttributes { queryItems.append(URLQueryItem(name: "remove_attributes", value: removeAttributes)) }
        if let addFsqCategoryIds { queryItems.append(URLQueryItem(name: "add_fsq_category_ids", value: addFsqCategoryIds)) }
        if let removeFsqCategoryIds { queryItems.append(URLQueryItem(name: "remove_fsq_category_ids", value: removeFsqCategoryIds)) }
        if let primaryFsqCategoryId { queryItems.append(URLQueryItem(name: "primary_fsq_category_id", value: primaryFsqCategoryId)) }
        if let addFsqChainIds { queryItems.append(URLQueryItem(name: "add_fsq_chain_ids", value: addFsqChainIds)) }
        if let removeFsqChainIds { queryItems.append(URLQueryItem(name: "remove_fsq_chain_ids", value: removeFsqChainIds)) }
        if let primaryFsqChainId { queryItems.append(URLQueryItem(name: "primary_fsq_chain_id", value: primaryFsqChainId)) }
        if let unsetFields { queryItems.append(URLQueryItem(name: "unset_fields", value: unsetFields)) }
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
            return try JSONDecoder().decode(PlaceSuggestEditResponse.self, from: data)
        } catch {
            throw FoursquareAPIError.decodingFailure(error)
        }
    }
}
```
