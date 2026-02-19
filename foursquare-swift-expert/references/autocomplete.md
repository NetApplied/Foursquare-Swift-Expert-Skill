# Autocomplete

Documentation source: https://docs.foursquare.com/fsq-developers-places/reference/autocomplete

## Endpoint

- Method: `GET`
- Path: `/autocomplete`
- Operation ID: `autocomplete`

## Path parameters

None.

## Query parameters

| Name | Type | Required | Description | Notes |
| --- | --- | --- | --- | --- |
| query | string | Yes | A search term to be applied against titles. Must be at least 3 characters long. |  |
| ll | string | No | The latitude/longitude around which you wish to retrieve place information. Specified as latitude,longitude (e.g., ll=41.8781,-87.6298). If you do not specify ll, the server will attempt to retrieve the IP address from the request, and geolocate that IP address. |  |
| radius | integer | No | Defines the distance (in meters) within which to return place results. Setting a radius biases the results to the indicated area, but may not fully restrict results to that specified area. If not provided, default radius is set to 5000 meters. | Format: int32 |
| types | string | No | The types of results to return; any combination of place, search, and/or geo.If no types are specified, all types will be returned. |  |
| bias | string | No | Bias the autocomplete results by a specific type; one of place, search, or geo. |  |
| session_token | string | No | A user-generated token to to group the user's query and the user's selected result into a discrete session for billing purposes. Learn more about [session tokens](https://docs.foursquare.com/reference/session-tokens). *If the session_token parameter is omitted, the session is charged per keystroke/request.* |  |
| limit | integer | No | The number of results to return, up to 50. Defaults to 10. | Min: 1; Max: 50; Format: int32 |

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
          "results": {
            "type": "array",
            "properties": {
              "traversable_again": {
                "type": "boolean"
              }
            },
            "items": {
              "type": "object",
              "properties": {
                "type": {
                  "type": "string"
                },
                "text": {
                  "type": "object",
                  "properties": {
                    "primary": {
                      "type": "string"
                    },
                    "secondary": {
                      "type": "string"
                    },
                    "highlight": {
                      "type": "array",
                      "properties": {
                        "traversable_again": {
                          "type": "boolean"
                        }
                      },
                      "items": {
                        "type": "object",
                        "properties": {
                          "start": {
                            "type": "integer",
                            "format": "int32"
                          },
                          "length": {
                            "type": "integer",
                            "format": "int32"
                          }
                        }
                      }
                    }
                  }
                },
                "icon": {
                  "type": "object",
                  "properties": {
                    "fsq_photo_id": {
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
                        "fsq_tip_id": {
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
                        "photo": {
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
                        },
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
                },
                "link": {
                  "type": "string"
                },
                "place": {
                  "type": "object",
                  "properties": {
                    "fsq_place_id": {
                      "type": "string"
                    },
                    "latitude": {
                      "type": "object"
                    },
                    "longitude": {
                      "type": "object"
                    },
                    "categories": {
                      "type": "array",
                      "properties": {
                        "traversable_again": {
                          "type": "boolean"
                        }
                      },
                      "items": {
                        "type": "object",
                        "properties": {
                          "fsq_category_id": {
                            "type": "string"
                          },
                          "name": {
                            "type": "string"
                          },
                          "short_name": {
                            "type": "string"
                          },
                          "plural_name": {
                            "type": "string"
                          },
                          "icon": {
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
                    },
                    "chains": {
                      "type": "array",
                      "properties": {
                        "traversable_again": {
                          "type": "boolean"
                        }
                      },
                      "items": {
                        "type": "object",
                        "properties": {
                          "fsq_chain_id": {
                            "type": "string"
                          },
                          "name": {
                            "type": "string"
                          },
                          "logo": {
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
                          },
                          "parent_id": {
                            "type": "string"
                          }
                        }
                      }
                    },
                    "date_closed": {
                      "type": "string",
                      "format": "date"
                    },
                    "date_created": {
                      "type": "string"
                    },
                    "date_refreshed": {
                      "type": "string"
                    },
                    "description": {
                      "type": "string"
                    },
                    "distance": {
                      "type": "integer",
                      "format": "int32"
                    },
                    "email": {
                      "type": "string"
                    },
                    "extended_location": {
                      "type": "object",
                      "properties": {
                        "dma": {
                          "type": "string"
                        },
                        "census_block_id": {
                          "type": "string"
                        }
                      }
                    },
                    "attributes": {
                      "type": "object",
                      "properties": {
                        "restroom": {
                          "type": "object"
                        },
                        "outdoor_seating": {
                          "type": "object"
                        },
                        "atm": {
                          "type": "object"
                        },
                        "has_parking": {
                          "type": "object"
                        },
                        "wifi": {
                          "type": "string"
                        },
                        "delivery": {
                          "type": "object"
                        },
                        "reservations": {
                          "type": "object"
                        },
                        "takes_credit_card": {
                          "type": "object"
                        }
                      }
                    },
                    "hours": {
                      "type": "object",
                      "properties": {
                        "display": {
                          "type": "string"
                        },
                        "is_local_holiday": {
                          "type": "boolean"
                        },
                        "open_now": {
                          "type": "boolean"
                        },
                        "regular": {
                          "type": "array",
                          "properties": {
                            "traversable_again": {
                              "type": "boolean"
                            }
                          },
                          "items": {
                            "type": "object",
                            "properties": {
                              "close": {
                                "type": "string"
                              },
                              "day": {
                                "type": "integer",
                                "format": "int32"
                              },
                              "open": {
                                "type": "string"
                              }
                            }
                          }
                        }
                      }
                    },
                    "hours_popular": {
                      "type": "array",
                      "properties": {
                        "traversable_again": {
                          "type": "boolean"
                        }
                      },
                      "items": {
                        "type": "object",
                        "properties": {
                          "close": {
                            "type": "string"
                          },
                          "day": {
                            "type": "integer",
                            "format": "int32"
                          },
                          "open": {
                            "type": "string"
                          }
                        }
                      }
                    },
                    "link": {
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
                    "menu": {
                      "type": "string"
                    },
                    "name": {
                      "type": "string"
                    },
                    "photos": {
                      "type": "array",
                      "properties": {
                        "traversable_again": {
                          "type": "boolean"
                        }
                      },
                      "items": {
                        "type": "object",
                        "properties": {
                          "fsq_photo_id": {
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
                              "fsq_tip_id": {
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
                              "photo": {
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
                              },
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
                    },
                    "place_actions": {
                      "type": "array",
                      "properties": {
                        "traversable_again": {
                          "type": "boolean"
                        }
                      },
                      "items": {
                        "type": "object",
                        "properties": {
                          "action": {
                            "type": "string"
                          },
                          "url": {
                            "type": "string"
                          },
                          "provider_id": {
                            "type": "string"
                          }
                        }
                      }
                    },
                    "popularity": {
                      "type": "number",
                      "format": "double"
                    },
                    "placemaker_url": {
                      "type": "string"
                    },
                    "price": {
                      "type": "integer",
                      "format": "int32"
                    },
                    "rating": {
                      "type": "number",
                      "format": "double"
                    },
                    "related_places": {
                      "type": "object",
                      "properties": {
                        "parent": {
                          "type": "object",
                          "properties": {
                            "fsq_place_id": {
                              "type": "string"
                            },
                            "latitude": {
                              "type": "object"
                            },
                            "longitude": {
                              "type": "object"
                            },
                            "categories": {
                              "type": "array",
                              "properties": {
                                "traversable_again": {
                                  "type": "boolean"
                                }
                              },
                              "items": {
                                "type": "object",
                                "properties": {
                                  "fsq_category_id": {
                                    "type": "string"
                                  },
                                  "name": {
                                    "type": "string"
                                  },
                                  "short_name": {
                                    "type": "string"
                                  },
                                  "plural_name": {
                                    "type": "string"
                                  },
                                  "icon": {
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
                            },
                            "chains": {
                              "type": "array",
                              "properties": {
                                "traversable_again": {
                                  "type": "boolean"
                                }
                              },
                              "items": {
                                "type": "object",
                                "properties": {
                                  "fsq_chain_id": {
                                    "type": "string"
                                  },
                                  "name": {
                                    "type": "string"
                                  },
                                  "logo": {
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
                                  },
                                  "parent_id": {
                                    "type": "string"
                                  }
                                }
                              }
                            },
                            "date_closed": {
                              "type": "string",
                              "format": "date"
                            },
                            "date_created": {
                              "type": "string"
                            },
                            "date_refreshed": {
                              "type": "string"
                            },
                            "description": {
                              "type": "string"
                            },
                            "distance": {
                              "type": "integer",
                              "format": "int32"
                            },
                            "email": {
                              "type": "string"
                            },
                            "extended_location": {
                              "type": "object",
                              "properties": {
                                "dma": {
                                  "type": "string"
                                },
                                "census_block_id": {
                                  "type": "string"
                                }
                              }
                            },
                            "attributes": {
                              "type": "object",
                              "properties": {
                                "restroom": {
                                  "type": "object"
                                },
                                "outdoor_seating": {
                                  "type": "object"
                                },
                                "atm": {
                                  "type": "object"
                                },
                                "has_parking": {
                                  "type": "object"
                                },
                                "wifi": {
                                  "type": "string"
                                },
                                "delivery": {
                                  "type": "object"
                                },
                                "reservations": {
                                  "type": "object"
                                },
                                "takes_credit_card": {
                                  "type": "object"
                                }
                              }
                            },
                            "hours": {
                              "type": "object",
                              "properties": {
                                "display": {
                                  "type": "string"
                                },
                                "is_local_holiday": {
                                  "type": "boolean"
                                },
                                "open_now": {
                                  "type": "boolean"
                                },
                                "regular": {
                                  "type": "array",
                                  "properties": {
                                    "traversable_again": {
                                      "type": "boolean"
                                    }
                                  },
                                  "items": {
                                    "type": "object",
                                    "properties": {
                                      "close": {
                                        "type": "string"
                                      },
                                      "day": {
                                        "type": "integer",
                                        "format": "int32"
                                      },
                                      "open": {
                                        "type": "string"
                                      }
                                    }
                                  }
                                }
                              }
                            },
                            "hours_popular": {
                              "type": "array",
                              "properties": {
                                "traversable_again": {
                                  "type": "boolean"
                                }
                              },
                              "items": {
                                "type": "object",
                                "properties": {
                                  "close": {
                                    "type": "string"
                                  },
                                  "day": {
                                    "type": "integer",
                                    "format": "int32"
                                  },
                                  "open": {
                                    "type": "string"
                                  }
                                }
                              }
                            },
                            "link": {
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
                            "menu": {
                              "type": "string"
                            },
                            "name": {
                              "type": "string"
                            },
                            "photos": {
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
                            },
                            "place_actions": {
                              "type": "array",
                              "properties": {
                                "traversable_again": {
                                  "type": "boolean"
                                }
                              },
                              "items": {
                                "type": "object",
                                "properties": {
                                  "action": {
                                    "type": "string"
                                  },
                                  "url": {
                                    "type": "string"
                                  },
                                  "provider_id": {
                                    "type": "string"
                                  }
                                }
                              }
                            },
                            "popularity": {
                              "type": "number",
                              "format": "double"
                            },
                            "placemaker_url": {
                              "type": "string"
                            },
                            "price": {
                              "type": "integer",
                              "format": "int32"
                            },
                            "rating": {
                              "type": "number",
                              "format": "double"
                            },
                            "related_places": {},
                            "social_media": {
                              "type": "object",
                              "properties": {
                                "facebook_id": {
                                  "type": "string"
                                },
                                "instagram": {
                                  "type": "string"
                                },
                                "twitter": {
                                  "type": "string"
                                }
                              }
                            },
                            "stats": {
                              "type": "object",
                              "properties": {
                                "total_photos": {
                                  "type": "integer",
                                  "format": "int32"
                                },
                                "total_ratings": {
                                  "type": "integer",
                                  "format": "int64"
                                },
                                "total_tips": {
                                  "type": "integer",
                                  "format": "int32"
                                }
                              }
                            },
                            "store_id": {
                              "type": "string"
                            },
                            "tastes": {
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
                            "tel": {
                              "type": "string"
                            },
                            "tips": {
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
                            },
                            "verified": {
                              "type": "boolean"
                            },
                            "website": {
                              "type": "string"
                            }
                          }
                        },
                        "children": {
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
                              "latitude": {
                                "type": "object"
                              },
                              "longitude": {
                                "type": "object"
                              },
                              "categories": {
                                "type": "array",
                                "properties": {
                                  "traversable_again": {
                                    "type": "boolean"
                                  }
                                },
                                "items": {
                                  "type": "object",
                                  "properties": {
                                    "fsq_category_id": {
                                      "type": "string"
                                    },
                                    "name": {
                                      "type": "string"
                                    },
                                    "short_name": {
                                      "type": "string"
                                    },
                                    "plural_name": {
                                      "type": "string"
                                    },
                                    "icon": {
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
                              },
                              "chains": {
                                "type": "array",
                                "properties": {
                                  "traversable_again": {
                                    "type": "boolean"
                                  }
                                },
                                "items": {
                                  "type": "object",
                                  "properties": {
                                    "fsq_chain_id": {
                                      "type": "string"
                                    },
                                    "name": {
                                      "type": "string"
                                    },
                                    "logo": {
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
                                    },
                                    "parent_id": {
                                      "type": "string"
                                    }
                                  }
                                }
                              },
                              "date_closed": {
                                "type": "string",
                                "format": "date"
                              },
                              "date_created": {
                                "type": "string"
                              },
                              "date_refreshed": {
                                "type": "string"
                              },
                              "description": {
                                "type": "string"
                              },
                              "distance": {
                                "type": "integer",
                                "format": "int32"
                              },
                              "email": {
                                "type": "string"
                              },
                              "extended_location": {
                                "type": "object",
                                "properties": {
                                  "dma": {
                                    "type": "string"
                                  },
                                  "census_block_id": {
                                    "type": "string"
                                  }
                                }
                              },
                              "attributes": {
                                "type": "object",
                                "properties": {
                                  "restroom": {
                                    "type": "object"
                                  },
                                  "outdoor_seating": {
                                    "type": "object"
                                  },
                                  "atm": {
                                    "type": "object"
                                  },
                                  "has_parking": {
                                    "type": "object"
                                  },
                                  "wifi": {
                                    "type": "string"
                                  },
                                  "delivery": {
                                    "type": "object"
                                  },
                                  "reservations": {
                                    "type": "object"
                                  },
                                  "takes_credit_card": {
                                    "type": "object"
                                  }
                                }
                              },
                              "hours": {
                                "type": "object",
                                "properties": {
                                  "display": {
                                    "type": "string"
                                  },
                                  "is_local_holiday": {
                                    "type": "boolean"
                                  },
                                  "open_now": {
                                    "type": "boolean"
                                  },
                                  "regular": {
                                    "type": "array",
                                    "properties": {
                                      "traversable_again": {
                                        "type": "boolean"
                                      }
                                    },
                                    "items": {
                                      "type": "object",
                                      "properties": {
                                        "close": {
                                          "type": "string"
                                        },
                                        "day": {
                                          "type": "integer",
                                          "format": "int32"
                                        },
                                        "open": {
                                          "type": "string"
                                        }
                                      }
                                    }
                                  }
                                }
                              },
                              "hours_popular": {
                                "type": "array",
                                "properties": {
                                  "traversable_again": {
                                    "type": "boolean"
                                  }
                                },
                                "items": {
                                  "type": "object",
                                  "properties": {
                                    "close": {
                                      "type": "string"
                                    },
                                    "day": {
                                      "type": "integer",
                                      "format": "int32"
                                    },
                                    "open": {
                                      "type": "string"
                                    }
                                  }
                                }
                              },
                              "link": {
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
                              "menu": {
                                "type": "string"
                              },
                              "name": {
                                "type": "string"
                              },
                              "photos": {
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
                              },
                              "place_actions": {
                                "type": "array",
                                "properties": {
                                  "traversable_again": {
                                    "type": "boolean"
                                  }
                                },
                                "items": {
                                  "type": "object",
                                  "properties": {
                                    "action": {
                                      "type": "string"
                                    },
                                    "url": {
                                      "type": "string"
                                    },
                                    "provider_id": {
                                      "type": "string"
                                    }
                                  }
                                }
                              },
                              "popularity": {
                                "type": "number",
                                "format": "double"
                              },
                              "placemaker_url": {
                                "type": "string"
                              },
                              "price": {
                                "type": "integer",
                                "format": "int32"
                              },
                              "rating": {
                                "type": "number",
                                "format": "double"
                              },
                              "related_places": {},
                              "social_media": {
                                "type": "object",
                                "properties": {
                                  "facebook_id": {
                                    "type": "string"
                                  },
                                  "instagram": {
                                    "type": "string"
                                  },
                                  "twitter": {
                                    "type": "string"
                                  }
                                }
                              },
                              "stats": {
                                "type": "object",
                                "properties": {
                                  "total_photos": {
                                    "type": "integer",
                                    "format": "int32"
                                  },
                                  "total_ratings": {
                                    "type": "integer",
                                    "format": "int64"
                                  },
                                  "total_tips": {
                                    "type": "integer",
                                    "format": "int32"
                                  }
                                }
                              },
                              "store_id": {
                                "type": "string"
                              },
                              "tastes": {
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
                              "tel": {
                                "type": "string"
                              },
                              "tips": {
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
                              },
                              "verified": {
                                "type": "boolean"
                              },
                              "website": {
                                "type": "string"
                              }
                            }
                          }
                        }
                      }
                    },
                    "social_media": {
                      "type": "object",
                      "properties": {
                        "facebook_id": {
                          "type": "string"
                        },
                        "instagram": {
                          "type": "string"
                        },
                        "twitter": {
                          "type": "string"
                        }
                      }
                    },
                    "stats": {
                      "type": "object",
                      "properties": {
                        "total_photos": {
                          "type": "integer",
                          "format": "int32"
                        },
                        "total_ratings": {
                          "type": "integer",
                          "format": "int64"
                        },
                        "total_tips": {
                          "type": "integer",
                          "format": "int32"
                        }
                      }
                    },
                    "store_id": {
                      "type": "string"
                    },
                    "tastes": {
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
                    "tel": {
                      "type": "string"
                    },
                    "tips": {
                      "type": "array",
                      "properties": {
                        "traversable_again": {
                          "type": "boolean"
                        }
                      },
                      "items": {
                        "type": "object",
                        "properties": {
                          "fsq_tip_id": {
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
                          "photo": {
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
                          },
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
                    },
                    "verified": {
                      "type": "boolean"
                    },
                    "unresolved_flags": {
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
                    "veracity_rating": {
                      "type": "object"
                    },
                    "website": {
                      "type": "string"
                    },
                    "plugins": {
                      "type": "object",
                      "properties": {
                        "traversable_again": {
                          "type": "boolean"
                        }
                      },
                      "additionalProperties": {
                        "type": "object"
                      }
                    }
                  }
                },
                "search": {
                  "type": "object",
                  "properties": {
                    "query": {
                      "type": "string"
                    },
                    "category": {
                      "type": "object",
                      "properties": {
                        "fsq_category_id": {
                          "type": "string"
                        },
                        "name": {
                          "type": "string"
                        },
                        "short_name": {
                          "type": "string"
                        },
                        "plural_name": {
                          "type": "string"
                        },
                        "icon": {
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
                    },
                    "chain": {
                      "type": "object",
                      "properties": {
                        "fsq_chain_id": {
                          "type": "string"
                        },
                        "name": {
                          "type": "string"
                        },
                        "logo": {
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
                        },
                        "parent_id": {
                          "type": "string"
                        }
                      }
                    }
                  }
                },
                "geo": {
                  "type": "object",
                  "properties": {
                    "name": {
                      "type": "string"
                    },
                    "center": {
                      "type": "object",
                      "properties": {
                        "latitude": {
                          "type": "number",
                          "format": "double"
                        },
                        "longitude": {
                          "type": "number",
                          "format": "double"
                        }
                      }
                    },
                    "bounds": {
                      "type": "object",
                      "properties": {
                        "ne": {
                          "type": "object",
                          "properties": {
                            "latitude": {
                              "type": "number",
                              "format": "double"
                            },
                            "longitude": {
                              "type": "number",
                              "format": "double"
                            }
                          }
                        },
                        "sw": {
                          "type": "object",
                          "properties": {
                            "latitude": {
                              "type": "number",
                              "format": "double"
                            },
                            "longitude": {
                              "type": "number",
                              "format": "double"
                            }
                          }
                        }
                      }
                    },
                    "cc": {
                      "type": "string"
                    },
                    "type": {
                      "type": "string"
                    }
                  }
                },
                "debug": {
                  "type": "object",
                  "properties": {
                    "score": {
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

struct AutocompleteRequest: Codable, Identifiable, Hashable {
    let query: String
    let ll: String?
    let radius: Int?
    let types: String?
    let bias: String?
    let sessionToken: String?
    let limit: Int?

    var id: String { "autocomplete-request" }

    enum CodingKeys: String, CodingKey {
        case query = "query"
        case ll = "ll"
        case radius = "radius"
        case types = "types"
        case bias = "bias"
        case sessionToken = "session_token"
        case limit = "limit"
    }
}

struct AutocompleteResponse: Codable, Identifiable, Hashable {
    let results: [JSONValue]?

    var id: String { "autocomplete-response" }

    enum CodingKeys: String, CodingKey {
        case results = "results"
    }
}

final class AutocompleteClient {
    private let apiKey: String
    private let apiVersion: String
    private let baseURL = URL(string: "https://places-api.foursquare.com")!
    private let session: URLSession

    init(apiKey: String, apiVersion: String = "2025-06-17", session: URLSession = .shared) {
        self.apiKey = apiKey
        self.apiVersion = apiVersion
        self.session = session
    }

    func autocomplete(parameters: AutocompleteRequest) async throws -> AutocompleteResponse {
        let endpointPath = "/autocomplete"
        guard var components = URLComponents(url: baseURL.appendingPathComponent(endpointPath), resolvingAgainstBaseURL: false) else {
            throw FoursquareAPIError.invalidURL
        }

        var queryItems: [URLQueryItem] = []
        queryItems.append(URLQueryItem(name: "query", value: query))
        if let ll { queryItems.append(URLQueryItem(name: "ll", value: ll)) }
        if let radius { queryItems.append(URLQueryItem(name: "radius", value: String(radius))) }
        if let types { queryItems.append(URLQueryItem(name: "types", value: types)) }
        if let bias { queryItems.append(URLQueryItem(name: "bias", value: bias)) }
        if let sessionToken { queryItems.append(URLQueryItem(name: "session_token", value: sessionToken)) }
        if let limit { queryItems.append(URLQueryItem(name: "limit", value: String(limit))) }
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
            return try JSONDecoder().decode(AutocompleteResponse.self, from: data)
        } catch {
            throw FoursquareAPIError.decodingFailure(error)
        }
    }
}
```
