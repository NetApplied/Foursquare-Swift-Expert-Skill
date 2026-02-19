## **Role**: Senior Technical Writer and Swift Developer.

## **Task**: Create a modular **Agent Skill** directory at `foursquare-swift-expert/`. This skill will serve as the primary knowledge base for implementing Foursquare Places API integration in Swift.

## 1. Main Manifest (`foursquare-swift-expert/SKILL.md`):

**Frontmatter**: Include `name: foursquare-swift-expert` and a description: "Comprehensive Swift-ready documentation and implementation library for the Foursquare Places API."

**Primary Reference**: Foursquare Places API Overview.

**Logic**: Instruct the agent to reference the subfolder `./references/` for all endpoint-specific queries.

**Swift Standards**: Enforce `async/await`, `URLSession`, and `Codable` models with `CodingKeys` to explicitly map from snake_case JSON to camelCase Swift. All Swift models should implement `Codable`, `Identifiable`, and `Hashable`.   

## 2. Resource Subfolder (`foursquare-swift-expert/references/`):
Generate individual markdown files for each resource. Each file must include documentation for path/query parameters, mandatory headers, and complete Swift example code following specified **Swift Standards**. 

Every endpoint documentation must include a dedicated **Error Handling** section to implement FoursquareAPIError and FoursquareErrorResponse (parsing error_detail). Use the logic: if !(200...299).contains(httpResponse.statusCode) { decode FoursquareErrorResponse }. Every endpoint documentation must handle all response codes for that endpoint. 

Only use documentation from the provided documentation-source for each resource. For Swift data models, use the schema from **example-json-response** given for each endpoint.

For Swift example code that performs requests follow this coding style:
```
import Foundation

// 1. Safe URL Construction
let urlString = "https://places-api.foursquare.com/geotagging/confirm"
guard var components = URLComponents(string: urlString) else {
    fatalError("Invalid URL")
}

// 2. Define your query items
let newQueryItems = [
    URLQueryItem(name: "request_id", value: "699748d64f7117591e514117"),
    URLQueryItem(name: "fsq_place_id", value: "4ca61465a6e08cfa9d087794"),
    URLQueryItem(name: "confirm_context", value: "CurrentLocation"),
    URLQueryItem(name: "delayed", value: "false")
]

// 3. The best way to append: nil-coalesce to an empty array first
components.queryItems = (components.queryItems ?? []) + newQueryItems

// 4. Create the request
if let url = components.url {
    let request = URLRequest(url: url)
    // Use request...
}
```

### Autocomplete API Endpoint Resource
`autocomplete.md`: Covers Autocomplete API endpoint

**documentation-source**: https://docs.foursquare.com/fsq-developers-places/reference/autocomplete

