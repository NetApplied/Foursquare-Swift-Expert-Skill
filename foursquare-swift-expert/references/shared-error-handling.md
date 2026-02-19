# Shared Error Handling

Use these shared error models and helpers across all Foursquare endpoint integrations.

## Included Types

- `FoursquareAPIError`
- `FoursquareErrorResponse`
- `handleHTTPErrorIfNeeded(data:httpResponse:)`

## Swift Error Models

```swift
import Foundation

enum FoursquareAPIError: Error, Hashable {
    case invalidURL
    case invalidResponse
    case apiError(statusCode: Int, error: FoursquareErrorResponse?)
    case decoding(String)
    case unexpectedStatusCode(Int)
}

struct FoursquareErrorResponse: Codable, Identifiable, Hashable {
    let errorDetail: String?

    var id: String { errorDetail ?? "unknown-error" }

    enum CodingKeys: String, CodingKey {
        case errorDetail = "error_detail"
    }
}

func handleHTTPErrorIfNeeded(data: Data, httpResponse: HTTPURLResponse) throws {
    if !(200...299).contains(httpResponse.statusCode) {
        let apiError = try? JSONDecoder().decode(FoursquareErrorResponse.self, from: data)
        throw FoursquareAPIError.apiError(statusCode: httpResponse.statusCode, error: apiError)
    }
}
```

## Usage Notes

- Keep endpoint-specific response-code documentation in each endpoint file.
- For non-2xx responses, always decode `FoursquareErrorResponse` before throwing `FoursquareAPIError.apiError`.
- Continue to set `Authorization` and `X-Places-Api-Version` on all requests.
