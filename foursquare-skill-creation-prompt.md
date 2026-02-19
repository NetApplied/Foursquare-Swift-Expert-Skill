**Role**: Senior Technical Writer and Swift Developer.

**Task**: Create a modular **Agent Skill** directory at `.agent/skills/foursquare-swift-expert/`. This skill will serve as the primary knowledge base for implementing Foursquare Places API integration in Swift.

**1. Main Manifest (`.agent/skills/foursquare-swift-expert/SKILL.md`)**:

* **Frontmatter**: Include `name: foursquare-swift-expert` and a description: "Comprehensive Swift-ready documentation and implementation library for the Foursquare Places API."
* **Primary Reference**: Foursquare Places API Overview.
* **Logic**: Instruct the agent to reference the subfolder `./references/` for all endpoint-specific queries.
* **Swift Standards**: Enforce `async/await`, `URLSession`, and `Codable` models with `CodingKeys` to map `fsq_id` to `id`.

**2. Resource Subfolder (`.agent/skills/foursquare-swift-expert/references/`)**:
Generate individual markdown files for each resource. Each file must include path/query parameters, mandatory headers, full JSON response schemas, and complete Swift code.

* `core-data.md`: Covers Categories, Chains, and Response Fields.
* `search-and-discovery.md`: Covers Autocomplete and Place Search.
* `place-content.md`: Covers Place Details, Tips, and Photos.
* `crowdsourcing.md`: Covers Suggest Merge, Suggest Edit, Suggest Remove, and Flagging.
* `suggest-and-status.md`: Covers Suggest Place and Suggest Status.
* `geotagging.md`: Covers Geotagging Candidates.

**3. Swift Implementation Baseline**:
Use the following pattern for all Swift models and API functions to ensure consistency across the skill.

All reference files must strictly follow this pattern:

* **Swift Codable Models**:

```
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

* **Swift Usage Example**:

```
func fetchGeotaggingCandidates(lat: Double, lng: Double, apiKey: String) async throws -> [PlaceCandidate] {
    let urlString = "https://api.foursquare.com\(lat),\(lng)"
    guard let url = URL(string: urlString) else { throw URLError(.badURL) }

    var request = URLRequest(url: url)
    request.httpMethod = "GET"
    request.addValue("application/json", forHTTPHeaderField: "accept")
    request.addValue(apiKey, forHTTPHeaderField: "Authorization")

    let (data, response) = try await URLSession.shared.data(for: request)

    guard let httpResponse = response as? HTTPURLResponse, httpResponse.statusCode == 200 else {
        throw URLError(.badServerResponse)
    }

    let decodedResponse = try JSONDecoder().decode(GeotaggingResponse.self, from: data)
    return decodedResponse.results
}
```

**4. Swift Error Handling Baseline**:
Every endpoint documentation must include a dedicated **Error Handling** section with the following Swift implementation.

* **Four Square Error Model**

```
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
        case .badRequest(let detail): return "Bad Request: \(detail ?? "Invalid parameters")"
        case .unauthorized: return "Unauthorized: Check your API Key."
        case .forbidden(let detail): return "Forbidden: \(detail ?? "Access restricted")"
        case .notFound: return "Resource not found."
        case .serverError(let code): return "Foursquare server error (Status: \(code))"
        case .decodingError(let error): return "Failed to decode response: \(error.localizedDescription)"
        case .unknown(let code): return "An unknown error occurred (Status: \(code))"
        }
    }
}
```

* **Standardized Decoding Logic**:

```
// In each API function, use this pattern after receiving (data, response):
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


*All reference files in `./references/` must follow the coding style, naming conventions, and [Response Field](https://docs.foursquare.com/developer/reference/response-fields) mappings demonstrated in this baseline.*

**Output**: Create the directory structure and all markdown files listed above.