**example-json-response**:
```
{
  "results": [
    {
      "type": "place",
      "text": {
        "primary": "Cloister Restaurant",
        "secondary": "607 W Main St, Ephrata, PA 17522",
        "highlight": [
          {
            "start": 9,
            "length": 7
          }
        ]
      },
      "link": "/places/4dc548e6d4c0ad9c0f831d5c",
      "place": {
        "fsq_place_id": "4dc548e6d4c0ad9c0f831d5c",
        "latitude": 40.18407794925135,
        "longitude": -76.1853263301531,
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d14e941735",
            "name": "American Restaurant",
            "short_name": "American",
            "plural_name": "American Restaurants",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/food/default_",
              "suffix": ".png"
            }
          }
        ],
        "distance": 2926,
        "location": {
          "address": "607 W Main St",
          "locality": "Ephrata",
          "region": "PA",
          "postcode": "17522",
          "country": "US",
          "formatted_address": "607 W Main St, Ephrata, PA 17522"
        },
        "name": "Cloister Restaurant"
      }
    },
    {
      "type": "place",
      "text": {
        "primary": "Arments Restaurant",
        "secondary": "36 Akron Rd, Ephrata, PA 17522",
        "highlight": [
          {
            "start": 8,
            "length": 7
          }
        ]
      },
      "link": "/places/4f328c3819836c91c7e2801a",
      "place": {
        "fsq_place_id": "4f328c3819836c91c7e2801a",
        "latitude": 40.17131876931908,
        "longitude": -76.17333517504936,
        "categories": [
          {
            "fsq_category_id": "4d4b7105d754a06374d81259",
            "name": "Restaurant",
            "short_name": "Restaurant",
            "plural_name": "Restaurants",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/food/default_",
              "suffix": ".png"
            }
          }
        ],
        "distance": 2672,
        "location": {
          "address": "36 Akron Rd",
          "locality": "Ephrata",
          "region": "PA",
          "postcode": "17522",
          "country": "US",
          "formatted_address": "36 Akron Rd, Ephrata, PA 17522"
        },
        "name": "Arments Restaurant"
      }
    },
    {
      "type": "place",
      "text": {
        "primary": "Pancake Farm Restaurant",
        "secondary": "1032 S State St, Ephrata, PA 17522",
        "highlight": [
          {
            "start": 13,
            "length": 7
          }
        ]
      },
      "link": "/places/4c2f560b66e40f47b7acc18b",
      "place": {
        "fsq_place_id": "4c2f560b66e40f47b7acc18b",
        "latitude": 40.17089357381298,
        "longitude": -76.19755920282962,
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d143941735",
            "name": "Breakfast Spot",
            "short_name": "Breakfast",
            "plural_name": "Breakfast Spots",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/food/breakfast_",
              "suffix": ".png"
            }
          }
        ],
        "distance": 1164,
        "location": {
          "address": "1032 S State St",
          "locality": "Ephrata",
          "region": "PA",
          "postcode": "17522",
          "country": "US",
          "formatted_address": "1032 S State St (at S Reading Rd), Ephrata, PA 17522"
        },
        "name": "Pancake Farm Restaurant"
      }
    },
    {
      "type": "place",
      "text": {
        "primary": "Tokyo Asian Restaurant",
        "secondary": "433 N Reading Rd, Ephrata, PA 17522",
        "highlight": [
          {
            "start": 12,
            "length": 7
          }
        ]
      },
      "link": "/places/4c9245b51adc3704800d37d1",
      "place": {
        "fsq_place_id": "4c9245b51adc3704800d37d1",
        "latitude": 40.19318223253933,
        "longitude": -76.1730840263892,
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d111941735",
            "name": "Japanese Restaurant",
            "short_name": "Japanese",
            "plural_name": "Japanese Restaurants",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/food/japanese_",
              "suffix": ".png"
            }
          },
          {
            "fsq_category_id": "4bf58dd8d48988d1d2941735",
            "name": "Sushi Restaurant",
            "short_name": "Sushi",
            "plural_name": "Sushi Restaurants",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/food/sushi_",
              "suffix": ".png"
            }
          }
        ],
        "distance": 4332,
        "location": {
          "address": "433 N Reading Rd",
          "locality": "Ephrata",
          "region": "PA",
          "postcode": "17522",
          "country": "US",
          "formatted_address": "433 N Reading Rd, Ephrata, PA 17522"
        },
        "name": "Tokyo Asian Restaurant"
      }
    },
    {
      "type": "place",
      "text": {
        "primary": "White Swan Restaurant",
        "secondary": "1264 E Newport Rd, Lititz, PA 17543",
        "highlight": [
          {
            "start": 11,
            "length": 7
          }
        ]
      },
      "link": "/places/4c9fcfe28afca09344960d16",
      "place": {
        "fsq_place_id": "4c9fcfe28afca09344960d16",
        "latitude": 40.151595054528244,
        "longitude": -76.25559981761202,
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d14e941735",
            "name": "American Restaurant",
            "short_name": "American",
            "plural_name": "American Restaurants",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/food/default_",
              "suffix": ".png"
            }
          }
        ],
        "distance": 4693,
        "location": {
          "address": "1264 E Newport Rd",
          "locality": "Lititz",
          "region": "PA",
          "postcode": "17543",
          "country": "US",
          "formatted_address": "1264 E Newport Rd, Lititz, PA 17543"
        },
        "name": "White Swan Restaurant"
      }
    },
    {
      "type": "place",
      "text": {
        "primary": "La Promesa Restaurant",
        "secondary": "52 E Main St, Ephrata, PA 17522",
        "highlight": [
          {
            "start": 11,
            "length": 7
          }
        ]
      },
      "link": "/places/52a20cfd498e9ade920b7917",
      "place": {
        "fsq_place_id": "52a20cfd498e9ade920b7917",
        "latitude": 40.17790926659946,
        "longitude": -76.17593117321151,
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d150941735",
            "name": "Spanish Restaurant",
            "short_name": "Spanish",
            "plural_name": "Spanish Restaurants",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/food/spanish_",
              "suffix": ".png"
            }
          }
        ],
        "distance": 2891,
        "location": {
          "address": "52 E Main St",
          "locality": "Ephrata",
          "region": "PA",
          "postcode": "17522",
          "country": "US",
          "formatted_address": "52 E Main St, Ephrata, PA 17522"
        },
        "name": "La Promesa Restaurant"
      }
    },
    {
      "type": "place",
      "text": {
        "primary": "Caruso's Italian Restaurant",
        "secondary": "2036 Main St, Lititz, PA 17543",
        "highlight": [
          {
            "start": 17,
            "length": 7
          }
        ]
      },
      "link": "/places/4c0ebeafc6cf76b0663a8151",
      "place": {
        "fsq_place_id": "4c0ebeafc6cf76b0663a8151",
        "latitude": 40.15128289439884,
        "longitude": -76.25081732567571,
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d110941735",
            "name": "Italian Restaurant",
            "short_name": "Italian",
            "plural_name": "Italian Restaurants",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/food/italian_",
              "suffix": ".png"
            }
          }
        ],
        "distance": 4306,
        "location": {
          "address": "2036 Main St",
          "locality": "Lititz",
          "region": "PA",
          "postcode": "17543",
          "country": "US",
          "formatted_address": "2036 Main St, Lititz, PA 17543"
        },
        "name": "Caruso's Italian Restaurant"
      }
    },
    {
      "type": "place",
      "text": {
        "primary": "Ephrata House Restaurant",
        "secondary": "333 N State St, Ephrata, PA 17522",
        "highlight": [
          {
            "start": 14,
            "length": 7
          }
        ]
      },
      "link": "/places/61cc5b99e932425a5065e4eb",
      "place": {
        "fsq_place_id": "61cc5b99e932425a5065e4eb",
        "latitude": 40.181796783262314,
        "longitude": -76.17081356653841,
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d14e941735",
            "name": "American Restaurant",
            "short_name": "American",
            "plural_name": "American Restaurants",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/food/default_",
              "suffix": ".png"
            }
          }
        ],
        "distance": 3502,
        "location": {
          "address": "333 N State St",
          "locality": "Ephrata",
          "region": "PA",
          "postcode": "17522",
          "country": "US",
          "formatted_address": "333 N State St, Ephrata, PA 17522"
        },
        "name": "Ephrata House Restaurant"
      }
    },
    {
      "type": "place",
      "text": {
        "primary": "Gus's Keystone Family Restaurant",
        "secondary": "3687 Rothsville Rd, Ephrata, PA 17522",
        "highlight": [
          {
            "start": 22,
            "length": 7
          }
        ]
      },
      "link": "/places/4b65fea1f964a5202e0d2be3",
      "place": {
        "fsq_place_id": "4b65fea1f964a5202e0d2be3",
        "latitude": 40.16973583430921,
        "longitude": -76.19889280701054,
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d147941735",
            "name": "Diner",
            "short_name": "Diner",
            "plural_name": "Diners",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/food/diner_",
              "suffix": ".png"
            }
          }
        ],
        "distance": 1010,
        "location": {
          "address": "3687 Rothsville Rd",
          "locality": "Ephrata",
          "region": "PA",
          "postcode": "17522",
          "country": "US",
          "formatted_address": "3687 Rothsville Rd, Ephrata, PA 17522"
        },
        "name": "Gus's Keystone Family Restaurant"
      }
    },
    {
      "type": "place",
      "text": {
        "primary": "Sal's Pizza & Italian Restaurant",
        "secondary": "920 W Main St, Ephrata, PA 17522",
        "highlight": [
          {
            "start": 22,
            "length": 7
          }
        ]
      },
      "link": "/places/f579eba7d5de4c487c0fa76a",
      "place": {
        "fsq_place_id": "f579eba7d5de4c487c0fa76a",
        "latitude": 40.18754231658483,
        "longitude": -76.19051169127809,
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d110941735",
            "name": "Italian Restaurant",
            "short_name": "Italian",
            "plural_name": "Italian Restaurants",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/food/italian_",
              "suffix": ".png"
            }
          },
          {
            "fsq_category_id": "4bf58dd8d48988d1ca941735",
            "name": "Pizzeria",
            "short_name": "Pizza",
            "plural_name": "Pizzerias",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/food/pizza_",
              "suffix": ".png"
            }
          }
        ],
        "distance": 3109,
        "location": {
          "address": "920 W Main St",
          "locality": "Ephrata",
          "region": "PA",
          "postcode": "17522",
          "country": "US",
          "formatted_address": "920 W Main St, Ephrata, PA 17522"
        },
        "name": "Sal's Pizza & Italian Restaurant"
      }
    }
  ]
}
```


