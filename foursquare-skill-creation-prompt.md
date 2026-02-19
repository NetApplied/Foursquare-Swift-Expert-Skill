**Role**: Senior Technical Writer and Swift Developer.

**Task**: Create a modular **Agent Skill** directory at `foursquare-swift-expert/`. This skill will serve as the primary knowledge base for implementing Foursquare Places API integration in Swift.

**1. Main Manifest (`foursquare-swift-expert/SKILL.md`)**:

* **Frontmatter**: Include `name: foursquare-swift-expert` and a description: "Comprehensive Swift-ready documentation and implementation library for the Foursquare Places API."
* **Primary Reference**: Foursquare Places API Overview.
* **Logic**: Instruct the agent to reference the subfolder `./references/` for all endpoint-specific queries.
* **Swift Standards**: Enforce `async/await`, `URLSession`, and `Codable` models with `CodingKeys` to explicitly map from snake_case JSON to camelCase Swift. All Swift models should implement `Codable`, `Identifiable`, and `Hashable`.   

**2. Resource Subfolder (`foursquare-swift-expert/references/`)**:
Generate individual markdown files for each resource. Each file must include documentation for path/query parameters, mandatory headers, full JSON response schemas, and complete Swift example code following specified **Swift Standards**. Every endpoint documentation must include a dedicated **Error Handling** section to implement FoursquareAPIError and FoursquareErrorResponse (parsing error_detail). Use the logic: if !(200...299).contains(httpResponse.statusCode) { decode FoursquareErrorResponse }. Every endpoint documentation must handle all response codes for that endpoint. Only use documentation from the provided documentation-source for each resource. 


* `common-data.md`: Covers Categories, Chains, and Response Fields.
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/response-fields
* `autocomplete.md`: Covers Autocomplete API endpoint
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/autocomplete
* `place-search.md`: Covers Place Search API endpoint.
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/place-search
* `place-details.md`: Covers Place Details API endpoint.
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/place-details
* `place-tips.md`: Covers Place Tips API endpoint.
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/place-tips
* `place-photos.md`: Covers Place Photos API endpoint.
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/place-photos
* `place-flag.md`: Covers Flag Place API endpoint:
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/place-flag
* `suggest-merge-places.md`: Covers Suggest Merge Places API endpoint.
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/suggest-merge
* `suggest-edit-place.md`: Covers Suggest Edit Place API endpoint.
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/place-suggest-edit
* `suggest-remove-place.md`: Covers Suggest Remove Place API endpoint.
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/place-suggest-remove
* `suggest-new-place.md`: Covers Suggest New Place API endpoint.
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/places-suggest-place
* `suggest-status.md`: Covers Get Suggest Status API endpoint.
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/place-suggest-status
* `suggest-review.md`: Covers Get Places With Pending Suggested Edits API endpoint.
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/place-top-venue-woes
* `geotagging-candidates.md`: Covers Find Geotagging Candidates API endpoint.
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/geotagging-candidates
* `geotagging-confirm.md`: Covers Confirm Geotagging Candidate Selection API endpoint.
    - documentation-source: https://docs.foursquare.com/fsq-developers-places/reference/geotagging-confirm

**Output**: Create the directory structure and all markdown files listed above.

