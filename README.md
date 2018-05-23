Realtime DataFeed API Documentation
===================================

Here will present an [example request](#example-request) and the [request model explained](#request-model).

<a name="example-request"></a> Example request
---------------
```
POST https://api.commercialpeople.com/import
Content-Type: application/json
```
json example:
```
{
  "network": {
    "authenticate": "9c71e343c9324c69fc397696d9a284bd",
    "timestamp": "2017-02-23 11:53:42",
    "submission_type": "all_update"
  },
  "branch": {
    "branch_id": "CP55CC38E797361123131",
    "channel": 1,
    "branch_details": {
      "branch_name": "Scotland - Edinburgh",
      "branch_logo": "http://www.agent-example.com/logo.png",
      "branch_website": "http://www.example.com",
      "branch_social": {
        "twitter": "https://twitter.com/CommercialPplUK",
        "linkedin": "https://twitter.com/CommercialPplUK",
        "youtube": "https://www.youtube.com/user/CommPpl/videos"
      },
      "branch_building_name": "The Big Building",
      "branch_address_1": "225 Marsh Wall",
      "branch_address_2": "Isle of Dogs",
      "branch_town": "London",
      "branch_postcode": "E14 9FW",
      "branch_email": "branch@commercialpeople.com",
      "branch_telephone": "02085948220"
    }
  },
  "property": {
    "agent_ref": "778",
    "published": 1,
    "property_type": [
      1,
      2
    ],
    "status": 1,
    "update_date": "2017-02-23 11:53:42",
    "address": {
      "building_door_number": "12",
      "building_name": "Marshwall Building",
      "address_2": "Marsh Wall",
      "address_3": "Isle of Dogs",
      "town": "London",
      "postcode_1": "E14",
      "postcode_2": "9FW",
      "display_address": "Marshwall Building, Isle of Dogs, London",
      "country": "GB",
      "region": "East End",
      "latitude": "51.499856",
      "longitude": "-0.011954"
    },
    "price_information": {
      "currency": "GBP",
      "price_rent_min": 1200,
      "price_rent_max": 1500,
      "price_sales_min": 350000,
      "price_sales_max": 400000,
      "price_qualifier": 0,
      "tenure_type": 1,
      "price_rent_per_unit_area": 14,
      "rent_frequency": 1
    },
    "details": {
      "summary": "A mixed use development located on a recent 25 acre extension of the North Road Trading Estate on the outskirts of Berwick on Tweed",
      "description": "Bla bla",
      "internal_min_area": 1035,
      "internal_max_area": 146256,
      "internal_area_unit": 1
    },
    "listing_media": [
      {
        "media_type": 1,
        "media_file_name": "Image Name",
        "media_data": "base64 encoded content of image",
        "caption": "Front entrance"
      }
    ],
    "listing_contact": [
      {
        "contact_first_name": "Dwight",
        "contact_last_name": "Estava",
        "contact_email": "d.estava@commercialpeople.com",
        "contact_telephone": "0201 000 000",
        "contact_fax": "0201 000 000"
      }
    ]
  }
}
```

<a name="request-model"></a>Request model
-------------------------------------------
PHP eg of network object generation:
```
$time = time();
$network = [];
$network['authenticate'] = md5("CPBRANCHID" . (hash("sha224","your_agent_password")) . date('Y-m-d H:i:s', $time));
$network['timestamp'] = date('Y-m-d H:i:s', $time);
$network['submission_type'] = 'all_update';
```
Notes: very important to use same time value in 'authenticate' and 'timestamp' fields otherwise authorization will fail!

Below we list the request model explained

```
{
    "network": {
        "type": "object",
        "required": [
          "authenticate",
          "timestamp"
        ],
        "properties": {
            "authenticate": {
              "type": "string",
              "example": "9c71e343c9324c69fc397696d9a284bd",
              "description": "md5 hash of concatenated {branch_id}{sha224 hash of agent password}{current date format: 'Y-m-d H:i:s'}"
            },
            "timestamp": {
              "type": "string",
              "example": "2017-02-23 11:53:42",
              "description": "current date and time format yyyy-mm-dd hh:ii:ss"
            },
            "submission_type": {
              "type": "string",
              "example": "all_update"
            }
        }
    },
    "branch": {
        "type": "object",
        "required": [
          "branch_id",
          "channel"
        ],
        "properties": {
          "branch_id": {
            "type": "string",
            "example": "CP55CC38E797361XXX"
          },
          "channel": {
            "type": "integer",
            "enum": [
              1,
              2,
              3,
              4,
              5,
              6
            ],
            "example": 1,
            "description": "1 - for sale, 2 - for rent, 3 - for rent & for sale"
          },
          "branch_details": {
            "$ref": "#/definitions/BranchDetails"
          }
        }
    },
    "property": {
        "type": "object",
        "required": [
          "agent_ref",
          "published",
          "property_type",
          "status",
          "address",
          "price_information",
          "details"
        ],
        "properties": {
            "agent_ref": {
              "type": "string",
              "example": "778"
            },
            "published": {
              "type": "integer",
              "example": 1
            },
            "property_type": {
              "example": [
                1,
                2
              ],
              "type": "array",
              "items": {
                "type": "integer"
              }
            },
            "status": {
              "type": "integer",
              "enum": [
                1,
                2,
                3,
                4,
                5,
                6
              ],
              "example": 1,
              "description": "1 - Available, 2 - SSTC, 3 - SSTCM, 4 - Under Offer, 5 - Reserved, 6 - Let Agreed"
            },
            "update_date": {
              "type": "string",
              "example": "2017-02-23 11:53:42"
            },
            "address": {
                "type": "object",
                "required": [
                "town",
                "latitude",
                "longitude"
                ],
                "properties": {
                "building_door_number": {
                  "type": "string",
                  "example": "12"
                },
                "building_name": {
                  "type": "string",
                  "example": "Marshwall Building"
                },
                "address_2": {
                  "type": "string",
                  "example": "Marsh Wall"
                },
                "address_3": {
                  "type": "string",
                  "example": "Isle of Dogs"
                },
                "town": {
                  "type": "string",
                  "example": "London"
                },
                "postcode_1": {
                  "type": "string",
                  "example": "E14"
                },
                "postcode_2": {
                  "type": "string",
                  "example": "9FW"
                },
                "display_address": {
                  "type": "string",
                  "example": "Marshwall Building, Isle of Dogs, London"
                },
                "country": {
                  "type": "string",
                  "example": "GB"
                },
                "region": {
                  "type": "string",
                  "example": "East End"
                },
                "latitude": {
                  "type": "string",
                  "example": "51.499856"
                },
                "longitude": {
                  "type": "string",
                  "example": "-0.011954"
                }
            },
            "price_information": {
                "type": "object",
                "properties": {
                    "currency": {
                        "type": "string",
                        "example": "GBP"
                    },
                    "price_rent_min": {
                        "type": "integer",
                        "example": 1200
                    },
                    "price_rent_max": {
                        "type": "integer",
                        "example": 1500
                    },
                    "price_sales_min": {
                        "type": "integer",
                        "example": 350000
                    },
                    "price_sales_max": {
                        "type": "integer",
                        "example": 400000
                    },
                    "price_qualifier": {
                        "type": "integer",
                        "enum": [
                          0,
                          1,
                          2,
                          3,
                          4,
                          5,
                          6,
                          7,
                          9,
                          10,
                          11,
                          12,
                          15,
                          16
                        ],
                        "description": "0 - Default, 1 - POA, 2 - Guide Price, 3 - Fixed Price, 4 - OIEO, 5 - OIRO, 6 - Sale by Tender, 7 - From, 9 - Shared Ownership, 10 - Offers over, 11 - Part buy/Part Rent, 12 - Shared Equity, 15 - Offers Invited"
                    },
                    "tenure_type": {
                        "type": "integer",
                        "enum": [
                          1,
                          2,
                          3,
                          4,
                          5,
                          6,
                          7
                        ],
                        "default": 1,
                        "description": "1 -freehold, 2 - leasehold, 3 - feudal, 4 - commonhold, 5 - share of freehold, 6 - long leasehold, 7 - freehold or leasehold"
                    },
                    "price_rent_per_unit_area": {
                        "type": "integer",
                        "example": 14
                    },
                    "rent_frequency": {
                        "type": "integer",
                        "enum": [
                          1,
                          4,
                          12,
                          52,
                          365
                        ],
                        "example": 1,
                        "description": "1 - Yearly, 4 - Quarterly, 12 - Monthly, 52 - Weekly, 365 - Daily"
                    }
                }
            },
            "details": {
                "type": "object",
                "required": [
                  "description"
                ],
                "properties": {
                    "summary": {
                        "type": "string",
                        "example": "A mixed use development located on a recent 25 acre extension of the North Road Trading Estate on the outskirts of Berwick on Tweed"
                    },
                    "description": {
                        "type": "string",
                        "example": "Bla bla"
                    },
                    "internal_min_area": {
                        "type": "integer",
                        "example": 1035
                    },
                    "internal_max_area": {
                        "type": "integer",
                        "example": 146256
                    },
                    "internal_area_unit": {
                        "type": "integer",
                        "example": 1,
                        "enum": [
                          1,
                          2,
                          3,
                          4
                        ],
                        "description": "1 - square_feet, 2 - square_metres, 3 - acres, 4 - hectares"
                    }
                }
            },
            "listing_media": {
                "type": "array",
                "items": {
                    "type": "object",
                    "properties": {
                        "media_type": {
                            "type": "integer",
                            "example": 1,
                            "description": "1,2,3"
                        },
                        "media_url": {
                            "type": "string",
                            "example": "http://images.com/image.jpg"
                        },
                        "media_file_name": {
                            "type": "string",
                            "example": "Image Name"
                        },
                        "media_data": {
                            "type": "string",
                            "example": "base64 encoded content"
                        },
                        "caption": {
                            "type": "string",
                            "example": "Front entrance"
                        }
                    }
                }
            },
            "listing_contact": {
                "type": "array",
                "items": {
                    "type": "object",
                    "properties": {
                        "contact_first_name": {
                            "type": "string",
                            "example": "Dwight"
                        },
                        "contact_last_name": {
                            "type": "string",
                            "example": "Estava"
                        },
                        "contact_email": {
                            "type": "string",
                            "example": "d.estava@commercialpeople.com"
                        },
                        "contact_telephone": {
                            "type": "string",
                            "example": "0201 000 000"
                        },
                        "contact_fax": {
                            "type": "string",
                            "example": "0201 000 001"
                        }
                    }
                }
            }
        }
    }
}
```