### Place Search API Endpoint Resource
`place-search.md`: Covers Place Search API endpoint.

**documentation-source**: https://docs.foursquare.com/fsq-developers-places/reference/place-search

**example-json-response**:
```
{
  "results": [
    {
      "fsq_place_id": "4c2f560b66e40f47b7acc18b",
      "latitude": 40.17089357381298,
      "longitude": -76.19755920282962,
      "categories": [
        {
          "fsq_category_id": "4bf58dd8d48988d143941735",
          "name": "Breakfast Spot",
          "short_name": "Breakfast",
          "plural_name": "Breakfast Spots",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/breakfast_",
            "suffix": ".png"
          }
        }
      ],
      "date_created": "2010-07-03",
      "date_refreshed": "2025-12-28",
      "distance": 1164,
      "email": "lillieflaps@comcast.net",
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710123022027"
      },
      "link": "/places/4c2f560b66e40f47b7acc18b",
      "location": {
        "address": "1032 S State St",
        "locality": "Ephrata",
        "region": "PA",
        "postcode": "17522",
        "country": "US",
        "formatted_address": "1032 S State St (at S Reading Rd), Ephrata, PA 17522"
      },
      "name": "Pancake Farm Restaurant",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/4c2f560b66e40f47b7acc18b",
      "related_places": {},
      "social_media": {
        "facebook_id": "223686027658951"
      },
      "tel": "(717) 733-7118",
      "website": "http://www.pancakefarmpa.com"
    },
    {
      "fsq_place_id": "5828bbd151856d5035bfd0bd",
      "latitude": 40.16913050947841,
      "longitude": -76.19853198472421,
      "categories": [
        {
          "fsq_category_id": "4bf58dd8d48988d1ca941735",
          "name": "Pizzeria",
          "short_name": "Pizza",
          "plural_name": "Pizzerias",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/pizza_",
            "suffix": ".png"
          }
        }
      ],
      "chains": [
        {
          "fsq_chain_id": "556e3827bd6a82902e253a7a",
          "name": "Little Caesars Pizza"
        }
      ],
      "date_created": "2016-11-13",
      "date_refreshed": "2025-12-17",
      "distance": 953,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710123022037"
      },
      "link": "/places/5828bbd151856d5035bfd0bd",
      "location": {
        "address": "1111 S State St",
        "locality": "Ephrata",
        "region": "PA",
        "postcode": "17522",
        "country": "US",
        "formatted_address": "1111 S State St, Ephrata, PA 17522"
      },
      "name": "Little Caesars Pizza",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/5828bbd151856d5035bfd0bd",
      "related_places": {},
      "social_media": {
        "twitter": "littlecaesars"
      },
      "store_id": "11620",
      "tel": "(717) 271-7377",
      "website": "https://littlecaesars.com/en-us/store/11620"
    },
    {
      "fsq_place_id": "55a4284a498ebf3dc68a36b7",
      "latitude": 40.169023571948124,
      "longitude": -76.19852278988289,
      "categories": [
        {
          "fsq_category_id": "4bf58dd8d48988d148941735",
          "name": "Donut Shop",
          "short_name": "Donuts",
          "plural_name": "Donut Shops",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/donuts_",
            "suffix": ".png"
          }
        },
        {
          "fsq_category_id": "4bf58dd8d48988d1e0931735",
          "name": "Coffee Shop",
          "short_name": "Coffee Shop",
          "plural_name": "Coffee Shops",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/coffeeshop_",
            "suffix": ".png"
          }
        }
      ],
      "chains": [
        {
          "fsq_chain_id": "556f676fbd6a75a99038d8f0",
          "name": "Dunkin'"
        }
      ],
      "date_created": "2015-07-13",
      "date_refreshed": "2026-02-09",
      "distance": 942,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710123022037"
      },
      "link": "/places/55a4284a498ebf3dc68a36b7",
      "location": {
        "address": "1111 S State St",
        "locality": "Ephrata",
        "region": "PA",
        "postcode": "17522",
        "country": "US",
        "formatted_address": "1111 S State St, Ephrata, PA 17522"
      },
      "name": "Dunkin'",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/55a4284a498ebf3dc68a36b7",
      "related_places": {},
      "social_media": {
        "facebook_id": "1367786516574163",
        "instagram": "dunkin",
        "twitter": "dunkindonuts"
      },
      "store_id": "351517",
      "tel": "(717) 466-2888",
      "website": "https://www.dunkindonuts.com"
    },
    {
      "fsq_place_id": "4b65fea1f964a5202e0d2be3",
      "latitude": 40.16973583430921,
      "longitude": -76.19889280701054,
      "categories": [
        {
          "fsq_category_id": "4bf58dd8d48988d147941735",
          "name": "Diner",
          "short_name": "Diner",
          "plural_name": "Diners",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/diner_",
            "suffix": ".png"
          }
        }
      ],
      "date_created": "2010-01-31",
      "date_refreshed": "2026-01-19",
      "distance": 1010,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710123022027"
      },
      "link": "/places/4b65fea1f964a5202e0d2be3",
      "location": {
        "address": "3687 Rothsville Rd",
        "locality": "Ephrata",
        "region": "PA",
        "postcode": "17522",
        "country": "US",
        "formatted_address": "3687 Rothsville Rd, Ephrata, PA 17522"
      },
      "name": "Gus's Keystone Family Restaurant",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/4b65fea1f964a5202e0d2be3",
      "related_places": {},
      "social_media": {
        "facebook_id": "402822329900695",
        "twitter": ""
      },
      "tel": "(717) 738-7381",
      "website": "http://www.guskeystone.com"
    },
    {
      "fsq_place_id": "4bb2a84feb3e95218c5fca0a",
      "latitude": 40.18798181531123,
      "longitude": -76.18501764563226,
      "categories": [
        {
          "fsq_category_id": "4bf58dd8d48988d1ca941735",
          "name": "Pizzeria",
          "short_name": "Pizza",
          "plural_name": "Pizzerias",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/pizza_",
            "suffix": ".png"
          }
        }
      ],
      "date_created": "2010-03-31",
      "date_refreshed": "2025-12-20",
      "distance": 3326,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710123011021"
      },
      "link": "/places/4bb2a84feb3e95218c5fca0a",
      "location": {
        "address": "128 N Reading Rd",
        "locality": "Ephrata",
        "region": "PA",
        "postcode": "17522",
        "country": "US",
        "formatted_address": "128 N Reading Rd (at Martin Ave), Ephrata, PA 17522"
      },
      "name": "Maria’s New York Pizzeria",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/4bb2a84feb3e95218c5fca0a",
      "related_places": {
        "parent": {
          "fsq_place_id": "55d9d865498e40a79148fa43",
          "categories": [
            {
              "fsq_category_id": "5744ccdfe4b0c0459246b4dc",
              "name": "Shopping Plaza",
              "short_name": "Shopping Plaza",
              "plural_name": "Shopping Plazas",
              "icon": {
                "prefix": "https://ss3.4sqi.net/img/categories_v2/shops/mall_",
                "suffix": ".png"
              }
            }
          ],
          "name": "Cloister Shopping Center"
        }
      },
      "social_media": {
        "facebook_id": "117451214940294",
        "twitter": ""
      },
      "tel": "(717) 721-6003",
      "website": "http://chrisnypizza.com"
    },
    {
      "fsq_place_id": "4ba017b1f964a520545937e3",
      "latitude": 40.169204218833315,
      "longitude": -76.20156539739315,
      "categories": [
        {
          "fsq_category_id": "4bf58dd8d48988d145941735",
          "name": "Chinese Restaurant",
          "short_name": "Chinese",
          "plural_name": "Chinese Restaurants",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/asian_",
            "suffix": ".png"
          }
        }
      ],
      "date_created": "2010-03-16",
      "date_refreshed": "2025-08-19",
      "distance": 922,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710124044009"
      },
      "link": "/places/4ba017b1f964a520545937e3",
      "location": {
        "address": "3583 Rothsville Rd",
        "locality": "Ephrata",
        "region": "PA",
        "postcode": "17522",
        "country": "US",
        "formatted_address": "3583 Rothsville Rd, Ephrata, PA 17522"
      },
      "name": "New Panda",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/4ba017b1f964a520545937e3",
      "related_places": {},
      "social_media": {},
      "tel": "(717) 721-1888",
      "website": "http://www.newpandaephrata.com"
    },
    {
      "fsq_place_id": "4b6eff59f964a520abd52ce3",
      "latitude": 40.1875599162429,
      "longitude": -76.18541673130227,
      "categories": [
        {
          "fsq_category_id": "4bf58dd8d48988d1c5941735",
          "name": "Sandwich Spot",
          "short_name": "Sandwich Spot",
          "plural_name": "Sandwich Spots",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/deli_",
            "suffix": ".png"
          }
        },
        {
          "fsq_category_id": "4bf58dd8d48988d14e941735",
          "name": "American Restaurant",
          "short_name": "American",
          "plural_name": "American Restaurants",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/default_",
            "suffix": ".png"
          }
        }
      ],
      "chains": [
        {
          "fsq_chain_id": "5568f4f1a7c8a9cf8ec4a140"
        }
      ],
      "date_created": "2010-02-07",
      "date_refreshed": "2025-08-15",
      "distance": 3269,
      "email": "ep@isaacdeli.com",
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710123011021"
      },
      "link": "/places/4b6eff59f964a520abd52ce3",
      "location": {
        "address": "120 N Reading Rd",
        "locality": "Ephrata",
        "region": "PA",
        "postcode": "17522",
        "country": "US",
        "formatted_address": "120 N Reading Rd, Ephrata, PA 17522"
      },
      "name": "Isaac's Famous Grilled Sandwiches",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/4b6eff59f964a520abd52ce3",
      "related_places": {
        "parent": {
          "fsq_place_id": "55d9d865498e40a79148fa43",
          "categories": [
            {
              "fsq_category_id": "5744ccdfe4b0c0459246b4dc",
              "name": "Shopping Plaza",
              "short_name": "Shopping Plaza",
              "plural_name": "Shopping Plazas",
              "icon": {
                "prefix": "https://ss3.4sqi.net/img/categories_v2/shops/mall_",
                "suffix": ".png"
              }
            }
          ],
          "name": "Cloister Shopping Center"
        }
      },
      "social_media": {
        "facebook_id": "54200277300",
        "twitter": "isaacsdeli"
      },
      "store_id": "",
      "tel": "(717) 733-7777",
      "website": "http://www.isaacsdeli.com"
    },
    {
      "fsq_place_id": "50379e09e4b00c877a2c06ad",
      "latitude": 40.15476438445247,
      "longitude": -76.1934927127495,
      "categories": [
        {
          "fsq_category_id": "4bf58dd8d48988d16a941735",
          "name": "Bakery",
          "short_name": "Bakery",
          "plural_name": "Bakeries",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/bakery_",
            "suffix": ".png"
          }
        }
      ],
      "date_created": "2012-08-24",
      "date_refreshed": "2025-10-16",
      "distance": 975,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710124023012"
      },
      "link": "/places/50379e09e4b00c877a2c06ad",
      "location": {
        "address": "1229 Diamond St",
        "locality": "Akron",
        "region": "PA",
        "postcode": "17501",
        "country": "US",
        "formatted_address": "1229 Diamond St, Akron, PA 17501"
      },
      "name": "Martin's Pretzel Bakery",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/50379e09e4b00c877a2c06ad",
      "related_places": {},
      "social_media": {
        "twitter": ""
      },
      "tel": "(717) 859-1272",
      "website": "http://www.martinspretzelspa.com"
    },
    {
      "fsq_place_id": "4bbcd279a8cf76b07600b1fd",
      "latitude": 40.169689220180054,
      "longitude": -76.19806109342088,
      "categories": [
        {
          "fsq_category_id": "4bf58dd8d48988d16e941735",
          "name": "Fast Food Restaurant",
          "short_name": "Fast Food",
          "plural_name": "Fast Food Restaurants",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/fastfood_",
            "suffix": ".png"
          }
        }
      ],
      "chains": [
        {
          "fsq_chain_id": "556f46f6bd6a007c77390fa9",
          "name": "Wendy's"
        }
      ],
      "date_created": "2010-04-07",
      "date_refreshed": "2025-12-17",
      "distance": 1024,
      "email": "digital@wendys.com",
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710123022027"
      },
      "link": "/places/4bbcd279a8cf76b07600b1fd",
      "location": {
        "address": "1075 S State St",
        "locality": "Ephrata",
        "region": "PA",
        "postcode": "17522",
        "country": "US",
        "formatted_address": "1075 S State St (at Rothsville Rd), Ephrata, PA 17522"
      },
      "name": "Wendy's",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/4bbcd279a8cf76b07600b1fd",
      "related_places": {},
      "social_media": {
        "facebook_id": "116395195115469",
        "twitter": "wendys"
      },
      "store_id": "825",
      "tel": "(717) 733-8450",
      "website": "https://locations.wendys.com/united-states/pa/ephrata/1075-s.-state-st"
    },
    {
      "fsq_place_id": "4e552087a8093d27ccae4b28",
      "latitude": 40.1791723912288,
      "longitude": -76.17763951044547,
      "categories": [
        {
          "fsq_category_id": "4bf58dd8d48988d1ca941735",
          "name": "Pizzeria",
          "short_name": "Pizza",
          "plural_name": "Pizzerias",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/food/pizza_",
            "suffix": ".png"
          }
        }
      ],
      "date_created": "2011-08-24",
      "date_refreshed": "2025-12-17",
      "distance": 2880,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710123012039"
      },
      "link": "/places/4e552087a8093d27ccae4b28",
      "location": {
        "address": "15 W Main St",
        "locality": "Ephrata",
        "region": "PA",
        "postcode": "17522",
        "country": "US",
        "formatted_address": "15 W Main St, Ephrata, PA 17522"
      },
      "name": "Roma Pizza",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/4e552087a8093d27ccae4b28",
      "related_places": {},
      "social_media": {
        "facebook_id": "111673678867867",
        "twitter": ""
      },
      "tel": "(717) 733-8786",
      "website": ""
    }
  ],
  "context": {
    "geo_bounds": {
      "circle": {
        "center": {
          "latitude": 40.1609,
          "longitude": -76.2017
        },
        "radius": 9429
      }
    }
  }
}
```

