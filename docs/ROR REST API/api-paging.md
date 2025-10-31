---
title: Paging
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR API paging
  description: >-
    Instructions for paging through lists of ROR records retrieved from the ROR
    API.
  robots: index
---
> 👍 ROR REST API v2
>
> This page documents v2 of the ROR REST API. For v1 documentation of the ROR REST API, see [https://ror.readme.io/v1/docs/api-paging](https://ror.readme.io/v1/docs/api-paging). You can also read more about ROR [API versions](doc:api-versions) and a summary of what's new in [Schema 2.0](doc:schema-v2) and [Schema 2.1](doc:schema-2-1).

> 🚧 Version 1 of the ROR schema and API will be sunset in December 2025
>
> In December 2025, version 1 of the ROR schema and API will be sunset, meaning that ROR API requests with v1 in the path will no longer return a response, v1 files will no longer be included in the ROR data dump, and v1 documentation will no longer be available. Read more in our [changelog](https://ror.readme.io/changelog/2025-07-01-sunset-of-version-1#/).

# About paging

Responses to queries of the ROR API are broken into pages with a maximum of 20 results per page beginning at page 1. If `metadata.number_of_results` is greater than 20, you can retrieve subsequent records by specifying the page number of the result. You can page through results of both standard [queries](doc:api-query) and [advanced queries](doc:api-advanced-query).

> 📘 Paging format
>
> `https://api.ror.org/v2/organizations?query=[query]&page=[page number]`

The maximum number of pages that can be retrieved is 500, which means that the maximum number of ROR records that can be retrieved via the ROR API is 10,000. If you send a request for a page beyond 500, you will receive a 200 status, but also an error response like `{"errors":["page '5144' outside of range 1-500”]}`

To determine how many pages you will need to retrieve in order to obtain your entire result set, check `metadata.number_of_results` and divide by 20. Regardless of which page you are on, `metadata.number_of_results` indicates the total number of results returned by your request.

> ❗️ It is not possible to retrieve all ROR records from the API
>
> The API is best for use cases that involve searching for or retrieving individual records. The maximum number of results that can be retrieved via the API is 10,000, which means that it is currently not possible to retrieve all 100,000+ records from the ROR API. Moreover, attempting to page through all ROR records can result in <Anchor label="duplicates and omissions across different pages of results" target="_blank" href="https://github.com/ror-community/ror-roadmap/issues/333">duplicates and omissions across different pages of results</Anchor>. If you need to use the entire ROR dataset in your application, please download the [data dump](doc:data-dump). 

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query=Hospital&page=2' | json_pp
```

The response is a list of the 20 organization records from the second page of results of a request to list all active records in ROR with the term "Hospital" in a name field. Counts in `<metadata>` pertain to the entire results list, not the individual page.

```json
{
   "items" : [
      {
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
                  "grid.439492.1"
               ],
               "preferred" : "grid.439492.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0486 5909"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30292908"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02se1sp19",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.nhs.uk/Services/hospitals/Overview/DefaultView.aspx?id=2905"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Harplands_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 53.00415,
                  "lng" : -2.18538,
                  "name" : "Stoke-on-Trent"
               },
               "geonames_id" : 2636841
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harplands Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02d5d0r05",
               "label" : "North Staffordshire Combined Healthcare NHS Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
         "established" : 2005,
         "external_ids" : [
            {
               "all" : [
                  "grid.439502.9"
               ],
               "preferred" : "grid.439502.9",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0400 3460"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30292918"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/046rmzh87",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.qegateshead.nhs.uk/benshamhospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 54.96209,
                  "lng" : -1.60168,
                  "name" : "Gateshead"
               },
               "geonames_id" : 2648773
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bensham Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01aye5y64",
               "label" : "Gateshead Health NHS Foundation Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
                  "grid.439506.d"
               ],
               "preferred" : "grid.439506.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0578 7850"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30292922"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03caevp73",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.renacreshospital.co.uk/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 53.56685,
                  "lng" : -2.88178,
                  "name" : "Ormskirk"
               },
               "geonames_id" : 2640908
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Renacres Hospital"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
         "established" : 1922,
         "external_ids" : [
            {
               "all" : [
                  "grid.439508.3"
               ],
               "preferred" : "grid.439508.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0398 6581"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30292924"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04f4nzq23",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.dbth.nhs.uk/locations/retford-hospital/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 53.32213,
                  "lng" : -0.94315,
                  "name" : "East Retford"
               },
               "geonames_id" : 2650346
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Retford Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01yc93g67",
               "label" : "Doncaster and Bassetlaw Teaching Hospitals NHS Foundation Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
         "established" : 2010,
         "external_ids" : [
            {
               "all" : [
                  "grid.439515.f"
               ],
               "preferred" : "grid.439515.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0490 5923"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q4894895"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03zbds496",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.nhft.nhs.uk/berrywood/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Berrywood_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 52.25,
                  "lng" : -0.88333,
                  "name" : "Northampton"
               },
               "geonames_id" : 2641430
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Berrywood Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0358tcd02",
               "label" : "Northamptonshire Healthcare NHS Foundation Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
         "established" : 1933,
         "external_ids" : [
            {
               "all" : [
                  "grid.439516.c"
               ],
               "preferred" : "grid.439516.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0502 5098"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30292928"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0279qd249",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.esht.nhs.uk/hospitals-and-community/bexhill-hospital/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bexhill_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 50.85023,
                  "lng" : 0.47095,
                  "name" : "Bexhill"
               },
               "geonames_id" : 2655777
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bexhill Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04tvjvp97",
               "label" : "East Sussex Healthcare NHS Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
                  "grid.439517.d"
               ],
               "preferred" : "grid.439517.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30292929"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/032kn8622",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.norfolkcommunityhealthandcare.nhs.uk/The-care-we-offer/Service-search/colman-hospital/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 52.62783,
                  "lng" : 1.29834,
                  "name" : "Norwich"
               },
               "geonames_id" : 2641181
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Colman Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00xkkpn05",
               "label" : "Norfolk Community Health and Care NHS Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
         "established" : 1893,
         "external_ids" : [
            {
               "all" : [
                  "grid.439519.3"
               ],
               "preferred" : "grid.439519.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0399 9817"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5169736"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00f147v33",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.nhs.uk/Services/hospitals/Overview/DefaultView.aspx?id=RNA04"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Corbett_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 52.45608,
                  "lng" : -2.14317,
                  "name" : "Stourbridge"
               },
               "geonames_id" : 2636769
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Corbett Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/014hmqv77",
               "label" : "Dudley Group NHS Foundation Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
         "established" : 2002,
         "external_ids" : [
            {
               "all" : [
                  "grid.439520.9"
               ],
               "preferred" : "grid.439520.9",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0579 513X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30292931"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00ekntx28",
         "links" : [],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 54.33901,
                  "lng" : -1.43243,
                  "name" : "Northallerton"
               },
               "geonames_id" : 2641435
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Rutson Hospital"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
         "established" : 1907,
         "external_ids" : [
            {
               "all" : [
                  "grid.439524.d"
               ],
               "preferred" : "grid.439524.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0417 1253"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5174706"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04e0p7290",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.nbt.nhs.uk/our-hospitals/cossham-hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 51.45523,
                  "lng" : -2.59665,
                  "name" : "Bristol"
               },
               "geonames_id" : 2654675
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cossham Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/036x6gt55",
               "label" : "North Bristol NHS Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
         "established" : 1983,
         "external_ids" : [
            {
               "all" : [
                  "grid.439527.e"
               ],
               "preferred" : "grid.439527.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0648 9687"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7596631"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02xphqm22",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.uhnm.nhs.uk/OurServices/Maternity/Pages/County-Hospital-service.aspx"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 52.80521,
                  "lng" : -2.11636,
                  "name" : "Stafford"
               },
               "geonames_id" : 2637142
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "County Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03g47g866",
               "label" : "University Hospitals of North Midlands NHS Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
                  "grid.439532.a"
               ],
               "preferred" : "grid.439532.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1756 6342"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5182983"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02vh6gg23",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.surreyandsussex.nhs.uk/crawley-hospital/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Crawley_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 51.11303,
                  "lng" : -0.18312,
                  "name" : "Crawley"
               },
               "geonames_id" : 2652053
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Crawley Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0480vrj36",
               "label" : "Surrey and Sussex Healthcare NHS Trust",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/04e4sh030",
               "label" : "Sussex Community NHS Foundation Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
         "established" : 1932,
         "external_ids" : [
            {
               "all" : [
                  "grid.439538.0"
               ],
               "preferred" : "grid.439538.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q5187593"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0457fah73",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.nnuh.nhs.uk/our-services/our-hospitals/cromer-and-district-hospital/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Cromer_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 52.93123,
                  "lng" : 1.29892,
                  "name" : "Cromer"
               },
               "geonames_id" : 2651930
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cromer Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01wspv808",
               "label" : "Norfolk and Norwich University Hospitals NHS Foundation Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
         "established" : 2006,
         "external_ids" : [
            {
               "all" : [
                  "grid.439549.6"
               ],
               "preferred" : "grid.439549.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30292950"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/003dyzj34",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.nhft.nhs.uk/danetre/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 52.25688,
                  "lng" : -1.16066,
                  "name" : "Daventry"
               },
               "geonames_id" : 2651485
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Danetre Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0358tcd02",
               "label" : "Northamptonshire Healthcare NHS Foundation Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
         "established" : 2006,
         "external_ids" : [
            {
               "all" : [
                  "grid.439550.e"
               ],
               "preferred" : "grid.439550.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30292951"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/024mswg55",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.blakelandshospital.co.uk/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 52.04172,
                  "lng" : -0.75583,
                  "name" : "Milton Keynes"
               },
               "geonames_id" : 2642465
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Blakelands Hospital"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
                  "grid.439554.a"
               ],
               "preferred" : "grid.439554.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30292954"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02j3qj605",
         "links" : [],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 50.3522,
                  "lng" : -3.5794,
                  "name" : "Dartmouth"
               },
               "geonames_id" : 2651498
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Dartmouth Hospital"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
                  "grid.439555.b"
               ],
               "preferred" : "grid.439555.b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0497 2181"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30292955"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03jjx5w61",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.merseycare.nhs.uk/our-services/our-sites/rathbone-hospital/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 53.41058,
                  "lng" : -2.97794,
                  "name" : "Liverpool"
               },
               "geonames_id" : 2644210
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Rathbone Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vtyzs10",
               "label" : "Mersey Care NHS Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
         "established" : 2002,
         "external_ids" : [
            {
               "all" : [
                  "grid.439561.c"
               ],
               "preferred" : "grid.439561.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0520 7609"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q4936671"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02q2vkv24",
         "links" : [
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bodmin_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 50.47151,
                  "lng" : -4.7243,
                  "name" : "Bodmin"
               },
               "geonames_id" : 2655273
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bodmin Community Hospital"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bodmin Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0517ad239",
               "label" : "Cornwall Partnership NHS Foundation Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
                  "grid.439565.8"
               ],
               "preferred" : "grid.439565.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 7669 1012"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30292963"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/049q27a79",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.norfolkcommunityhealthandcare.nhs.uk/The-care-we-offer/Service-search/dereham-hospital/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 52.68333,
                  "lng" : 0.93333,
                  "name" : "Dereham"
               },
               "geonames_id" : 2650470
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Dereham Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00xkkpn05",
               "label" : "Norfolk Community Health and Care NHS Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
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
                  "grid.439573.f"
               ],
               "preferred" : "grid.439573.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0447 0915"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30292968"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05w3scj59",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.nhs.uk/Services/hospitals/Overview/DefaultView.aspx?id=RT133"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 52.55131,
                  "lng" : 0.08828,
                  "name" : "March"
               },
               "geonames_id" : 2643071
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Doddington Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/040ch0e11",
               "label" : "Cambridgeshire and Peterborough NHS Foundation Trust",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 3344,
            "id" : "as",
            "title" : "Asia"
         },
         {
            "count" : 1991,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 1475,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 274,
            "id" : "af",
            "title" : "Africa"
         },
         {
            "count" : 236,
            "id" : "sa",
            "title" : "South America"
         },
         {
            "count" : 186,
            "id" : "oc",
            "title" : "Oceania"
         }
      ],
      "countries" : [
         {
            "count" : 1259,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 1040,
            "id" : "jp",
            "title" : "Japan"
         },
         {
            "count" : 995,
            "id" : "gb",
            "title" : "United Kingdom"
         },
         {
            "count" : 980,
            "id" : "cn",
            "title" : "China"
         },
         {
            "count" : 464,
            "id" : "in",
            "title" : "India"
         },
         {
            "count" : 220,
            "id" : "es",
            "title" : "Spain"
         },
         {
            "count" : 213,
            "id" : "kr",
            "title" : "South Korea"
         },
         {
            "count" : 159,
            "id" : "au",
            "title" : "Australia"
         },
         {
            "count" : 152,
            "id" : "ca",
            "title" : "Canada"
         },
         {
            "count" : 111,
            "id" : "br",
            "title" : "Brazil"
         }
      ],
      "statuses" : [
         {
            "count" : 7506,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 7245,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 358,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 154,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 51,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 20,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 15,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 11,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 9,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 2,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 7506,
   "time_taken" : 10
}
```
