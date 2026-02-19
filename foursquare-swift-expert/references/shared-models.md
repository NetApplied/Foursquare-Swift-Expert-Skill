# Shared Swift Models

Use these base models across endpoint integrations to avoid repeating the same `Place`, `Category`, and `Location` definitions in every resource file.

## Included Core Models

- `Place`
- `Category`
- `Location`
- `Icon`
- `Chain`
- `ExtendedLocation`
- `SocialMedia`
- `RelatedPlaces`
- `RelatedPlace`
- `Coordinate`
- `GeoCircle`
- `GeoBounds`

## Swift Models

```swift
import Foundation

struct Place: Codable, Identifiable, Hashable {
    let fsqPlaceID: String
    let latitude: Double?
    let longitude: Double?
    let categories: [Category]?
    let chains: [Chain]?
    let dateClosed: String?
    let dateCreated: String?
    let dateRefreshed: String?
    let distance: Int?
    let email: String?
    let extendedLocation: ExtendedLocation?
    let link: String?
    let location: Location?
    let name: String?
    let placemakerURL: String?
    let relatedPlaces: RelatedPlaces?
    let socialMedia: SocialMedia?
    let storeID: String?
    let tel: String?
    let website: String?

    var id: String { fsqPlaceID }

    enum CodingKeys: String, CodingKey {
        case fsqPlaceID = "fsq_place_id"
        case latitude = "latitude"
        case longitude = "longitude"
        case categories = "categories"
        case chains = "chains"
        case dateClosed = "date_closed"
        case dateCreated = "date_created"
        case dateRefreshed = "date_refreshed"
        case distance = "distance"
        case email = "email"
        case extendedLocation = "extended_location"
        case link = "link"
        case location = "location"
        case name = "name"
        case placemakerURL = "placemaker_url"
        case relatedPlaces = "related_places"
        case socialMedia = "social_media"
        case storeID = "store_id"
        case tel = "tel"
        case website = "website"
    }
}

struct Category: Codable, Identifiable, Hashable {
    let fsqCategoryID: String
    let name: String
    let shortName: String
    let pluralName: String
    let icon: Icon

    var id: String { fsqCategoryID }

    enum CodingKeys: String, CodingKey {
        case fsqCategoryID = "fsq_category_id"
        case name = "name"
        case shortName = "short_name"
        case pluralName = "plural_name"
        case icon = "icon"
    }
}

struct Location: Codable, Identifiable, Hashable {
    let address: String?
    let locality: String?
    let region: String?
    let postcode: String?
    let country: String?
    let formattedAddress: String?

    var id: String {
        [address, locality, region, postcode, country]
            .compactMap { $0 }
            .joined(separator: "|")
    }

    enum CodingKeys: String, CodingKey {
        case address = "address"
        case locality = "locality"
        case region = "region"
        case postcode = "postcode"
        case country = "country"
        case formattedAddress = "formatted_address"
    }
}

struct Coordinate: Codable, Identifiable, Hashable {
    let latitude: Double
    let longitude: Double

    var id: String { "\(latitude),\(longitude)" }

    enum CodingKeys: String, CodingKey {
        case latitude = "latitude"
        case longitude = "longitude"
    }
}

struct GeoCircle: Codable, Identifiable, Hashable {
    let center: Coordinate
    let radius: Int

    var id: String { "\(center.latitude),\(center.longitude),\(radius)" }

    enum CodingKeys: String, CodingKey {
        case center = "center"
        case radius = "radius"
    }
}

struct GeoBounds: Codable, Identifiable, Hashable {
    let circle: GeoCircle?

    var id: Int { hashValue }

    enum CodingKeys: String, CodingKey {
        case circle = "circle"
    }
}

struct Icon: Codable, Identifiable, Hashable {
    let prefix: String
    let suffix: String

    var id: String { prefix + suffix }

    enum CodingKeys: String, CodingKey {
        case prefix = "prefix"
        case suffix = "suffix"
    }
}

struct Chain: Codable, Identifiable, Hashable {
    let fsqChainID: String
    let name: String

    var id: String { fsqChainID }

    enum CodingKeys: String, CodingKey {
        case fsqChainID = "fsq_chain_id"
        case name = "name"
    }
}

struct ExtendedLocation: Codable, Identifiable, Hashable {
    let dma: String?
    let censusBlockID: String?

    var id: String { "\(dma ?? "")|\(censusBlockID ?? "")" }

    enum CodingKeys: String, CodingKey {
        case dma = "dma"
        case censusBlockID = "census_block_id"
    }
}

struct SocialMedia: Codable, Identifiable, Hashable {
    let facebookID: String?
    let instagram: String?
    let twitter: String?

    var id: String { "\(facebookID ?? "")|\(instagram ?? "")|\(twitter ?? "")" }

    enum CodingKeys: String, CodingKey {
        case facebookID = "facebook_id"
        case instagram = "instagram"
        case twitter = "twitter"
    }
}

struct RelatedPlaces: Codable, Identifiable, Hashable {
    let parent: [RelatedPlace]?
    let children: [RelatedPlace]?

    var id: Int { hashValue }

    enum CodingKeys: String, CodingKey {
        case parent = "parent"
        case children = "children"
    }
}

struct RelatedPlace: Codable, Identifiable, Hashable {
    let fsqPlaceID: String?
    let categories: [Category]?
    let name: String?

    var id: String {
        if let fsqPlaceID {
            return fsqPlaceID
        }
        return name ?? "related-place"
    }

    enum CodingKeys: String, CodingKey {
        case fsqPlaceID = "fsq_place_id"
        case categories = "categories"
        case name = "name"
    }
}
```