### Place Details API Endpoint Resource
`place-details.md`: Covers Place Details API endpoint.

**documentation-source**: https://docs.foursquare.com/fsq-developers-places/reference/place-details

**example-json-response**:

```
{
  "fsq_place_id": "5a187743ccad6b307315e6fe",
  "latitude": 40.742058823215544,
  "longitude": -73.9918041229248,
  "categories": [
    {
      "fsq_category_id": "4bf58dd8d48988d125941735",
      "name": "Tech Startup",
      "short_name": "Tech Startup",
      "plural_name": "Tech Startups",
      "icon": {
        "prefix": "https://ss3.4sqi.net/img/categories_v2/shops/technology_",
        "suffix": ".png"
      }
    }
  ],
  "chains": [
    {
      "fsq_chain_id": "556e5779bd6a82902e28bcea",
      "name": "Foursquare"
    }
  ],
  "date_closed": "2025-09-02",
  "date_created": "2017-11-24",
  "date_refreshed": "2025-09-02",
  "extended_location": {
    "dma": "New York",
    "census_block_id": "360610058003001"
  },
  "link": "/places/5a187743ccad6b307315e6fe",
  "location": {
    "address": "50 W 23rd St",
    "locality": "New York",
    "region": "NY",
    "postcode": "10010",
    "country": "US",
    "formatted_address": "50 W 23rd St (btwn 5th & 6th Ave), New York, NY 10010"
  },
  "name": "Foursquare HQ",
  "placemaker_url": "https://foursquare.com/placemakers/review-place/5a187743ccad6b307315e6fe",
  "related_places": {
    "parent": {
      "fsq_place_id": "5b30ed35c824ae002c74e557",
      "categories": [
        {
          "fsq_category_id": "63be6904847c3692a84b9b76",
          "name": "Office Building",
          "short_name": "Office Building",
          "plural_name": "Office Buildings",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/building/default_",
            "suffix": ".png"
          }
        }
      ],
      "name": "50 W 23rd Street"
    },
    "children": [
      {
        "fsq_place_id": "5d386be39bc0e7000859f0d8",
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d127941735",
            "name": "Meeting Room",
            "short_name": "Meeting Room",
            "plural_name": "Meeting Rooms",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/building/office_conferenceroom_",
              "suffix": ".png"
            }
          }
        ],
        "name": "Barbershop"
      },
      {
        "fsq_place_id": "5cb795d9c876c8002c3cbfad",
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d124941735",
            "name": "Office",
            "short_name": "Office",
            "plural_name": "Offices",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/building/default_",
              "suffix": ".png"
            }
          }
        ],
        "name": "Flora and Fauna"
      },
      {
        "fsq_place_id": "573f1a5b498e03d46895ea20",
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d124941735",
            "name": "Office",
            "short_name": "Office",
            "plural_name": "Offices",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/building/default_",
              "suffix": ".png"
            }
          }
        ],
        "name": "Victor's Desk"
      },
      {
        "fsq_place_id": "5c59cd3dd69ed0002c56e765",
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d100941735",
            "name": "Conference Room",
            "short_name": "Conference Room",
            "plural_name": "Conference Rooms",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/building/conventioncenter_meetingroom_",
              "suffix": ".png"
            }
          }
        ],
        "name": "The Garden"
      },
      {
        "fsq_place_id": "5b27c0773fcee8002c74236d",
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d11f941735",
            "name": "Night Club",
            "short_name": "Night Club",
            "plural_name": "Night Clubs",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/nightlife/nightclub_",
              "suffix": ".png"
            }
          }
        ],
        "name": "Intern VIP Room"
      },
      {
        "fsq_place_id": "5aecc187acb00b002ca0963e",
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d127941735",
            "name": "Meeting Room",
            "short_name": "Meeting Room",
            "plural_name": "Meeting Rooms",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/building/office_conferenceroom_",
              "suffix": ".png"
            }
          }
        ],
        "name": "8Th Floor Pod"
      },
      {
        "fsq_place_id": "5aecc187d3cce8002c2549d2",
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d127941735",
            "name": "Meeting Room",
            "short_name": "Meeting Room",
            "plural_name": "Meeting Rooms",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/building/office_conferenceroom_",
              "suffix": ".png"
            }
          }
        ],
        "name": "8Th Floor Pod"
      },
      {
        "fsq_place_id": "5b4cd2c41ffe97002c233587",
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d100941735",
            "name": "Conference Room",
            "short_name": "Conference Room",
            "plural_name": "Conference Rooms",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/building/conventioncenter_meetingroom_",
              "suffix": ".png"
            }
          }
        ],
        "name": "Foursquare HQ : The Boardroom"
      },
      {
        "fsq_place_id": "5bf5cd6aa30619002c8d6892",
        "categories": [
          {
            "fsq_category_id": "52f2ab2ebcbc57f1066b8b36",
            "name": "IT Service",
            "short_name": "IT Service",
            "plural_name": "IT Services",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/shops/technology_",
              "suffix": ".png"
            }
          }
        ],
        "name": "IT Helpdesk"
      },
      {
        "fsq_place_id": "5bff3f43c8b2fb002c559b45",
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d124941735",
            "name": "Office",
            "short_name": "Office",
            "plural_name": "Offices",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/building/default_",
              "suffix": ".png"
            }
          },
          {
            "fsq_category_id": "4bf58dd8d48988d126941735",
            "name": "Government Building",
            "short_name": "Government",
            "plural_name": "Government Buildings",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/building/government_",
              "suffix": ".png"
            }
          }
        ],
        "name": "Office Of The Executive Chairman"
      },
      {
        "fsq_place_id": "5d386be423f68a0008106a41",
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d127941735",
            "name": "Meeting Room",
            "short_name": "Meeting Room",
            "plural_name": "Meeting Rooms",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/building/office_conferenceroom_",
              "suffix": ".png"
            }
          }
        ],
        "name": "Barbershop"
      },
      {
        "fsq_place_id": "5c59cd3cd69ed0002c56e5f9",
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d100941735",
            "name": "Conference Room",
            "short_name": "Conference Room",
            "plural_name": "Conference Rooms",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/building/conventioncenter_meetingroom_",
              "suffix": ".png"
            }
          }
        ],
        "name": "The Garden"
      },
      {
        "fsq_place_id": "5b2839eb32b61d002c262fc5",
        "categories": [
          {
            "fsq_category_id": "4bf58dd8d48988d124941735",
            "name": "Office",
            "short_name": "Office",
            "plural_name": "Offices",
            "icon": {
              "prefix": "https://ss3.4sqi.net/img/categories_v2/building/default_",
              "suffix": ".png"
            }
          }
        ],
        "name": "Fsq PM Desk"
      }
    ]
  },
  "social_media": {
    "instagram": "foursquare",
    "twitter": "foursquare"
  },
  "tel": "(646) 738-8691",
  "website": "https://foursquare.com"
}
```



