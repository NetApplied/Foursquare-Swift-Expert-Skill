# Places Response Fields, Categories, and Chains

Documentation source: https://docs.foursquare.com/fsq-developers-places/reference/response-fields

## Scope

This page is reference data (not a callable endpoint). Path and query parameters are not applicable.

## Categories schema

```json
{
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
  },
  "x-readme-ref-name": "VersionedCategoryV001"
}
```

## Chains schema

```json
{
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
  },
  "x-readme-ref-name": "VersionedVenueChainV001"
}
```

## Place Search response schema (common container)

```json
{
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
        },
        "x-readme-ref-name": "VenueV002"
      }
    },
    "context": {
      "type": "object",
      "properties": {
        "geo_bounds": {
          "type": "object",
          "properties": {
            "circle": {
              "type": "object",
              "properties": {
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
                "radius": {
                  "type": "integer",
                  "format": "int32"
                }
              }
            }
          }
        }
      },
      "x-readme-ref-name": "Context"
    }
  },
  "x-readme-ref-name": "PlaceSearchResponseV002"
}
```

## Places Pro response fields

| Field | Description |
| --- | --- |
| `fsq_place_id` | A unique identifier for a FSQ Place (formerly known as Venue ID). |
| `name` | The best known name for the FSQ Place. |
| `categories` | An array, possibly empty, of categories that describe the FSQ Place. Included subfields: `id` (Category ID), `name` (Category Label), and `icon` (Category's Icon). [View the list of FSQ Categories](/places/docs/categories) → [Places Photos Guide](https://docs.foursquare.com/developer/reference/places-photos-guide#assembling-a-photo-url) |
| `location` | An object containing the following fields, if available: - `address` - `locality` - `region` - `postcode` - `country` - `admin_region` - `post_town` - `po_box` |
| `latitude` | The latitude of the location's main entrance. |
| `longitude` | The longitude of the location's main entrance. |
| `distance` | The calculated distance (in meters) from the provided location (i.e. ll + radius OR near OR ne + sw) in the API call. This field will only be returned by the Place Search endpoint. |
| `tel` | The best known telephone number, with local formatting. |
| `email` | The primary contact email address for the FSQ Place. |
| `website` | The official website for the FSQ Place. |
| `social_media` | An object containing a FSQ Place's social media identifiers. Included subfields: `facebook_id`, `instagram`, and `twitter`. Not all FSQ Places will have all subfields. |
| `link` | The URL associated with the FSQ Place Detail API call. |
| `date_closed` | The recorded date when the FSQ Place was marked as permanently closed in Foursquare's databases. This does not necessarily indicate the Place was actually closed on this date. |
| `placemaker_url` | The Foursquare Placemaker URL associated with the FSQ Place |
| `chains` | An array, possibly empty, of chains that the FSQ Place belongs to. Included subfields: `id` (Chain ID) and `name` (Chain Name). [View the list of FSQ Chains](/places/docs/chains) → |
| `store_id` | A unique ID assigned to a venue to differentiate it from other stores in the same chain. |
| `related_places` | An object containing the information for Places related to the FSQ Place. Included subfields: `parent` and `children`. Not all FSQ Places will have parent or children relationships. Some Children Place responses may not show a `fsq_id`. This indicates that Foursquare does not have enough data inputs to mark the Child Place as an official Place. Example: `parent` = Los Angeles International Airport `children` = Terminal 1, Terminal 2, Terminal 3... |
| `extended_location` | An object containing additional location metadata, including `census_block` and `dma`. |
| `unresolved_flags` | An array containing a set of quality issue flags reported by Placemakers, human or agent, that require further corroboration. Each flag marks a suspected problem that has not yet been resolved. The values can be one or more of the following: - `closed` – the place is believed to be permanently closed - `duplicate` – the place is a duplicate of another record - `delete` – the place should be removed entirely - `privatevenue` – the place is private and should not be publicly listed - `inappropriate` – the content violates policy or is otherwise unsuitable - `doesnt_exist` – the place is thought not to exist at the specified location |

## Places Premium response fields

| Field | Description |
| --- | --- |
| `attributes` | A list of boolean tags that help to describe services and additional metadata offered by the Place (e.g. takes_reservations: true) For a full list of tags, see [Places Tags](https://docs.foursquare.com/data-products/docs/flat-file-data-schema#tags). |
| `description` | A general description of the FSQ Place. Typically provided by the owner/claimant of the FSQ Place and/or updated by City Guide Superusers. |
| `hours` | An array containing the `regular` hours of operation, `is_local_holiday`, `open_now`, and a formatted `display` string. `regular` hours contain subfields: `day` (`1` = Monday, `2` = Tuesday, `7` = Sunday) and `open`/`close` (24-hour time). |
| `hours_popular` | An array containing the hours during the week when a FSQ Place is most often visited. This place must have a minimum number of check-ins to be considered for the calculation. Similar to `hours`, this field contains subfields: `day` (`1` = Monday, `2` = Tuesday, `7` = Sunday) and `open`/`close` (24-hour time). |
| `menu` | A menu URL for the FSQ Place. |
| `photos` | An array of objects containing the following subfields: - `id` - `created_at` - `classifications` - `prefix` - `suffix` - `width` - `height`.For more information, see our [Places Photos Guide](https://docs.foursquare.com/developer/reference/places-photos-guide#assembling-a-photo-url) → |
| `place_actions` | A list of actionable links associated with the place, such as reservations or delivery ordering. Each action includes the action type, a URL for completing the action, and the provider source ID. |
| `popularity` | A measure of the FSQ Place's popularity, by foot traffic. This score is on a 0 to 1 scale and uses a 6-month span of Place visits for a given geographic area. |
| `price` | A numerical value (from 1 to 4) that best describes the pricing tier of the FSQ Place, based on known prices for menu items and other offerings. Values include: - `1` = Cheap - `2` = Moderate - `3` = Expensive - `4` = Very Expensive. |
| `rating` | A numerical rating (from 0.0 to 10.0) of the FSQ Place, based on user votes, likes/dislikes, tips sentiment, and visit data. Not all FSQ Places will have a rating. |
| `stats` | An object containing counts of photos, ratings, and tips. Included subfields: `total_photos`, `total_ratings`, and `total_tips`. |
| `tastes` | An array of up to 25 tastes describing the FSQ Place. |
| `tips` | An array containing the following subfields: `created_at` and `text`. |
| `veracity_rating` | A 1 to 5 star quality rating indicating how complete, current, and reliable the location's information is, helping you understand how strong our data signals are for this place. `5` = Highly reliable and complete information, regularly updated from trusted sources and real user activity. `4` = Very good information verified from reliable sources with some user engagement. `3` = Good essential information from multiple sources that's generally current but less frequently updated. `2` = Basic information available but some details may be missing or outdated. `1` = Limited information from few sources that may not have been updated recently. |

## Swift example models

```swift
import Foundation

struct FoursquareCategory: Codable, Identifiable, Hashable {
    let fsqCategoryId: String?
    let name: String?
    let shortName: String?
    let pluralName: String?
    let icon: FoursquareCategoryIcon?

    var id: String { fsqCategoryId ?? name ?? "category" }

    enum CodingKeys: String, CodingKey {
        case fsqCategoryId = "fsq_category_id"
        case name = "name"
        case shortName = "short_name"
        case pluralName = "plural_name"
        case icon = "icon"
    }
}

struct FoursquareCategoryIcon: Codable, Identifiable, Hashable {
    let iconId: String?
    let prefix: String?
    let suffix: String?

    var id: String { iconId ?? "category-icon" }

    enum CodingKeys: String, CodingKey {
        case iconId = "id"
        case prefix = "prefix"
        case suffix = "suffix"
    }
}

struct FoursquareChain: Codable, Identifiable, Hashable {
    let fsqChainId: String?
    let name: String?

    var id: String { fsqChainId ?? name ?? "chain" }

    enum CodingKeys: String, CodingKey {
        case fsqChainId = "fsq_chain_id"
        case name = "name"
    }
}

struct FoursquareCorePlaceFields: Codable, Identifiable, Hashable {
    let fsqPlaceId: String?
    let name: String?
    let categories: [FoursquareCategory]?
    let chains: [FoursquareChain]?

    var id: String { fsqPlaceId ?? name ?? "place" }

    enum CodingKeys: String, CodingKey {
        case fsqPlaceId = "fsq_place_id"
        case name = "name"
        case categories = "categories"
        case chains = "chains"
    }
}
```
