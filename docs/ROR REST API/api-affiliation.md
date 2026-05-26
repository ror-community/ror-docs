---
title: Affiliation parameter
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR API affiliation parameter
  description: >-
    Instructions for using the affiliation parameter of the ROR API to match
    text strings to ROR IDs.
  robots: index
next:
  pages:
    - slug: matching
      title: Match organization names to ROR IDs
      type: basic
    - title: ROR blog posts on matching
      type: link
      url: https://ror.org/tags/matching/
    - title: ROR single search affiliation matching strategy demo (YouTube)
      type: link
      url: https://www.youtube.com/watch?v=DC7mZSnECsQ
---
# About the affiliation parameter

For many years, publishers of scholarly information often captured author and contributor affiliation information as unstructured data, sometimes storing an organization's name, a sub-unit or department's name, a street address, and a geographical location along with copious irregular punctuation as a text string in a single field. Some affiliation strings even include multiple affiliations.

The affiliation parameter of the ROR API is designed to match these messy text strings to ROR records automatically to produce cleaner affiliation data. The affiliation service attempts to find the ROR record that is the most probable match for the given affiliation string; if it finds a likely candidate, it returns that result with a `chosen:true` indicator that can be used in ROR integration code. Not all affiliation searches will produce a "chosen" result. The method of matching is also indicated in the `matching_type` field.

Additional possibilities that might match the string are also included in results, listed in descending order by confidence `score`. Note that **we do not recommend using the confidence `score` to select matches**; use the `chosen:true` indicator instead.

An API-based approach to matching affiliation strings to ROR IDs can work well for large-scale systems where human review of every proposed match is impractical, but **no large-scale programmatic approach to matching is perfect**, especially since there are many similar and even identical names and acronyms among research organizations globally. Often, the matching service will not be able to suggest a match for a particular string, and in some cases, the matching service might suggest an incorrect match. Human review is always the best fallback.

<Callout icon="📘" theme="info">
  ## Affiliation parameter format

  `https://api.ror.org/v2/organizations?affiliation=[URL-encoded-string]`
</Callout>

# Formatting searches