### Flag Place API Endpoint Resource
`place-flag.md`: Covers Flag Place API endpoint:

**documentation-source**: https://docs.foursquare.com/fsq-developers-places/reference/place-flag

**example-json-response**:
```
{
  "suggested_edits": [
    {
      "id": "6997502c0da0eb6118081628",
      "fsq_place_id": "4ca61465a6e08cfa9d087794",
      "suggested_edit_type": "fr",
      "created_at": "2026-02-19T18:02:20.224Z",
      "status": "pending"
    }
  ],
  "errors": []
}
```

### Find Geotagging Candidates API Endpoint Resource
`geotagging-candidates.md`: Covers Find Geotagging Candidates API endpoint.

**documentation-source**: https://docs.foursquare.com/fsq-developers-places/reference/geotagging-candidates

**example-json-response**:
```
{
  "candidates": [
    {
      "fsq_place_id": "4ca61465a6e08cfa9d087794",
      "latitude": 40.159009999999995,
      "longitude": -76.204295,
      "categories": [
        {
          "fsq_category_id": "4bf58dd8d48988d172941735",
          "name": "Post Office",
          "short_name": "Post Office",
          "plural_name": "Post Offices",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/building/postoffice_",
            "suffix": ".png"
          }
        }
      ],
      "chains": [
        {
          "fsq_chain_id": "556f676fbd6a75a99038d8e8",
          "name": "United States Postal Service"
        }
      ],
      "date_created": "2010-10-01",
      "date_refreshed": "2025-12-22",
      "distance": 304,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710124022002"
      },
      "link": "/places/4ca61465a6e08cfa9d087794",
      "location": {
        "address": "100 N 7th St",
        "locality": "Akron",
        "region": "PA",
        "postcode": "17501",
        "country": "US",
        "formatted_address": "100 N 7th St, Akron, PA 17501"
      },
      "name": "US Post Office",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/4ca61465a6e08cfa9d087794",
      "related_places": {},
      "social_media": {
        "facebook_id": "60774464809",
        "twitter": "usps"
      },
      "tel": "(800) 275-8777",
      "website": "https://tools.usps.com/find-location.htm?location=1352533"
    },
    {
      "fsq_place_id": "4e3e1f86d22d102e854c04cc",
      "latitude": 40.158874188027276,
      "longitude": -76.20322749361527,
      "categories": [
        {
          "fsq_category_id": "4bf58dd8d48988d132941735",
          "name": "Church",
          "short_name": "Church",
          "plural_name": "Churches",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/building/religious_church_",
            "suffix": ".png"
          }
        }
      ],
      "date_created": "2011-08-07",
      "date_refreshed": "2024-01-09",
      "distance": 259,
      "email": "akgec@dejazzd.com",
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710124023009"
      },
      "link": "/places/4e3e1f86d22d102e854c04cc",
      "location": {
        "address": "N 7TH St",
        "locality": "Akron",
        "region": "PA",
        "postcode": "17501",
        "country": "US",
        "formatted_address": "N 7TH St, Akron, PA 17501"
      },
      "name": "Grace EC Church",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/4e3e1f86d22d102e854c04cc",
      "related_places": {},
      "social_media": {
        "twitter": ""
      },
      "tel": "(717) 859-2700",
      "website": "http://www.akronpaecc.com"
    },
    {
      "fsq_place_id": "4edcbf9d0039e227b7f7dbef",
      "latitude": 40.155803616798444,
      "longitude": -76.20577327340715,
      "categories": [
        {
          "fsq_category_id": "4bf58dd8d48988d125941735",
          "name": "Tech Startup",
          "short_name": "Tech Startup",
          "plural_name": "Tech Startups",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/shops/technology_",
            "suffix": ".png"
          }
        }
      ],
      "date_created": "2011-12-05",
      "date_refreshed": "2016-07-12",
      "distance": 663,
      "email": "info@webtekcc.com",
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710124022015"
      },
      "link": "/places/4edcbf9d0039e227b7f7dbef",
      "location": {
        "address": "100 South 7th Street",
        "locality": "Akron",
        "region": "PA",
        "postcode": "17501",
        "country": "US",
        "formatted_address": "100 South 7th Street, Akron, PA 17501"
      },
      "name": "WebTek",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/4edcbf9d0039e227b7f7dbef",
      "related_places": {},
      "social_media": {
        "twitter": "webtekcc"
      },
      "store_id": "358815",
      "tel": "(717) 859-3250",
      "website": "http://www.webtekcc.com/#LNGvX8"
    },
    {
      "fsq_place_id": "60d16b169cf29b2dc4f2f2df",
      "latitude": 40.166215517547144,
      "longitude": -76.20580533655428,
      "categories": [
        {
          "fsq_category_id": "52f2ab2ebcbc57f1066b8b57",
          "name": "Employment Agency",
          "short_name": "Employment Agency",
          "plural_name": "Employment Agencies",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/building/default_",
            "suffix": ".png"
          }
        }
      ],
      "chains": [
        {
          "fsq_chain_id": "60d11c6c8ce4e137b8d265cf",
          "name": "US Army Recruiting"
        }
      ],
      "date_created": "2021-06-22",
      "date_refreshed": "2021-08-21",
      "distance": 685,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710124021004"
      },
      "link": "/places/60d16b169cf29b2dc4f2f2df",
      "location": {
        "address": "3370 Rothsville Rd",
        "locality": "Akron",
        "region": "PA",
        "postcode": "17501",
        "country": "US",
        "formatted_address": "3370 Rothsville Rd, Akron, PA 17501"
      },
      "name": "US Army Recruiting",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/60d16b169cf29b2dc4f2f2df",
      "related_places": {},
      "social_media": {
        "twitter": ""
      },
      "tel": "(717) 721-6145",
      "website": "https://www.goarmy.com"
    },
    {
      "fsq_place_id": "8e1d5229db4047cebfd2fa3f",
      "latitude": 40.16920822466354,
      "longitude": -76.20147925410016,
      "categories": [
        {
          "fsq_category_id": "63be6904847c3692a84b9b34",
          "name": "Chemicals and Gasses Manufacturer",
          "short_name": "Chemicals and Gasses Manufacturer",
          "plural_name": "Chemicals and Gasses Manufacturers",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/building/default_",
            "suffix": ".png"
          }
        }
      ],
      "chains": [
        {
          "fsq_chain_id": "556a2fdca7c8957d73d447d1",
          "name": "Blue Rhino"
        }
      ],
      "date_created": "2019-12-13",
      "date_refreshed": "2019-12-13",
      "distance": 923,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710124044009"
      },
      "link": "/places/8e1d5229db4047cebfd2fa3f",
      "location": {
        "address": "3585 Rothsville Rd",
        "locality": "Ephrata",
        "region": "PA",
        "postcode": "17522",
        "country": "US",
        "formatted_address": "3585 Rothsville Rd, Ephrata, PA 17522"
      },
      "name": "Blue Rhino",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/8e1d5229db4047cebfd2fa3f",
      "related_places": {},
      "social_media": {},
      "tel": "(717) 733-6435",
      "website": "https://bluerhino.com"
    },
    {
      "fsq_place_id": "d91cf8350ad448066e8f1eba",
      "latitude": 40.16920822466354,
      "longitude": -76.20147925410016,
      "categories": [
        {
          "fsq_category_id": "63be6904847c3692a84b9b3d",
          "name": "Financial Service",
          "short_name": "Financial Service",
          "plural_name": "Financial Services",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/shops/financial_",
            "suffix": ".png"
          }
        }
      ],
      "chains": [
        {
          "fsq_chain_id": "561fef25498e8b23a9c5b294",
          "name": "Western Union"
        }
      ],
      "date_created": "2021-10-23",
      "date_refreshed": "2026-02-13",
      "distance": 923,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710124044009"
      },
      "link": "/places/d91cf8350ad448066e8f1eba",
      "location": {
        "address": "3585 Rothsville Rd",
        "locality": "Ephrata",
        "region": "PA",
        "postcode": "17522",
        "country": "US",
        "formatted_address": "3585 Rothsville Rd, Ephrata, PA 17522"
      },
      "name": "Western Union",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/d91cf8350ad448066e8f1eba",
      "related_places": {},
      "social_media": {},
      "tel": "(717) 733-6435",
      "website": "https://www.westernunion.com"
    },
    {
      "fsq_place_id": "5c060c46f5e9d7002cc170b4",
      "latitude": 40.156404810214234,
      "longitude": -76.19977252713485,
      "categories": [
        {
          "fsq_category_id": "5032885091d4c4b30a586d66",
          "name": "Real Estate Agency",
          "short_name": "Real Estate",
          "plural_name": "Real Estate Agencies",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/shops/realestate_",
            "suffix": ".png"
          }
        }
      ],
      "date_created": "2018-12-04",
      "date_refreshed": "2025-08-22",
      "distance": 525,
      "email": "info@wolfkline.com",
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710124024000"
      },
      "link": "/places/5c060c46f5e9d7002cc170b4",
      "location": {
        "address": "1018 Main St",
        "locality": "Akron",
        "region": "PA",
        "postcode": "17501",
        "country": "US",
        "formatted_address": "1018 Main St, Akron, PA 17501"
      },
      "name": "Wolf & Kline Property Management, Inc.",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/5c060c46f5e9d7002cc170b4",
      "related_places": {},
      "social_media": {
        "twitter": ""
      },
      "tel": "(717) 859-2010",
      "website": "http://www.wolfkline.com"
    },
    {
      "fsq_place_id": "d0e73320fa714ba6162144d4",
      "latitude": 40.15557374547543,
      "longitude": -76.20609932878028,
      "categories": [
        {
          "fsq_category_id": "63be6904847c3692a84b9b34",
          "name": "Chemicals and Gasses Manufacturer",
          "short_name": "Chemicals and Gasses Manufacturer",
          "plural_name": "Chemicals and Gasses Manufacturers",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/building/default_",
            "suffix": ".png"
          }
        }
      ],
      "chains": [
        {
          "fsq_chain_id": "556a2fdca7c8957d73d447d1",
          "name": "Blue Rhino"
        }
      ],
      "date_created": "2019-12-13",
      "date_refreshed": "2019-12-13",
      "distance": 699,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710124022015"
      },
      "link": "/places/d0e73320fa714ba6162144d4",
      "location": {
        "address": "106 S 7th St",
        "locality": "Akron",
        "region": "PA",
        "postcode": "17501",
        "country": "US",
        "formatted_address": "106 S 7th St, Akron, PA 17501"
      },
      "name": "Blue Rhino",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/d0e73320fa714ba6162144d4",
      "related_places": {},
      "social_media": {},
      "tel": "(717) 859-3187",
      "website": "https://bluerhino.com"
    },
    {
      "fsq_place_id": "03885a0a5eee4b41edc48330",
      "latitude": 40.15557374547543,
      "longitude": -76.20609932878028,
      "categories": [
        {
          "fsq_category_id": "63be6904847c3692a84b9b3d",
          "name": "Financial Service",
          "short_name": "Financial Service",
          "plural_name": "Financial Services",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/shops/financial_",
            "suffix": ".png"
          }
        }
      ],
      "chains": [
        {
          "fsq_chain_id": "561fef25498e8b23a9c5b294",
          "name": "Western Union"
        }
      ],
      "date_created": "2019-08-10",
      "date_refreshed": "2026-02-13",
      "distance": 699,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710124022015"
      },
      "link": "/places/03885a0a5eee4b41edc48330",
      "location": {
        "address": "106 S 7th St",
        "locality": "Akron",
        "region": "PA",
        "postcode": "17501",
        "country": "US",
        "formatted_address": "106 S 7th St, Akron, PA 17501"
      },
      "name": "Western Union",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/03885a0a5eee4b41edc48330",
      "related_places": {},
      "social_media": {},
      "tel": "(717) 859-3187",
      "website": "https://www.westernunion.com"
    },
    {
      "fsq_place_id": "32aa22e373d24b270b1562b7",
      "latitude": 40.167969,
      "longitude": -76.200421,
      "categories": [
        {
          "fsq_category_id": "63be6904847c3692a84b9b3d",
          "name": "Financial Service",
          "short_name": "Financial Service",
          "plural_name": "Financial Services",
          "icon": {
            "prefix": "https://ss3.4sqi.net/img/categories_v2/shops/financial_",
            "suffix": ".png"
          }
        }
      ],
      "chains": [
        {
          "fsq_chain_id": "561fef25498e8b23a9c5b294",
          "name": "Western Union"
        }
      ],
      "date_created": "2015-07-14",
      "date_refreshed": "2015-07-14",
      "distance": 793,
      "extended_location": {
        "dma": "Harrisburg-Lancaster-Lebanon-York",
        "census_block_id": "420710124044007"
      },
      "link": "/places/32aa22e373d24b270b1562b7",
      "location": {
        "address": "1127 S State St",
        "locality": "Ephrata",
        "region": "PA",
        "postcode": "17522",
        "country": "US",
        "formatted_address": "1127 S State St, Ephrata, PA 17522"
      },
      "name": "Western Union",
      "placemaker_url": "https://foursquare.com/placemakers/review-place/32aa22e373d24b270b1562b7",
      "related_places": {},
      "social_media": {},
      "store_id": "9330414e1cea74cbe8ecdf70969fddb3",
      "tel": "(717) 738-4221",
      "website": "http://www.westernunion.com"
    }
  ]
}
```

## Output: Create the directory structure and all markdown files listed above.