All request strings must be [URL-encoded](https://www.w3schools.com/tags/ref_urlencode.asp). The affiliation parameter is specifically designed to handle strings with punctuation, special characters, and spaces, so **it is not necessary to enclose multi-term search strings in quotation marks or to escape special characters**.

# Paging and filtering

The affiliation parameter **does not accept filters** and results **are not paginated**. If [filter](doc:api-filtering) syntax is added to the end of an affiliation query, the terms will be treated as part of the affiliation search and will produce unwanted results. Results are listed in descending order by matching confidence score.

<Callout icon="🚧" theme="warn">
  ## Be aware of differences between the affiliation parameter and the query parameters

  Unlike the [?query parameter](https://ror.readme.io/docs/api-query) and the [?query.advanced parameter](https://ror.readme.io/docs/api-advanced-query), the affiliation parameter does not accept filters, and results are not paginated. If filter syntax is added to the end of an affiliation search, the terms will be treated as part of the affiliation search.

  Also unlike the query and advanced query parameters, the affiliation parameter expects multi-word strings that include spaces, punctuation, and special characters. Surrounding terms in quotation marks or escaping special characters can produce worse results when using the affiliation parameter.

  The affiliation parameter is designed for automatic matching of text strings to ROR IDs. The query parameter and the advanced query parameter are designed for use cases in which a person chooses a result, such as <Anchor label="ROR-powered forms" target="_blank" href="doc:forms">ROR-powered forms</Anchor> and the <Anchor label="ROR web search" target="_blank" href="doc:web-search">ROR web search</Anchor>.
</Callout>

# Matching a string

Use the affiliation parameter to match a text string that includes an organization name to a ROR ID. If a likely match is found, it will be identified with the `chosen:true` indicator and can be automatically selected. No more than one result (if any) will include the `chosen:true` indicator.

## Example

A text string such as "Dept. of Microbiology, University of Pennsylvania School of Medicine, Philadelphia, PA, 19104" can be matched to a ROR ID with the affiliation parameter. Note that the string must be URL-encoded.

```curl
curl 'https://api.ror.org/v2/organizations?affiliation=Dept.%20of%20Microbiology%2C%20University%20of%20Pennsylvania%20School%20of%20Medicine%2C%20Philadelphia%2C%20PA%2C%2019104' | json_pp
```

The list of results includes a `chosen:true` indicator that correctly matches the string to the ROR record for the University of Pennsylvania, [https://ror.org/00b30xv10](https://ror.org/00b30xv10).

```json
{
   "items" : [
      {
         "chosen" : true,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2025-05-05",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "upenn.edu"
            ],
            "established" : 1740,
            "external_ids" : [
               {
                  "all" : [
                     "100006920",
                     "100005312",
                     "100007929",
                     "100007928",
                     "100005882",
                     "100005467",
                     "100008435",
                     "100010410",
                     "100010490",
                     "100009949",
                     "100007486",
                     "100017303"
                  ],
                  "preferred" : "100006920",
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.25879.31"
                  ],
                  "preferred" : "grid.25879.31",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 1936 8972"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q49117"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/00b30xv10",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.upenn.edu"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/University_of_Pennsylvania"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "PA",
                     "country_subdivision_name" : "Pennsylvania",
                     "lat" : 39.95238,
                     "lng" : -75.16362,
                     "name" : "Philadelphia"
                  },
                  "geonames_id" : 4560349
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "alias"
                  ],
                  "value" : "UPenn"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "University of Pennsylvania"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/047939x15",
                  "label" : "Penn Center for AIDS Research",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05hs3x327",
                  "label" : "American Research Institute in Turkey",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/01z7r7q48",
                  "label" : "Children's Hospital of Philadelphia",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/02917wp91",
                  "label" : "Hospital of the University of Pennsylvania",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/00swv7d52",
                  "label" : "Penn Presbyterian Medical Center",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/03mvdc478",
                  "label" : "Pennsylvania Hospital",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/03j05zz84",
                  "label" : "Philadelphia VA Medical Center",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/04h81rw26",
                  "label" : "University of Pennsylvania Health System",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/03xwa9562",
                  "label" : "University of Pennsylvania Press",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 1,
         "substring" : "Dept. of Microbiology, University of Pennsylvania School of Medicine, Philadelphia, PA, 19104"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2024-04-06",
                  "schema_version" : "2.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : null,
            "external_ids" : [
               {
                  "all" : [
                     "0000 0001 2296 1126"
                  ],
                  "preferred" : "0000 0001 2296 1126",
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q2123017"
                  ],
                  "preferred" : "Q2123017",
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/02ets8c94",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://medicine.iu.edu"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Indiana_University_School_of_Medicine"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "IN",
                     "country_subdivision_name" : "Indiana",
                     "lat" : 39.76838,
                     "lng" : -86.15804,
                     "name" : "Indianapolis"
                  },
                  "geonames_id" : 4259418
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "IU School of Medicine"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IUSM"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Indiana University School of Medicine"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/01kg8sb98",
                  "label" : "Indiana University",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/01aaptx40",
                  "label" : "Indiana University Health",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.95,
         "substring" : "Dept. of Microbiology, University of Pennsylvania School of Medicine, Philadelphia, PA, 19104"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2025-03-26",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1970,
            "external_ids" : [
               {
                  "all" : [
                     "100008571"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.280418.7"
                  ],
                  "preferred" : "grid.280418.7",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 0705 8684"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q7570024"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/0232r4451",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.siumed.edu/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/Southern_Illinois_University_School_of_Medicine"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "IL",
                     "country_subdivision_name" : "Illinois",
                     "lat" : 39.80172,
                     "lng" : -89.64371,
                     "name" : "Springfield"
                  },
                  "geonames_id" : 4250542
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "SIU Medicine"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "SIU School of Medicine"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Southern Illinois University School of Medicine"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/05vz28418",
                  "label" : "Southern Illinois University System",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/02hxrag63",
                  "label" : "Memorial Medical Center",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/04p4s5b35",
                  "label" : "St. John's Hospital",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 0.91,
         "substring" : "Dept. of Microbiology, University of Pennsylvania School of Medicine, Philadelphia, PA, 19104"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2024-03-28",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1890,
            "external_ids" : [
               {
                  "all" : [
                     "0000 0001 0634 5307"
                  ],
                  "preferred" : "0000 0001 0634 5307",
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q2037236"
                  ],
                  "preferred" : "Q2037236",
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/03xwa9562",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.pennpress.org"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/University_of_Pennsylvania_Press"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "PA",
                     "country_subdivision_name" : "Pennsylvania",
                     "lat" : 39.95238,
                     "lng" : -75.16362,
                     "name" : "Philadelphia"
                  },
                  "geonames_id" : 4560349
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Penn Press"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "University of Pennsylvania Press"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/00b30xv10",
                  "label" : "University of Pennsylvania",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "nonprofit"
            ]
         },
         "score" : 0.88,
         "substring" : "Dept. of Microbiology, University of Pennsylvania School of Medicine, Philadelphia, PA, 19104"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1837,
            "external_ids" : [
               {
                  "all" : [
                     "grid.254107.5"
                  ],
                  "preferred" : "grid.254107.5",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "Q3445541"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/02nckwn80",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.cheyney.edu/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/Cheyney_University_of_Pennsylvania"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "PA",
                     "country_subdivision_name" : "Pennsylvania",
                     "lat" : 39.96097,
                     "lng" : -75.60804,
                     "name" : "West Chester"
                  },
                  "geonames_id" : 4562144
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Cheyney University of Pennsylvania"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label"
                  ],
                  "value" : "Universidad de Cheyney de Pensilvania"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "label"
                  ],
                  "value" : "Université cheyney de pennsylvanie"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/00m2s0z68",
                  "label" : "Pennsylvania State System of Higher Education",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.82,
         "substring" : "Dept. of Microbiology, University of Pennsylvania School of Medicine, Philadelphia, PA, 19104"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "iup.edu"
            ],
            "established" : 1875,
            "external_ids" : [
               {
                  "all" : [
                     "grid.257427.1"
                  ],
                  "preferred" : "grid.257427.1",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0000 8874 0847"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q1661325"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/0511cmw96",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.iup.edu"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/Indiana_University_of_Pennsylvania"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "PA",
                     "country_subdivision_name" : "Pennsylvania",
                     "lat" : 40.62146,
                     "lng" : -79.15253,
                     "name" : "Indiana"
                  },
                  "geonames_id" : 5194868
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IUP"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Indiana University of Pennsylvania"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label"
                  ],
                  "value" : "Universidad de Pensilvania en Indiana"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/00m2s0z68",
                  "label" : "Pennsylvania State System of Higher Education",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.82,
         "substring" : "Dept. of Microbiology, University of Pennsylvania School of Medicine, Philadelphia, PA, 19104"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2019-11-07",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1914,
            "external_ids" : [
               {
                  "all" : [
                     "grid.507405.3"
                  ],
                  "preferred" : "grid.507405.3",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 0740 3062"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q3267626"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/02gsb6172",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.philadelphiafed.org/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Federal_Reserve_Bank_of_Philadelphia"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "PA",
                     "country_subdivision_name" : "Pennsylvania",
                     "lat" : 39.95238,
                     "lng" : -75.16362,
                     "name" : "Philadelphia"
                  },
                  "geonames_id" : 4560349
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Federal Reserve Bank of Philadelphia"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Philadelphia Fed"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Philly Fed"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/05fk5tr28",
                  "label" : "Federal Reserve",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "other"
            ]
         },
         "score" : 0.81,
         "substring" : "Dept. of Microbiology, University of Pennsylvania School of Medicine, Philadelphia, PA, 19104"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1995,
            "external_ids" : [
               {
                  "all" : [
                     "100004906"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.448596.2"
                  ],
                  "preferred" : "grid.448596.2",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 0509 3701"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q15265911"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/00wd70b02",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.dep.pa.gov/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Pennsylvania_Department_of_Environmental_Protection"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "PA",
                     "country_subdivision_name" : "Pennsylvania",
                     "lat" : 40.21924,
                     "lng" : -79.60948,
                     "name" : "New Stanton"
                  },
                  "geonames_id" : 5203263
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "DEP"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Pennsylvania DEP"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Pennsylvania Department of Environmental Protection"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "funder",
               "government"
            ]
         },
         "score" : 0.81,
         "substring" : "Dept. of Microbiology, University of Pennsylvania School of Medicine, Philadelphia, PA, 19104"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2008,
            "external_ids" : [
               {
                  "all" : [
                     "grid.472391.a"
                  ],
                  "preferred" : "grid.472391.a",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 4689 1304"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q16901997"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/00z50t224",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.trinityschoolofmedicine.org/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Trinity_School_of_Medicine"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "VC",
                     "country_name" : "St Vincent and Grenadines",
                     "country_subdivision_code" : "04",
                     "country_subdivision_name" : "Saint George Parish",
                     "lat" : 13.15527,
                     "lng" : -61.22742,
                     "name" : "Kingstown"
                  },
                  "geonames_id" : 3577887
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "TSOM"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Trinity School of Medicine"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.81,
         "substring" : "Dept. of Microbiology, University of Pennsylvania School of Medicine, Philadelphia, PA, 19104"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "kutztown.edu"
            ],
            "established" : 1866,
            "external_ids" : [
               {
                  "all" : [
                     "grid.258769.7"
                  ],
                  "preferred" : "grid.258769.7",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 0160 0129"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q1340908"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/056veft20",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://kutztown.edu"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/Kutztown_University_of_Pennsylvania"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "PA",
                     "country_subdivision_name" : "Pennsylvania",
                     "lat" : 40.51732,
                     "lng" : -75.77742,
                     "name" : "Kutztown"
                  },
                  "geonames_id" : 5196621
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "KU"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Kutztown University"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Kutztown University of Pennsylvania"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/00m2s0z68",
                  "label" : "Pennsylvania State System of Higher Education",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.8,
         "substring" : "Dept. of Microbiology, University of Pennsylvania School of Medicine, Philadelphia, PA, 19104"
      }
   ],
   "number_of_results" : 10
}
```

## Example

Relatively simple text strings for organization names alone, such as "Virginia Tech," can also be matched to ROR IDs.

```curl
curl 'https://api.ror.org/v2/organizations?affiliation=Virginia%20Tech' | json_pp
```

The list of results includes a `chosen:true` indicator that correctly matches the string to the ROR ID for Virginia Tech, [https://ror.org/02smfhw86](https://ror.org/02smfhw86).

```json
{
   "items" : [
      {
         "chosen" : true,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2025-05-05",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "vt.edu"
            ],
            "established" : 1872,
            "external_ids" : [
               {
                  "all" : [
                     "100007263",
                     "100006601",
                     "100009522",
                     "100009778",
                     "100011501",
                     "100014523",
                     "100016447",
                     "100016464",
                     "100018638"
                  ],
                  "preferred" : "100007263",
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.438526.e"
                  ],
                  "preferred" : "grid.438526.e",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 0694 4940"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q65379"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/02smfhw86",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.vt.edu"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/Virginia_Tech"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "VA",
                     "country_subdivision_name" : "Virginia",
                     "lat" : 37.22957,
                     "lng" : -80.41394,
                     "name" : "Blacksburg"
                  },
                  "geonames_id" : 4747845
               }
            ],
            "names" : [
               {
                  "lang" : "fr",
                  "types" : [
                     "label"
                  ],
                  "value" : "Institut polytechnique et université d'État de virginie"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label"
                  ],
                  "value" : "Instituto Politécnico y Universidad Estatal de Virginia"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Virginia Polytechnic Institute and State University"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Virginia Tech"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/01q1y4t48",
                  "label" : "Virginia Tech - Wake Forest University School of Biomedical Engineering & Sciences",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/010prmy50",
                  "label" : "Virginia–Maryland College of Veterinary Medicine",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05953j253",
                  "label" : "Virginia Tech Transportation Institute",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/0073tn017",
                  "label" : "Virginia Agricultural Experiment Station",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/036nxkh98",
                  "label" : "Carilion Roanoke Memorial Hospital",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/00mkh7345",
                  "label" : "Hubbard Brook Long Term Ecological Research",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/04r3s7465",
                  "label" : "McMurdo Dry Valleys Long Term Ecological Research",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/013zder45",
                  "label" : "Virginia Coast Reserve Long Term Ecological Research",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 1,
         "substring" : "Virginia Tech"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1998,
            "external_ids" : [
               {
                  "all" : [
                     "grid.422831.e"
                  ],
                  "preferred" : "grid.422831.e",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "Q30283944"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/009rbsv73",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.thetech.org/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/The_Tech_Interactive"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "CA",
                     "country_subdivision_name" : "California",
                     "lat" : 37.33939,
                     "lng" : -121.89496,
                     "name" : "San Jose"
                  },
                  "geonames_id" : 5392171
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Tech Museum of Innovation"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "The Tech"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "The Tech Interactive"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "archive"
            ]
         },
         "score" : 1,
         "substring" : "Virginia Tech"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2019-05-06",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1993,
            "external_ids" : [
               {
                  "all" : [
                     "grid.504909.1"
                  ],
                  "preferred" : "grid.504909.1",
                  "type" : "grid"
               }
            ],
            "id" : "https://ror.org/04h6dxr28",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.natech-inc.com/index.htm"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "CO",
                     "country_subdivision_name" : "Colorado",
                     "lat" : 39.75554,
                     "lng" : -105.2211,
                     "name" : "Golden"
                  },
                  "geonames_id" : 5423294
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "alias"
                  ],
                  "value" : "NA Tech"
               },
               {
                  "lang" : null,
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Native American Technologies (United States)"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "company"
            ]
         },
         "score" : 0.96,
         "substring" : "Virginia Tech"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1961,
            "external_ids" : [
               {
                  "all" : [
                     "grid.461948.6"
                  ],
                  "preferred" : "grid.461948.6",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0000 9627 7653"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q5439067"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/02twzqt54",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.faytechcc.edu/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Fayetteville_Technical_Community_College"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "NC",
                     "country_subdivision_name" : "North Carolina",
                     "lat" : 35.05266,
                     "lng" : -78.87836,
                     "name" : "Fayetteville"
                  },
                  "geonames_id" : 4466033
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "FTCC"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Fay Tech"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Fayetteville Technical Community College"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.92,
         "substring" : "Virginia Tech"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1967,
            "external_ids" : [
               {
                  "all" : [
                     "grid.469186.4"
                  ],
                  "preferred" : "grid.469186.4",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 0605 6419"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q16900342"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/03be58a36",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.sautech.edu/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Southern_Arkansas_University_Tech"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "AR",
                     "country_subdivision_name" : "Arkansas",
                     "lat" : 33.58456,
                     "lng" : -92.83433,
                     "name" : "Camden"
                  },
                  "geonames_id" : 4104182
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "SAU Tech"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Southern Arkansas University Tech"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.92,
         "substring" : "Virginia Tech"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2019-05-06",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2009,
            "external_ids" : [
               {
                  "all" : [
                     "grid.504699.7"
                  ],
                  "preferred" : "grid.504699.7",
                  "type" : "grid"
               }
            ],
            "id" : "https://ror.org/05qzrmp73",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://instrumentalpolymer.com/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "CA",
                     "country_subdivision_name" : "California",
                     "lat" : 34.14584,
                     "lng" : -118.80565,
                     "name" : "Westlake Village"
                  },
                  "geonames_id" : 5408395
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "alias"
                  ],
                  "value" : "IP TECH"
               },
               {
                  "lang" : null,
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Instrumental Polymer Technologies (United States)"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "company"
            ]
         },
         "score" : 0.92,
         "substring" : "Virginia Tech"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2025-09-19",
                  "schema_version" : "2.1"
               },
               "last_modified" : {
                  "date" : "2025-09-22",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2013,
            "external_ids" : [],
            "id" : "https://ror.org/03amy1q65",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.cea.fr/cea-tech"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "FR",
                     "country_name" : "France",
                     "country_subdivision_code" : "ARA",
                     "country_subdivision_name" : "Auvergne-Rhône-Alpes",
                     "lat" : 45.17869,
                     "lng" : 5.71479,
                     "name" : "Grenoble"
                  },
                  "geonames_id" : 3014728
               }
            ],
            "names" : [
               {
                  "lang" : "fr",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "CEA PRTT"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "alias"
                  ],
                  "value" : "CEA Tech"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "CEA-TECH-REG"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "alias"
                  ],
                  "value" : "CEATechRegion"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Plateformes régionales de transferts technologiques"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/00jjx8s55",
                  "label" : "Commissariat à l'Énergie Atomique et aux Énergies Alternatives",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "facility",
               "government"
            ]
         },
         "score" : 0.92,
         "substring" : "Virginia Tech"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1989,
            "external_ids" : [
               {
                  "all" : [
                     "grid.455267.7"
                  ],
                  "preferred" : "grid.455267.7",
                  "type" : "grid"
               }
            ],
            "id" : "https://ror.org/03gc99a64",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.cftechnologies.com/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "MA",
                     "country_subdivision_name" : "Massachusetts",
                     "lat" : 42.35843,
                     "lng" : -71.05977,
                     "name" : "Boston"
                  },
                  "geonames_id" : 4930956
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "alias"
                  ],
                  "value" : "CF Tech"
               },
               {
                  "lang" : null,
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "CF Technologies (United States)"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "company"
            ]
         },
         "score" : 0.92,
         "substring" : "Virginia Tech"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2007,
            "external_ids" : [
               {
                  "all" : [
                     "grid.470461.0"
                  ],
                  "preferred" : "grid.470461.0",
                  "type" : "grid"
               }
            ],
            "id" : "https://ror.org/018m4ac75",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.nutechtransfer.org/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "NC",
                     "country_subdivision_name" : "North Carolina",
                     "lat" : 35.82348,
                     "lng" : -78.82556,
                     "name" : "Morrisville"
                  },
                  "geonames_id" : 4480285
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "NU Tech"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Technology Partnership of Nagoya University"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "nonprofit"
            ]
         },
         "score" : 0.92,
         "substring" : "Virginia Tech"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2025-02-26",
                  "schema_version" : "2.1"
               },
               "last_modified" : {
                  "date" : "2025-11-24",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "sut.edu.eg"
            ],
            "established" : 2023,
            "external_ids" : [],
            "id" : "https://ror.org/05kay3028",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://sut.edu.eg"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "AF",
                     "continent_name" : "Africa",
                     "country_code" : "EG",
                     "country_name" : "Egypt",
                     "country_subdivision_code" : "C",
                     "country_subdivision_name" : "Cairo",
                     "lat" : 30.06263,
                     "lng" : 31.24967,
                     "name" : "Cairo"
                  },
                  "geonames_id" : 360630
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "ElSewedy University of Technology"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "ElSewedy University of Technology - POLYTECHNIC OF EGYPT"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "SU Tech"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "SU Tech ElSewedy University"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "SUT"
               },
               {
                  "lang" : "ar",
                  "types" : [
                     "label"
                  ],
                  "value" : "جامعة السويدي للتكنولوجيا"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.92,
         "substring" : "Virginia Tech"
      }
   ],
   "number_of_results" : 10
}
```

# No matches found

When there's no result with the `chosen:true` indicator (i.e., all results of a query are `chosen:false`) it can mean that the string does not include enough information for the algorithm to find a match, or that there are several good matches but no clear winner, or that the organization is not in ROR. If there is no result with `chosen:true`, some additional review by humans or machines is almost always needed in order to match the string to a ROR ID.

In some cases, a change to ROR data such as adding a common `alias` or `label` to a ROR record can improve matching results. Please <Anchor label="submit a curation request" target="_blank" href="https://curation-request.ror.org">submit a curation request</Anchor> to ask for a specific change to a ROR record or reach out to us at [support@ror.org](mailto:support@ror.org) if it is unclear what should be changed.

<Callout icon="❗️" theme="error">
  ## Don't automatically select results by score or order

  When no result has the `chosen: true` indicator, there might be no match with a high score, or several results might have the exact same score. In these cases, it is best to respect the absence of the `chosen:true` indicator. If there is no result with `chosen:true`, leave the string unmatched or add an additional layer of human or machine matching, at your discretion. Don't automatically select results higher than a particular confidence score, and don't automatically select the first result in the list.
</Callout>

## Example

An affiliation string such as "UCL School of Slavonic and East European Studies" does not contain enough identifying information for the ROR API affiliation matching service to choose a matching ROR record.

```curl
curl 'https://api.ror.org/v2/organizations?affiliation=UCL%20School%20of%20Slavonic%20and%20East%20European%20Studies' | json_pp
```

The response returns results with confidence scores ranging from .79 to .7 listed in descending order, but the affiliation matching service does not rate any of these results as a recommended match. In such cases, best practice is to add additional human or machine review or to leave the string unmatched.

```json
{
   "items" : [
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1950,
            "external_ids" : [
               {
                  "all" : [
                     "grid.493342.b"
                  ],
                  "preferred" : "grid.493342.b",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 0384 5818"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/030v00e77",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.iesabroad.org/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Institute_for_the_International_Education_of_Students"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "IL",
                     "country_subdivision_name" : "Illinois",
                     "lat" : 41.85003,
                     "lng" : -87.65005,
                     "name" : "Chicago"
                  },
                  "geonames_id" : 4887398
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IES"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "IES Abroad"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Institute for the International Education of Students"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Institute of European Studies"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "nonprofit"
            ]
         },
         "score" : 0.79,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2024-12-09",
                  "schema_version" : "2.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "ies.rs"
            ],
            "established" : 1992,
            "external_ids" : [],
            "id" : "https://ror.org/018v7v020",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://ies.rs"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "RS",
                     "country_name" : "Serbia",
                     "country_subdivision_code" : null,
                     "country_subdivision_name" : "Central Serbia",
                     "lat" : 44.80401,
                     "lng" : 20.46513,
                     "name" : "Belgrade"
                  },
                  "geonames_id" : 792680
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IES"
               },
               {
                  "lang" : "sr",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Institut za evropske studije"
               },
               {
                  "lang" : "sr",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Institut za evropske studije, Beograd, Trg Nikole Pašića 11"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Institute of European Studies"
               },
               {
                  "lang" : "sr",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Институт за европске студије"
               },
               {
                  "lang" : "sr",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Институт за европске студије, Београд, Трг Николе Пашића 11"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "facility"
            ]
         },
         "score" : 0.79,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1994,
            "external_ids" : [
               {
                  "all" : [
                     "100009050"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.458378.5"
                  ],
                  "preferred" : "grid.458378.5",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 5900 8576"
                  ],
                  "preferred" : "0000 0004 5900 8576",
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/055mqk127",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://ostersjostiftelsen.se/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "SE",
                     "country_name" : "Sweden",
                     "country_subdivision_code" : "AB",
                     "country_subdivision_name" : "Stockholm",
                     "lat" : 59.23705,
                     "lng" : 17.98192,
                     "name" : "Huddinge"
                  },
                  "geonames_id" : 2704620
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Foundation for Baltic and East European Studies"
               },
               {
                  "lang" : "sv",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Östersjöstiftelsen"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "facility",
               "funder"
            ]
         },
         "score" : 0.78,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2000,
            "external_ids" : [
               {
                  "all" : [
                     "grid.445206.1"
                  ],
                  "preferred" : "grid.445206.1",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "Q12788728"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/04jj9d877",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://fds.nova-uni.si"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "SI",
                     "country_name" : "Slovenia",
                     "country_subdivision_code" : "052",
                     "country_subdivision_name" : "Kranj",
                     "lat" : 46.23887,
                     "lng" : 14.35561,
                     "name" : "Kranj"
                  },
                  "geonames_id" : 3197378
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "FSS"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Graduate School of Government and European Studies"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.76,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2001,
            "external_ids" : [
               {
                  "all" : [
                     "grid.445954.d"
                  ],
                  "preferred" : "grid.445954.d",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 0399 1962"
                  ],
                  "preferred" : "0000 0004 0399 1962",
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q2304477"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/00m04y575",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.seeu.edu.mk/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/South_East_European_University"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "MK",
                     "country_name" : "North Macedonia",
                     "country_subdivision_code" : "609",
                     "country_subdivision_name" : "Tetovo",
                     "lat" : 42.00973,
                     "lng" : 20.97155,
                     "name" : "Tetovo"
                  },
                  "geonames_id" : 785082
               }
            ],
            "names" : [
               {
                  "lang" : "tr",
                  "types" : [
                     "label"
                  ],
                  "value" : "Güneydoğu Avrupa Üniversitesi"
               },
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "SEEU"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "South East European University"
               },
               {
                  "lang" : "sq",
                  "types" : [
                     "label"
                  ],
                  "value" : "Universiteti i Evropës Juglindore"
               },
               {
                  "lang" : "mk",
                  "types" : [
                     "label"
                  ],
                  "value" : "Универзитет на Југоисточна Европа"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.73,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2024-03-13",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : null,
            "external_ids" : [],
            "id" : "https://ror.org/007bytg49",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.acadsudest.ro"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "RO",
                     "country_name" : "Romania",
                     "country_subdivision_code" : "B",
                     "country_subdivision_name" : "București",
                     "lat" : 44.43225,
                     "lng" : 26.10626,
                     "name" : "Bucharest"
                  },
                  "geonames_id" : 683506
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Institute for South-East European Studies"
               },
               {
                  "lang" : "ro",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Institutul de Studii Sud-Est Europene"
               },
               {
                  "lang" : "ro",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Institutul de Studii Sud-Est Europene al Academiei Române"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "The Romanian Academy’s Institute for South-East European Studies"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/0561n6946",
                  "label" : "Romanian Academy",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "facility"
            ]
         },
         "score" : 0.71,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1971,
            "external_ids" : [
               {
                  "all" : [
                     "100010177"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.431734.6"
                  ],
                  "preferred" : "grid.431734.6",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 0558 9641"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q6023582"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/046z54t28",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.iue.edu/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Indiana_University_East"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "IN",
                     "country_subdivision_name" : "Indiana",
                     "lat" : 39.82894,
                     "lng" : -84.89024,
                     "name" : "Richmond"
                  },
                  "geonames_id" : 4263681
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "alias"
                  ],
                  "value" : "IU East"
               },
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IUE"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Indiana University East"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/01kg8sb98",
                  "label" : "Indiana University",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 0.71,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1987,
            "external_ids" : [
               {
                  "all" : [
                     "grid.497069.5"
                  ],
                  "preferred" : "grid.497069.5",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 2181 997X"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/00mh5ga26",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.jreast.co.jp/e/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/East_Japan_Railway_Company"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "AS",
                     "continent_name" : "Asia",
                     "country_code" : "JP",
                     "country_name" : "Japan",
                     "country_subdivision_code" : "13",
                     "country_subdivision_name" : "Tokyo",
                     "lat" : 35.6895,
                     "lng" : 139.69171,
                     "name" : "Tokyo"
                  },
                  "geonames_id" : 1850147
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "East Japan Railway (Japan)"
               },
               {
                  "lang" : null,
                  "types" : [
                     "alias"
                  ],
                  "value" : "JR East"
               },
               {
                  "lang" : "ja",
                  "types" : [
                     "label"
                  ],
                  "value" : "東日本旅客鉄道株式会社"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "company"
            ]
         },
         "score" : 0.71,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1994,
            "external_ids" : [
               {
                  "all" : [
                     "grid.496171.c"
                  ],
                  "preferred" : "grid.496171.c",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 5940 048X"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/01h6y6f73",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.europe.ac.kr/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "AS",
                     "continent_name" : "Asia",
                     "country_code" : "KR",
                     "country_name" : "South Korea",
                     "country_subdivision_code" : "11",
                     "country_subdivision_name" : "Seoul",
                     "lat" : 37.566,
                     "lng" : 126.9784,
                     "name" : "Seoul"
                  },
                  "geonames_id" : 1835848
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "KSCES"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Korea European Institute"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "The Korean Society of Contemporary European Studies"
               },
               {
                  "lang" : "ko",
                  "types" : [
                     "label"
                  ],
                  "value" : "한국유럽학회"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "other"
            ]
         },
         "score" : 0.71,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2025-05-19",
                  "schema_version" : "2.1"
               },
               "last_modified" : {
                  "date" : "2025-05-20",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "eessc.org.ua"
            ],
            "established" : 2022,
            "external_ids" : [],
            "id" : "https://ror.org/048sjjr07",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://eessc.org.ua"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "UA",
                     "country_name" : "Ukraine",
                     "country_subdivision_code" : "46",
                     "country_subdivision_name" : "Lviv",
                     "lat" : 49.83826,
                     "lng" : 24.02324,
                     "name" : "Lviv"
                  },
                  "geonames_id" : 702550
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "EESSC"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "East European Scientific Studies Center"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "The East European Scientific Studies Center"
               },
               {
                  "lang" : "uk",
                  "types" : [
                     "label"
                  ],
                  "value" : "Центр східноєвропейських наукових студій"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "nonprofit"
            ]
         },
         "score" : 0.7,
         "substring" : "UCL School of Slavonic and East European Studies"
      }
   ],
   "number_of_results" : 10
}
```

# Single search strategy

As of May 2026, the default search strategy for the affiliation parameter is a "single search" strategy that outperforms the legacy multisearch strategy in terms of precision and recall while also being faster and more computationally efficient. You can specify the single search strategy by appending `&single_search` to the query, but it is not necessary, since single search is the default.

<Callout icon="📘" theme="info">
  ## Affiliation parameter single search format

  `https://api.ror.org/v2/organizations?affiliation=[URL-encoded-string]&single_search`
</Callout>

The single search matching strategy, <Anchor label="available in the ROR API since December 2025" target="_blank" href="https://doi.org/10.71938/zz90-g810">available in the ROR API since December 2025</Anchor>, uses only a single query and the entirety of the input text string to find potential matches between affiliation strings and ROR records. The strategy is benchmarked against an F.05 score, meaning that it weights precision (accuracy of matches) more than recall (quantity of matches), with 97% precision and 72% recall. The maximum number of results in the response is 10.

## Example

Find a matching ROR record for the string "Instituto de Investigación en Informática de Albacete (I3A), Universidad de Castilla-La Mancha, Albacete, Spain" specifying the single search strategy.

```curl
curl 'https://api.ror.org/organizations?affiliation=Instituto%20de%20Investigaci%C3%B3n%20en%20Inform%C3%A1tica%20de%20Albacete%20%28I3A%29%2C%20Universidad%20de%20Castilla-La%20Mancha%2C%20Albacete%2C%20Spain&single_search' | json_pp 
```

The first item in the results list, the ROR record [https://ror.org/05r78ng12](https://ror.org/05r78ng12) for Universidad de Castilla La Mancha, has a `chosen` value of _true_, indicating that the affiliation service considers this record a sufficiently likely match to the text string. Not all affiliation searches will produce a "chosen" result.

The `matching_type` is given as "SINGLE SEARCH", which will always be the case for queries that use the `&single_search` parameter. The confidence `score` for the match is 1, the highest possible level of confidence in the match. Results are listed in descending order by matching confidence score. Use the `chosen:true` indicator, not the confidence score or position in the list of results, to select matches.

```json
{
   "items" : [
      {
         "chosen" : true,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "uclm.es"
            ],
            "established" : 1982,
            "external_ids" : [
               {
                  "all" : [
                     "501100007480"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.8048.4"
                  ],
                  "preferred" : "grid.8048.4",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 2194 2329"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q941806"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/05r78ng12",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.uclm.es"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/University_of_Castilla%E2%80%93La_Mancha"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CM",
                     "country_subdivision_name" : "Castille-La Mancha",
                     "lat" : 38.98626,
                     "lng" : -3.92907,
                     "name" : "Ciudad Real"
                  },
                  "geonames_id" : 2519402
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "UCLM"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label"
                  ],
                  "value" : "Universidad de Castilla-La Mancha"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "University of Castilla-La Mancha"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/03xw5ev35",
                  "label" : "ORFEO-CINQA Research Network",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/0140hpe71",
                  "label" : "Instituto de Investigación en Recursos Cinegéticos",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04a5hr295",
                  "label" : "Complejo Hospitalario Universitario de Albacete",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/055p2yz63",
                  "label" : "Hospital General Universitario de Albacete",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/02f30ff69",
                  "label" : "Hospital General Universitario de Ciudad Real",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 1,
         "substring" : "Instituto de Investigación en Informática de Albacete (I3A), Universidad de Castilla-La Mancha, Albacete, Spain"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2024-06-19",
                  "schema_version" : "2.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : null,
            "external_ids" : [],
            "id" : "https://ror.org/02e6ysw16",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.uanl.mx/centros_inv/instituto-de-investigaciones-sociales/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "MX",
                     "country_name" : "Mexico",
                     "country_subdivision_code" : "NLE",
                     "country_subdivision_name" : "Nuevo León",
                     "lat" : 25.74167,
                     "lng" : -100.30222,
                     "name" : "San Nicolás de los Garza"
                  },
                  "geonames_id" : 3985241
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IINSO"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Instituto de Investigaciones Sociales"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Instituto de Investigaciones Sociales de la Universidad Autónoma de Nuevo León"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/01fh86n78",
                  "label" : "Universidad Autónoma de Nuevo León",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.86,
         "substring" : "Instituto de Investigación en Informática de Albacete (I3A), Universidad de Castilla-La Mancha, Albacete, Spain"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2025-11-24",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "iin.sld.pe"
            ],
            "established" : 1961,
            "external_ids" : [
               {
                  "all" : [
                     "grid.419080.4"
                  ],
                  "preferred" : "grid.419080.4",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 2236 6140"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q30281971"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/05by4rq81",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.iin.sld.pe/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "SA",
                     "continent_name" : "South America",
                     "country_code" : "PE",
                     "country_name" : "Peru",
                     "country_subdivision_code" : "LMA",
                     "country_subdivision_name" : "Lima Province",
                     "lat" : -12.04318,
                     "lng" : -77.02824,
                     "name" : "Lima"
                  },
                  "geonames_id" : 3936456
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IIN"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Instituto de Investigación Nutricional"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "nonprofit"
            ]
         },
         "score" : 0.86,
         "substring" : "Instituto de Investigación en Informática de Albacete (I3A), Universidad de Castilla-La Mancha, Albacete, Spain"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "ie.edu"
            ],
            "established" : 1997,
            "external_ids" : [
               {
                  "all" : [
                     "grid.45343.35"
                  ],
                  "preferred" : "grid.45343.35",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 1782 8840"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q283501"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/02jjdwm75",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.ie.edu"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/IE_University"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CL",
                     "country_subdivision_name" : "Castille and León",
                     "lat" : 40.94808,
                     "lng" : -4.11839,
                     "name" : "Segovia"
                  },
                  "geonames_id" : 3109256
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "label"
                  ],
                  "value" : "IE Universidad"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "IE University"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Universidad Instituto de Empresa"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/00c5kmy11",
                  "label" : "Fundación IE",
                  "type" : "child"
               }
            ],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.86,
         "substring" : "Instituto de Investigación en Informática de Albacete (I3A), Universidad de Castilla-La Mancha, Albacete, Spain"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1947,
            "external_ids" : [
               {
                  "all" : [
                     "100011766"
                  ],
                  "preferred" : "100011766",
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.201894.6"
                  ],
                  "preferred" : "grid.201894.6",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 0321 4125"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q1509357"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/03tghng59",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.swri.org/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Southwest_Research_Institute"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "TX",
                     "country_subdivision_name" : "Texas",
                     "lat" : 29.42412,
                     "lng" : -98.49363,
                     "name" : "San Antonio"
                  },
                  "geonames_id" : 4726206
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "label"
                  ],
                  "value" : "Instituto de Investigación del Suroeste"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Southwest Research Institute"
               },
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "SwRI"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "funder",
               "nonprofit"
            ]
         },
         "score" : 0.84,
         "substring" : "Instituto de Investigación en Informática de Albacete (I3A), Universidad de Castilla-La Mancha, Albacete, Spain"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2024-07-08",
                  "schema_version" : "2.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "iicg-urjc.es"
            ],
            "established" : 2023,
            "external_ids" : [],
            "id" : "https://ror.org/01m870m49",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://iicg-urjc.es"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "MD",
                     "country_subdivision_name" : "Madrid",
                     "lat" : 40.32234,
                     "lng" : -3.86496,
                     "name" : "Móstoles"
                  },
                  "geonames_id" : 3116025
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Global Change Research Institute"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IICG-URJC"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Instituto de Investigación en Cambio Global"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/01v5cv687",
                  "label" : "Universidad Rey Juan Carlos",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "other"
            ]
         },
         "score" : 0.84,
         "substring" : "Instituto de Investigación en Informática de Albacete (I3A), Universidad de Castilla-La Mancha, Albacete, Spain"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2025-11-21",
                  "schema_version" : "2.1"
               },
               "last_modified" : {
                  "date" : "2025-11-24",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "i3a.es"
            ],
            "established" : 2002,
            "external_ids" : [
               {
                  "all" : [
                     "Q5811553"
                  ],
                  "preferred" : "Q5811553",
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/04eqgej32",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://i3a.es"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://es.wikipedia.org/wiki/Instituto_de_Investigaci%C3%B3n_en_Ingenier%C3%ADa_de_Arag%C3%B3n"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "AR",
                     "country_subdivision_name" : "Aragon",
                     "lat" : 41.65606,
                     "lng" : -0.87734,
                     "name" : "Zaragoza"
                  },
                  "geonames_id" : 3104324
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Aragon Institute for Engineering Research"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "I3A"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Instituto de Investigación en Ingeniería de Aragón"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/012a91z28",
                  "label" : "Universidad de Zaragoza",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.83,
         "substring" : "Instituto de Investigación en Informática de Albacete (I3A), Universidad de Castilla-La Mancha, Albacete, Spain"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2025-06-24",
                  "schema_version" : "2.1"
               },
               "last_modified" : {
                  "date" : "2025-06-24",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : null,
            "external_ids" : [],
            "id" : "https://ror.org/01ebs6k31",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.ungs.edu.ar/ici/ici"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "SA",
                     "continent_name" : "South America",
                     "country_code" : "AR",
                     "country_name" : "Argentina",
                     "country_subdivision_code" : "C",
                     "country_subdivision_name" : "Buenos Aires F.D.",
                     "lat" : -34.61315,
                     "lng" : -58.37723,
                     "name" : "Buenos Aires"
                  },
                  "geonames_id" : 3435910
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "ICI"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Instituto de Ciencias"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Instituto de Ciencias de la Universidad Nacional de General Sarmiento"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/01k6h2m87",
                  "label" : "National University of General Sarmiento",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "facility"
            ]
         },
         "score" : 0.83,
         "substring" : "Instituto de Investigación en Informática de Albacete (I3A), Universidad de Castilla-La Mancha, Albacete, Spain"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2012,
            "external_ids" : [
               {
                  "all" : [
                     "grid.424267.1"
                  ],
                  "preferred" : "grid.424267.1",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 7473 3346"
                  ],
                  "preferred" : "0000 0004 7473 3346",
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q30284793"
                  ],
                  "preferred" : "Q30284793",
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/028z00g40",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.kronikgune.org"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "PV",
                     "country_subdivision_name" : "Basque Country",
                     "lat" : 43.29639,
                     "lng" : -2.98813,
                     "name" : "Barakaldo"
                  },
                  "geonames_id" : 3109453
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Institute for Health Systems Research"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label"
                  ],
                  "value" : "Instituto de Investigación en Sistemas de Salud"
               },
               {
                  "lang" : null,
                  "types" : [
                     "alias"
                  ],
                  "value" : "Kronikgune"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Kronikgune Institute for Health Service Research"
               },
               {
                  "lang" : "eu",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Osasun Sistemen Ikerketa Institutua"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/02g7qcb42",
                  "label" : "Osakidetza",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/025yj9w07",
                  "label" : "Department of Health",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/054hj6489",
                  "label" : "Bioef - Fundación Vasca de Innovación e Investigación Sanitarias",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "other"
            ]
         },
         "score" : 0.83,
         "substring" : "Instituto de Investigación en Informática de Albacete (I3A), Universidad de Castilla-La Mancha, Albacete, Spain"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2019-11-07",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2012,
            "external_ids" : [
               {
                  "all" : [
                     "grid.507088.2"
                  ],
                  "preferred" : "grid.507088.2",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "Q18397927"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/026yy9j15",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.ibsgranada.es"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "AN",
                     "country_subdivision_name" : "Andalusia",
                     "lat" : 37.18817,
                     "lng" : -3.60667,
                     "name" : "Granada"
                  },
                  "geonames_id" : 2517117
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Instituto de Investigación Biosanitaria"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Instituto de Investigación Biosanitaria de Granada"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "ibs.GRANADA"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "facility"
            ]
         },
         "score" : 0.83,
         "substring" : "Instituto de Investigación en Informática de Albacete (I3A), Universidad de Castilla-La Mancha, Albacete, Spain"
      }
   ],
   "number_of_results" : 10
}
```

# Multisearch strategy

The original matching strategy for the ROR API affiliation parameter, in place [since November 2019](https://doi.org/10.71938/36jw-rs79), remains available in the ROR API and can be invoked by appending `&multisearch` to a query.

This strategy breaks long search strings into separate substrings, performing multiple searches with these values and limiting results to records matching any countries that can be derived from the text.  It then returns (if possible) the most likely match to a ROR record, as identified by a `chosen:true` indicator.

Additional candidates also appear in the results list and are ranked in descending order by confidence `score`. Only results with a score of at least 0.5 are returned.

<Callout icon="📘" theme="info">
  ## Affiliation parameter multisearch format

  `https://api.ror.org/v2/organizations?affiliation=[URL-encoded-string]&multisearch`
</Callout>

The matching types used in the multisearch strategy include the following:

* `PHRASE`: a phrase was matched to a variant of the organization's name
* `COMMON TERMS`: the matching was done by comparing the words separately
* `FUZZY`: the matching was done by a fuzzy comparison of the words separately
* `HEURISTICS`: "University of X" was matched to "X University"
* `ACRONYM`: matched by acronym
* `EXACT`: exact match of the entered string in name values in the `names` field excluding acronyms

## Example

Find a matching ROR record for the string "Instituto de Investigación en Informática de Albacete (I3A), Universidad de Castilla-La Mancha, Albacete, Spain" specifying the multisearch strategy.

```curl
curl 'https://api.ror.org/organizations?affiliation=Instituto%20de%20Investigaci%C3%B3n%20en%20Inform%C3%A1tica%20de%20Albacete%20%28I3A%29%2C%20Universidad%20de%20Castilla-La%20Mancha%2C%20Albacete%2C%20Spain&multisearch' | json_pp
```

The first item in the results list, the ROR record [https://ror.org/05r78ng12](https://ror.org/05r78ng12) for Universidad de Castilla La Mancha, has a `chosen` value of _true_, indicating that the affiliation service considers this record a sufficiently likely match to the text string. Not all affiliation searches will produce a "chosen" result.

The `matching_type` is given as _"PHRASE"_, indicating the method by which the affiliation parameter chose the matching record. The confidence `score` is 1, the highest possible level of confidence in the match. Results are listed in descending order by matching confidence score.

The substring used to find the match in this case is "Universidad de Castilla La Mancha."

```json
{
   "items" : [
      {
         "chosen" : true,
         "matching_type" : "PHRASE",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "uclm.es"
            ],
            "established" : 1982,
            "external_ids" : [
               {
                  "all" : [
                     "501100007480"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.8048.4"
                  ],
                  "preferred" : "grid.8048.4",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 2194 2329"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q941806"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/05r78ng12",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.uclm.es"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/University_of_Castilla%E2%80%93La_Mancha"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CM",
                     "country_subdivision_name" : "Castille-La Mancha",
                     "lat" : 38.98626,
                     "lng" : -3.92907,
                     "name" : "Ciudad Real"
                  },
                  "geonames_id" : 2519402
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "UCLM"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label"
                  ],
                  "value" : "Universidad de Castilla-La Mancha"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "University of Castilla-La Mancha"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/03xw5ev35",
                  "label" : "ORFEO-CINQA Research Network",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/0140hpe71",
                  "label" : "Instituto de Investigación en Recursos Cinegéticos",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04a5hr295",
                  "label" : "Complejo Hospitalario Universitario de Albacete",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/055p2yz63",
                  "label" : "Hospital General Universitario de Albacete",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/02f30ff69",
                  "label" : "Hospital General Universitario de Ciudad Real",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 1,
         "substring" : "Universidad de Castilla La Mancha"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2008,
            "external_ids" : [
               {
                  "all" : [
                     "grid.465942.8"
                  ],
                  "preferred" : "grid.465942.8",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 4682 7468"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/055sgt471",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.ui1.es/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Universidad_Isabel_I"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CL",
                     "country_subdivision_name" : "Castille and León",
                     "lat" : 42.34106,
                     "lng" : -3.70184,
                     "name" : "Burgos"
                  },
                  "geonames_id" : 3127461
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Universidad Isabel I"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Universidad Isabel I de Castilla"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.8,
         "substring" : "Universidad de Castilla La Mancha"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2025-11-21",
                  "schema_version" : "2.1"
               },
               "last_modified" : {
                  "date" : "2025-11-24",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "i3a.es"
            ],
            "established" : 2002,
            "external_ids" : [
               {
                  "all" : [
                     "Q5811553"
                  ],
                  "preferred" : "Q5811553",
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/04eqgej32",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://i3a.es"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://es.wikipedia.org/wiki/Instituto_de_Investigaci%C3%B3n_en_Ingenier%C3%ADa_de_Arag%C3%B3n"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "AR",
                     "country_subdivision_name" : "Aragon",
                     "lat" : 41.65606,
                     "lng" : -0.87734,
                     "name" : "Zaragoza"
                  },
                  "geonames_id" : 3104324
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Aragon Institute for Engineering Research"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "I3A"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Instituto de Investigación en Ingeniería de Aragón"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/012a91z28",
                  "label" : "Universidad de Zaragoza",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.77,
         "substring" : "Instituto de Investigación en Informática de Albacete I3A"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : null,
            "external_ids" : [
               {
                  "all" : [
                     "grid.426047.3"
                  ],
                  "preferred" : "grid.426047.3",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 1530 8903"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/03k8zj440",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://sescam.castillalamancha.es/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CM",
                     "country_subdivision_name" : "Castille-La Mancha",
                     "lat" : 39.8581,
                     "lng" : -4.02263,
                     "name" : "Toledo"
                  },
                  "geonames_id" : 2510409
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "SESCAM"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Servicio de Salud de Castilla La Mancha"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/04a5hr295",
                  "label" : "Complejo Hospitalario Universitario de Albacete",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04q4ppz72",
                  "label" : "Complejo Hospitalario Universitario de Toledo",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00k5pj069",
                  "label" : "Hospital General Nuestra Señora del Prado",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02f30ff69",
                  "label" : "Hospital General Universitario de Ciudad Real",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01wc63152",
                  "label" : "Hospital General de Almansa",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01zbsrk34",
                  "label" : "Hospital General de Tomelloso",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04xzgfg07",
                  "label" : "Hospital Nacional de Parapléjicos",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00jkz9152",
                  "label" : "Hospital Universitario de Guadalajara",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00k49k182",
                  "label" : "Hospital Virgen de la Luz",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01gaptw32",
                  "label" : "Hospital de Hellín",
                  "type" : "child"
               }
            ],
            "status" : "active",
            "types" : [
               "healthcare"
            ]
         },
         "score" : 0.72,
         "substring" : "Universidad de Castilla La Mancha"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2019-11-07",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2012,
            "external_ids" : [
               {
                  "all" : [
                     "grid.507088.2"
                  ],
                  "preferred" : "grid.507088.2",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "Q18397927"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/026yy9j15",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.ibsgranada.es"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "AN",
                     "country_subdivision_name" : "Andalusia",
                     "lat" : 37.18817,
                     "lng" : -3.60667,
                     "name" : "Granada"
                  },
                  "geonames_id" : 2517117
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Instituto de Investigación Biosanitaria"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Instituto de Investigación Biosanitaria de Granada"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "ibs.GRANADA"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "facility"
            ]
         },
         "score" : 0.71,
         "substring" : "Instituto de Investigación en Informática de Albacete I3A"
      },
      {
         "chosen" : false,
         "matching_type" : "FUZZY",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : null,
            "external_ids" : [
               {
                  "all" : [
                     "grid.424652.3"
                  ],
                  "preferred" : "grid.424652.3",
                  "type" : "grid"
               }
            ],
            "id" : "https://ror.org/05jdfph84",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://ajuntament.barcelona.cat/imi"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CT",
                     "country_subdivision_name" : "Catalonia",
                     "lat" : 41.38879,
                     "lng" : 2.15899,
                     "name" : "Barcelona"
                  },
                  "geonames_id" : 3128760
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IMI"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Institut Municipal D'Informática de Barcelona"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label"
                  ],
                  "value" : "Instituto Municipal de Informática de Barcelona"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "The Municipal Institute for Information Technology"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "government"
            ]
         },
         "score" : 0.71,
         "substring" : "Instituto de Investigación en Informática de Albacete I3A"
      },
      {
         "chosen" : false,
         "matching_type" : "FUZZY",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2026-03-31",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "udc.gal"
            ],
            "established" : 1989,
            "external_ids" : [
               {
                  "all" : [
                     "501100014597"
                  ],
                  "preferred" : "501100014597",
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.8073.c"
                  ],
                  "preferred" : "grid.8073.c",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 2176 8535"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q280305"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/01qckj285",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.udc.gal/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/University_of_A_Coru%C3%B1a"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "GA",
                     "country_subdivision_name" : "Galicia",
                     "lat" : 43.37135,
                     "lng" : -8.396,
                     "name" : "A Coruña"
                  },
                  "geonames_id" : 3119841
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Universidad de La Coruña"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Universidade da Coruña"
               },
               {
                  "lang" : "ca",
                  "types" : [
                     "label"
                  ],
                  "value" : "Universitat de la Corunya"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "University of A Coruña"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/044knj408",
                  "label" : "Complexo Hospitalario Universitario A Coruña",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/019z56k76",
                  "label" : "Consorcio Interuniversitario do Sistema Universitario de Galicia (CISUG)",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 0.7,
         "substring" : "Universidad de Castilla La Mancha"
      },
      {
         "chosen" : false,
         "matching_type" : "FUZZY",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2023-12-07",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2008,
            "external_ids" : [
               {
                  "all" : [
                     "0000 0004 7590 6907"
                  ],
                  "preferred" : "0000 0004 7590 6907",
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q56685817"
                  ],
                  "preferred" : "Q56685817",
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/01wfb3668",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.ub.edu/irbio"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CT",
                     "country_subdivision_name" : "Catalonia",
                     "lat" : 41.38879,
                     "lng" : 2.15899,
                     "name" : "Barcelona"
                  },
                  "geonames_id" : 3128760
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Biodiversity Research Institute"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Biodiversity Research Institute of the Universiy of Barcelona"
               },
               {
                  "lang" : "ca",
                  "types" : [
                     "alias"
                  ],
                  "value" : "IRBio-UB"
               },
               {
                  "lang" : "ca",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Institut de Recerca de la Biodiversitat"
               },
               {
                  "lang" : "ca",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Institut de Recerca de la Biodiversitat de la Universitat de Barcelona"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Instituto de Investigación de la Biodiversidad"
               },
               {
                  "lang" : "zh",
                  "types" : [
                     "label"
                  ],
                  "value" : "Instituto de Investigación de la Biodiversidad de la Universidad de Barcelona"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/021018s57",
                  "label" : "Universitat de Barcelona",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/01y43zx14",
                  "label" : "Institut de Biomedicina de la Universitat de Barcelona",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/044mj7r89",
                  "label" : "Institut de Biologia Evolutiva",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/05ect0289",
                  "label" : "Institut de Ciències del Mar",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/03abrgd14",
                  "label" : "Centre for Research on Ecology and Forestry Applications",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "facility"
            ]
         },
         "score" : 0.68,
         "substring" : "Instituto de Investigación en Informática de Albacete I3A Universidad de Castilla La Mancha Albacete Spain"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2025-08-06",
                  "schema_version" : "2.1"
               },
               "last_modified" : {
                  "date" : "2025-12-15",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "iriaf.castillalamancha.es"
            ],
            "established" : 2015,
            "external_ids" : [
               {
                  "all" : [
                     "0000 0005 1089 1377"
                  ],
                  "preferred" : "0000 0005 1089 1377",
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/029sr0q57",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://iriaf.castillalamancha.es"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CM",
                     "country_subdivision_name" : "Castille-La Mancha",
                     "lat" : 39.15759,
                     "lng" : -3.02156,
                     "name" : "Tomelloso"
                  },
                  "geonames_id" : 2510392
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IRIAF"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Instituto Regional de Investigación y Desarrollo Agroalimentario y Forestal"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Instituto Regional de Investigación y Desarrollo Agroalimentario y Forestal (IRIAF)"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Instituto Regional de Investigación y Desarrollo Agroalimentario y Forestal de Castilla-La Mancha"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Regional Institute for Agri-Food and Forestry Research and Development"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Regional Institute for Agri-Food and Forestry Research and Development of Castilla-La Mancha"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/02jv91m18",
                  "label" : "Regional Government of Castile-La Mancha",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "facility",
               "government"
            ]
         },
         "score" : 0.67,
         "substring" : "Instituto de Investigación en Informática de Albacete I3A Universidad de Castilla La Mancha Albacete Spain"
      },
      {
         "chosen" : false,
         "matching_type" : "FUZZY",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2020-03-15",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2025-04-28",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "iri.upc.edu"
            ],
            "established" : 1995,
            "external_ids" : [
               {
                  "all" : [
                     "grid.507641.1"
                  ],
                  "preferred" : "grid.507641.1",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 1763 2928"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q3815300"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/00mcdyn90",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.iri.upc.edu"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Institut_de_Rob%C3%B2tica_i_Inform%C3%A0tica_Industrial"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CT",
                     "country_subdivision_name" : "Catalonia",
                     "lat" : 41.38879,
                     "lng" : 2.15899,
                     "name" : "Barcelona"
                  },
                  "geonames_id" : 3128760
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IRII"
               },
               {
                  "lang" : "ca",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Institut de Robòtica i Informàtica Industrial"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Institute of Robotics and Industrial Informatics"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label"
                  ],
                  "value" : "Instituto de Robótica e Informática Industrial"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/02gfc7t72",
                  "label" : "Consejo Superior de Investigaciones Científicas",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/03mb6wj31",
                  "label" : "Universitat Politècnica de Catalunya",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "facility"
            ]
         },
         "score" : 0.66,
         "substring" : "Instituto de Investigación en Informática de Albacete I3A"
      },
      {
         "chosen" : false,
         "matching_type" : "FUZZY",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1967,
            "external_ids" : [
               {
                  "all" : [
                     "grid.470634.2"
                  ],
                  "preferred" : "grid.470634.2",
                  "type" : "grid"
               }
            ],
            "id" : "https://ror.org/02yp1e416",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://castellon.san.gva.es/hospital-general"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "VC",
                     "country_subdivision_name" : "Valencia",
                     "lat" : 39.98567,
                     "lng" : -0.04935,
                     "name" : "Castellon"
                  },
                  "geonames_id" : 2519752
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Hospital General Universitari de Castelló"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "healthcare"
            ]
         },
         "score" : 0.65,
         "substring" : "Universidad de Castilla La Mancha"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2026-02-24",
                  "schema_version" : "2.1"
               },
               "last_modified" : {
                  "date" : "2026-02-24",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "innocam.castillalamancha.es"
            ],
            "established" : 2023,
            "external_ids" : [],
            "id" : "https://ror.org/04cmtj866",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://innocam.castillalamancha.es"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CM",
                     "country_subdivision_name" : "Castille-La Mancha",
                     "lat" : 38.68712,
                     "lng" : -4.10734,
                     "name" : "Puertollano"
                  },
                  "geonames_id" : 2512177
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Agencia de Investigación e Innovación de Castilla-La-Mancha"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "INNOCAM"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "government"
            ]
         },
         "score" : 0.63,
         "substring" : "Instituto de Investigación en Informática de Albacete I3A Universidad de Castilla La Mancha Albacete Spain"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2023-11-21",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : null,
            "external_ids" : [],
            "id" : "https://ror.org/02cp22d42",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.unioviedo.es/IMIB/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "AS",
                     "country_subdivision_name" : "Asturias",
                     "lat" : 43.25,
                     "lng" : -5.76667,
                     "name" : "Mieres"
                  },
                  "geonames_id" : 3116789
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Biodiversity Research Institute"
               },
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IMIB"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Instituto Mixto de Investigación en Biodiversidad"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/02gfc7t72",
                  "label" : "Consejo Superior de Investigaciones Científicas",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/006gksa02",
                  "label" : "Universidad de Oviedo",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/05mj4yh71",
                  "label" : "Gobierno del Principado de Asturias",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "facility"
            ]
         },
         "score" : 0.62,
         "substring" : "Instituto de Investigación en Informática de Albacete I3A"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2025-12-15",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1982,
            "external_ids" : [
               {
                  "all" : [
                     "501100011698"
                  ],
                  "preferred" : "501100011698",
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.454818.4"
                  ],
                  "preferred" : "grid.454818.4",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 2198 1344"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q3113875"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/02jv91m18",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.jccm.es/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Regional_Government_of_Castile-La_Mancha"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CM",
                     "country_subdivision_name" : "Castille-La Mancha",
                     "lat" : 40.62862,
                     "lng" : -3.16185,
                     "name" : "Guadalajara"
                  },
                  "geonames_id" : 3121070
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "label"
                  ],
                  "value" : "Junta de Comunidades de Castilla-La Mancha"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Regional Government of Castile-La Mancha"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/0140hpe71",
                  "label" : "Instituto de Investigación en Recursos Cinegéticos",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/029sr0q57",
                  "label" : "Instituto Regional de Investigación y Desarrollo Agroalimentario y Forestal de Castilla-La Mancha",
                  "type" : "child"
               }
            ],
            "status" : "active",
            "types" : [
               "funder",
               "government"
            ]
         },
         "score" : 0.6,
         "substring" : "Universidad de Castilla La Mancha"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1985,
            "external_ids" : [
               {
                  "all" : [
                     "grid.411839.6"
                  ],
                  "preferred" : "grid.411839.6",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0000 9321 9781"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/04a5hr295",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.chospab.es/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CM",
                     "country_subdivision_name" : "Castille-La Mancha",
                     "lat" : 38.99424,
                     "lng" : -1.85643,
                     "name" : "Albacete"
                  },
                  "geonames_id" : 2522258
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Complejo Hospitalario Universitario de Albacete"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Hospital General Universitario de Albacete"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/055p2yz63",
                  "label" : "Hospital General Universitario de Albacete",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03k8zj440",
                  "label" : "Servicio de Salud de Castilla La Mancha",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/05r78ng12",
                  "label" : "University of Castilla-La Mancha",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "healthcare"
            ]
         },
         "score" : 0.59,
         "substring" : "Instituto de Investigación en Informática de Albacete I3A"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1985,
            "external_ids" : [
               {
                  "all" : [
                     "grid.411094.9"
                  ],
                  "preferred" : "grid.411094.9",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 0506 8127"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/055p2yz63",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.chospab.es/noticiario/TextoNoticia.php?ID=0642&AN=17"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CM",
                     "country_subdivision_name" : "Castille-La Mancha",
                     "lat" : 38.99424,
                     "lng" : -1.85643,
                     "name" : "Albacete"
                  },
                  "geonames_id" : 2522258
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Hospital General Universitario de Albacete"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/04a5hr295",
                  "label" : "Complejo Hospitalario Universitario de Albacete",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/05r78ng12",
                  "label" : "University of Castilla-La Mancha",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "healthcare"
            ]
         },
         "score" : 0.59,
         "substring" : "Instituto de Investigación en Informática de Albacete I3A"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2026-05-04",
                  "schema_version" : "2.1"
               },
               "last_modified" : {
                  "date" : "2026-05-05",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "grupoice.ugr.es"
            ],
            "established" : 2008,
            "external_ids" : [],
            "id" : "https://ror.org/01hn2ae91",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://grupoice.ugr.es"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "AN",
                     "country_subdivision_name" : "Andalusia",
                     "lat" : 37.18817,
                     "lng" : -3.60667,
                     "name" : "Granada"
                  },
                  "geonames_id" : 2517117
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Grupo de Investigación ICE"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Grupo de Investigación en Comunicación Educativa"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "HUM 871"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "ICE"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Investigación en Comunicación Educativa"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/04njjy449",
                  "label" : "Universidad de Granada",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "nonprofit"
            ]
         },
         "score" : 0.55,
         "substring" : "Instituto de Investigación en Informática de Albacete I3A"
      },
      {
         "chosen" : false,
         "matching_type" : "FUZZY",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2021-09-23",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2004,
            "external_ids" : [
               {
                  "all" : [
                     "grid.512895.2"
                  ],
                  "preferred" : "grid.512895.2",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 9296 2302"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/00c77q313",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.redris.es/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "MD",
                     "country_subdivision_name" : "Madrid",
                     "lat" : 40.47353,
                     "lng" : -3.87182,
                     "name" : "Majadahonda"
                  },
                  "geonames_id" : 3117667
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "RIS ISCIII"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Red Española de Investigación del Sida"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Red de Investigación en Sida"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/00ca2c886",
                  "label" : "Instituto de Salud Carlos III",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "facility"
            ]
         },
         "score" : 0.53,
         "substring" : "Instituto de Investigación en Informática de Albacete I3A"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2026-04-14",
                  "schema_version" : "2.1"
               },
               "last_modified" : {
                  "date" : "2026-04-14",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "consorciobucle.es"
            ],
            "established" : 2002,
            "external_ids" : [
               {
                  "all" : [
                     "0000 0004 0447 2048"
                  ],
                  "preferred" : "0000 0004 0447 2048",
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/051jb1k20",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://consorciobucle.es"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CL",
                     "country_subdivision_name" : "Castille and León",
                     "lat" : 42.34106,
                     "lng" : -3.70184,
                     "name" : "Burgos"
                  },
                  "geonames_id" : 3127461
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "BUCLE"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Consorcio de Bibliotecas Universitarias de Castilla y León"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Consorcio de Bibliotecas Universitarias de Castilla y León (BUCLE)"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/049da5t36",
                  "label" : "Universidad de Burgos",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/040r3xq19",
                  "label" : "Universidad de León",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/02f40zc51",
                  "label" : "Universidad de Salamanca",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/01fvbaw18",
                  "label" : "Universidad de Valladolid",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "nonprofit"
            ]
         },
         "score" : 0.53,
         "substring" : "Universidad de Castilla La Mancha"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2011,
            "external_ids" : [
               {
                  "all" : [
                     "grid.454765.1"
                  ],
                  "preferred" : "grid.454765.1",
                  "type" : "grid"
               }
            ],
            "id" : "https://ror.org/00t864161",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.educa.jcyl.es/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CL",
                     "country_subdivision_name" : "Castille and León",
                     "lat" : 41.65518,
                     "lng" : -4.72372,
                     "name" : "Valladolid"
                  },
                  "geonames_id" : 3106672
               }
            ],
            "names" : [
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Consejería de Educación"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Consejería de Educación de la Junta de Castilla y León"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Educacyl"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Portal de Educación de la Junta de Castilla y León"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "government"
            ]
         },
         "score" : 0.51,
         "substring" : "Universidad de Castilla La Mancha"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2025-10-28",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "icscyl.com"
            ],
            "established" : null,
            "external_ids" : [
               {
                  "all" : [
                     "grid.488835.a"
                  ],
                  "preferred" : "grid.488835.a",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 8495 0651"
                  ],
                  "preferred" : "0000 0004 8495 0651",
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/00vn23s59",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.icscyl.com/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "ES",
                     "country_name" : "Spain",
                     "country_subdivision_code" : "CL",
                     "country_subdivision_name" : "Castille and León",
                     "lat" : 41.76401,
                     "lng" : -2.46883,
                     "name" : "Soria"
                  },
                  "geonames_id" : 3108681
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IECSCYL"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Instituto de Estudios de Ciencias de la Salud de Castilla y León"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "nonprofit"
            ]
         },
         "score" : 0.51,
         "substring" : "Instituto de Investigación en Informática de Albacete I3A Universidad de Castilla La Mancha Albacete Spain"
      }
   ],
   "number_of_results" : 21
}
```

# Other ways to match affiliations to ROR records

See our guide on [Matching organization names to ROR IDs](doc:matching) for more details on using the ROR API affiliation parameter and other methods, including third-party machine learning models, to match text strings to ROR IDs.
