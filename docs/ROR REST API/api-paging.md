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
next:
  description: ''
  pages:
    - type: basic
      slug: api-query
      title: Query parameter
---
> 👍 ROR REST API v2
>
> This page documents v2 of the ROR REST API. For v1 documentation of the ROR REST API, see [https://ror.readme.io/v1/docs/api-paging](https://ror.readme.io/v1/docs/api-paging). You can also read more about ROR [API versions](doc:api-versions) and a summary of what's new in [Schema 2.0](doc:schema-v2) and [Schema 2.1](doc:schema-2-1).

> 🚧 Version 1 of the ROR schema and API will be sunset in December 2025
>
> In December 2025, version 1 of the ROR schema and API will be sunset, meaning that ROR API requests with v1 in the path will no longer return a response, v1 files will no longer be included in the ROR data dump, and v1 documentation will no longer be available. Read more in our [changelog](https://ror.readme.io/changelog/2025-07-01-sunset-of-version-1#/).

# About paging

Responses to queries of the ROR API are broken into pages with a maximum of 20 results per page beginning at page 1. If `metadata.number_of_results` is greater than 20, you can retrieve subsequent records by specifying the page number of the result.

> 📘 Paging format
>
> `https://api.ror.org/v2/organizations?page=[page number]`

The maximum number of pages that can be retrieved is 500, which means that the maximum number of ROR records that can be retrieved via the ROR API is 10,000. If you send a request for a page beyond 500, you will receive a 200 status, but also an error response like `{"errors":["page '5144' outside of range 1-500”]}`

To determine how many pages you will need to retrieve in order to obtain your entire result set, check `metadata.number_of_results` and divide by 20. Regardless of which page you are on, `metadata.number_of_results` indicates the total number of results returned by your request.

> ❗️ It is not possible to retrieve all ROR records from the API
>
> The API is best for use cases that involve searching for or retrieving individual records. The maximum number of results that can be retrieved via the API is 10,000, which means that it is currently not possible to retrieve all 100,000+ records from the ROR API. If you need to use the entire ROR dataset in your application, please download the [data dump](doc:data-dump).

## Example

```curl
curl 'https://api.ror.org/v2/organizations?page=4' | json_pp
```

The response is a list of the 20 organization records from the fourth page of results of a request to list all active records in ROR. Counts in `<metadata>` pertain to the entire results list, not the individual page.

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
         "established" : 1978,
         "external_ids" : [
            {
               "all" : [
                  "grid.6276.5"
               ],
               "preferred" : "grid.6276.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0618 8606"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/00b7dkt53",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.crf.it/EN"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "IT",
                  "country_name" : "Italy",
                  "country_subdivision_code" : "21",
                  "country_subdivision_name" : "Piedmont",
                  "lat" : 45.00547,
                  "lng" : 7.53813,
                  "name" : "Orbassano"
               },
               "geonames_id" : 3171986
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CRF"
            },
            {
               "lang" : "it",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Centro Ricerche FIAT"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/005m6rn77",
               "label" : "Fiat Chrysler Automobiles (Italy)",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "facility"
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
                  "grid.6287.b"
               ],
               "preferred" : "grid.6287.b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 6009 976X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30252678"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00qt5ka73",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.arttic.eu/pages/en/home.php"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "IDF",
                  "country_subdivision_name" : "Île-de-France",
                  "lat" : 48.85341,
                  "lng" : 2.3488,
                  "name" : "Paris"
               },
               "geonames_id" : 2988507
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Arttic (France)"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "company"
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
         "established" : 1957,
         "external_ids" : [
            {
               "all" : [
                  "grid.6467.3"
               ],
               "preferred" : "grid.6467.3",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/035gy9016",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.tecnatom.es/en/"
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
                  "lat" : 40.55555,
                  "lng" : -3.62733,
                  "name" : "San Sebastián de los Reyes"
               },
               "geonames_id" : 3110040
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Tecnatom (Spain)"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "company"
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
         "established" : 1985,
         "external_ids" : [
            {
               "all" : [
                  "grid.6484.e"
               ],
               "preferred" : "grid.6484.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30252785"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04vfyfx73",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.teercoatings.co.uk/"
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
                  "lat" : 52.26667,
                  "lng" : -2.15,
                  "name" : "Droitwich"
               },
               "geonames_id" : 2650983
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "TCL"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Teer Coatings (United Kingdom)"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "company"
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
         "established" : 1981,
         "external_ids" : [
            {
               "all" : [
                  "grid.6496.d"
               ],
               "preferred" : "grid.6496.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1763 8481"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30252786"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/033vryh36",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.tekniker.es/es"
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
                  "lat" : 43.18493,
                  "lng" : -2.47158,
                  "name" : "Eibar"
               },
               "geonames_id" : 3123709
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Tekniker"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "nonprofit"
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
         "established" : 1900,
         "external_ids" : [
            {
               "all" : [
                  "501100000855"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.6572.6"
               ],
               "preferred" : "grid.6572.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1936 7486"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q223429"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03angcq70",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.birmingham.ac.uk/index.aspx"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/University_of_Birmingham"
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
                  "lat" : 52.48142,
                  "lng" : -1.89983,
                  "name" : "Birmingham"
               },
               "geonames_id" : 2655603
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Birmingham University"
            },
            {
               "lang" : "cy",
               "types" : [
                  "label"
               ],
               "value" : "Prifysgol Birmingham"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Birmingham"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03qxptw71",
               "label" : "Cancer Research UK Clinical Trials Unit",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05a9avq33",
               "label" : "Centre for Printing History and Culture",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05c24ew78",
               "label" : "Walsall College",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/017k80q27",
               "label" : "Birmingham Children's Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/02smq5q54",
               "label" : "Birmingham City Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01xhmje49",
               "label" : "Birmingham Dental Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/015hfw664",
               "label" : "Good Hope Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00k5pte35",
               "label" : "NIHR Birmingham Liver Biomedical Research Unit",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05w3e4z48",
               "label" : "New Cross Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/048emj907",
               "label" : "Queen Elizabeth Hospital Birmingham",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/041jyhr41",
               "label" : "Selly Oak Hospital",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "education",
            "funder"
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
         "established" : 1999,
         "external_ids" : [
            {
               "all" : [
                  "grid.6606.6"
               ],
               "preferred" : "grid.6606.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q29576195"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/042e7c302",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.multitel.be/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "BE",
                  "country_name" : "Belgium",
                  "country_subdivision_code" : "WAL",
                  "country_subdivision_name" : "Wallonia",
                  "lat" : 50.45413,
                  "lng" : 3.95229,
                  "name" : "Mons"
               },
               "geonames_id" : 2790869
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Multitel"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "facility"
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
         "established" : 1842,
         "external_ids" : [
            {
               "all" : [
                  "501100005895"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.6615.4"
               ],
               "preferred" : "grid.6615.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2364 3824"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q137910"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02vbxt202",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.thyssenkrupp.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/ThyssenKrupp"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "NW",
                  "country_subdivision_name" : "North Rhine-Westphalia",
                  "lat" : 51.45657,
                  "lng" : 7.01228,
                  "name" : "Essen"
               },
               "geonames_id" : 2928810
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "ThyssenKrupp (Germany)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00akjja53",
               "label" : "Atlas Elektronik (Germany)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04y0ypp13",
               "label" : "ThyssenKrupp (Brazil)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0034dyb91",
               "label" : "ThyssenKrupp (China)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01zbqc597",
               "label" : "ThyssenKrupp (Italy)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/026kmj910",
               "label" : "ThyssenKrupp (Liechtenstein)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03w3b4f85",
               "label" : "ThyssenKrupp (United Kingdom)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01hcney72",
               "label" : "Thyssenkrupp (Slovakia)",
               "type" : "child"
            }
         ],
         "status" : "active",
         "types" : [
            "company",
            "funder"
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
         "established" : 1957,
         "external_ids" : [
            {
               "all" : [
                  "grid.6625.7"
               ],
               "preferred" : "grid.6625.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0623 4115"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q29123664"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01c74sd89",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.st.com/content/st_com/en.html"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/STMicroelectronicsce"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "IDF",
                  "country_subdivision_name" : "Île-de-France",
                  "lat" : 48.8162,
                  "lng" : 2.31393,
                  "name" : "Montrouge"
               },
               "geonames_id" : 2992017
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ST"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "STMicroelectronics (France)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00wm3b005",
               "label" : "STMicroelectronics (Switzerland)",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "company"
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
         "established" : 1952,
         "external_ids" : [
            {
               "all" : [
                  "grid.6933.f"
               ],
               "preferred" : "grid.6933.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2369 3573"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/02q8h2f47",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.fcba.fr/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "IDF",
                  "country_subdivision_name" : "Île-de-France",
                  "lat" : 48.85341,
                  "lng" : 2.3488,
                  "name" : "Paris"
               },
               "geonames_id" : 2988507
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "FCBA"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Institut Technologique FCBA"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut Technologique Forêt Cellulose Bois-Construction Ameublement"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "facility"
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
         "established" : 1945,
         "external_ids" : [
            {
               "all" : [
                  "grid.6975.d"
               ],
               "preferred" : "grid.6975.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0410 5926"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q21015571"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/030wyr187",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.ttl.fi/en/Pages/default.aspx"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Finnish_Institute_of_Occupational_Health"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FI",
                  "country_name" : "Finland",
                  "country_subdivision_code" : "18",
                  "country_subdivision_name" : "Uusimaa",
                  "lat" : 60.16952,
                  "lng" : 24.93545,
                  "name" : "Helsinki"
               },
               "geonames_id" : 658225
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "FIOH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Finnish Institute of Occupational Health"
            },
            {
               "lang" : "fi",
               "types" : [
                  "alias"
               ],
               "value" : "Työterveyslaitos"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "facility"
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
         "established" : 1998,
         "external_ids" : [
            {
               "all" : [
                  "501100001588"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.7088.7"
               ],
               "preferred" : "grid.7088.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1759 4183"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5380326"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/023z51242",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.enterprise-ireland.com/en/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Enterprise_Ireland"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "IE",
                  "country_name" : "Ireland",
                  "country_subdivision_code" : "L",
                  "country_subdivision_name" : "Leinster",
                  "lat" : 53.33306,
                  "lng" : -6.24889,
                  "name" : "Dublin"
               },
               "geonames_id" : 2964574
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Enterprise Ireland"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
            "government"
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
         "established" : 1970,
         "external_ids" : [
            {
               "all" : [
                  "grid.7151.2"
               ],
               "preferred" : "grid.7151.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0170 2635"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5088071"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0261g6j35",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.hau.ernet.in/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Chaudhary_Charan_Singh_Haryana_Agricultural_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "IN",
                  "country_name" : "India",
                  "country_subdivision_code" : "HR",
                  "country_subdivision_name" : "Haryana",
                  "lat" : 29.15394,
                  "lng" : 75.72294,
                  "name" : "Hisar"
               },
               "geonames_id" : 1270022
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CCS HAU"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Chaudhary Charan Singh Haryana Agricultural University"
            },
            {
               "lang" : "hi",
               "types" : [
                  "label"
               ],
               "value" : "चौधरी चरण सिंह हरियाणा कृषि विश्वविद्यालय"
            },
            {
               "lang" : "ta",
               "types" : [
                  "label"
               ],
               "value" : "சௌதரி சரண் சிங் அரியானா வேளாண்மை பல்கலைக்கழகம்"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education"
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
         "established" : 1974,
         "external_ids" : [
            {
               "all" : [
                  "501100004173"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.7220.7"
               ],
               "preferred" : "grid.7220.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2157 0393"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q972553",
                  "Q6156345",
                  "Q7863746",
                  "Q7863745"
               ],
               "preferred" : "Q972553",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02kta5139",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.uam.mx/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Universidad_Aut%C3%B3noma_Metropolitana"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "MX",
                  "country_name" : "Mexico",
                  "country_subdivision_code" : "CMX",
                  "country_subdivision_name" : "Mexico City",
                  "lat" : 19.42847,
                  "lng" : -99.12766,
                  "name" : "Mexico City"
               },
               "geonames_id" : 3530597
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Metropolitan Autonomous University"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UAM"
            },
            {
               "lang" : "es",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Universidad Autónoma Metropolitana"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education",
            "funder"
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
                  "501100002352",
                  "501100002374"
               ],
               "preferred" : "501100002352",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.7269.a"
               ],
               "preferred" : "grid.7269.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0621 1570"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q2723670"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00cb9w016",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.asu.edu.eg/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Ain_Shams_University"
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
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ASU"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ain Shams University"
            },
            {
               "lang" : "ar",
               "types" : [
                  "label"
               ],
               "value" : "جامعة عين شمس"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00p59qs14",
               "label" : "Ain Shams University Hospital",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "education",
            "funder"
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
         "established" : 2004,
         "external_ids" : [
            {
               "all" : [
                  "grid.7280.d"
               ],
               "preferred" : "grid.7280.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0561 4976"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/01jm19r53",
         "links" : [
            {
               "type" : "website",
               "value" : "http://tutech.net/index.php/page/about-us-2011-03-04#1"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "HH",
                  "country_subdivision_name" : "Hamburg",
                  "lat" : 53.55073,
                  "lng" : 9.99302,
                  "name" : "Hamburg"
               },
               "geonames_id" : 2911298
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "TuTech"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "TuTech Innovation (Germany)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/013e7et93",
               "label" : "Leipziger Institut für Energie",
               "type" : "child"
            }
         ],
         "status" : "active",
         "types" : [
            "company"
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
         "established" : 1970,
         "external_ids" : [
            {
               "all" : [
                  "grid.7307.3"
               ],
               "preferred" : "grid.7307.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2108 9006"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q616905"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03p14d497",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.uni-augsburg.de/en/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/University_of_Augsburg"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BY",
                  "country_subdivision_name" : "Bavaria",
                  "lat" : 48.37154,
                  "lng" : 10.89851,
                  "name" : "Augsburg"
               },
               "geonames_id" : 2954172
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Augsburg"
            },
            {
               "lang" : "de",
               "types" : [
                  "label"
               ],
               "value" : "Universität Augsburg"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03b0k9c14",
               "label" : "University Hospital Augsburg",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "education"
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
         "established" : 1940,
         "external_ids" : [
            {
               "all" : [
                  "grid.7320.6"
               ],
               "preferred" : "grid.7320.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0606 8858"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30252789"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/006rhxn50",
         "links" : [
            {
               "type" : "website",
               "value" : "http://forcetechnology.com/en"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DK",
                  "country_name" : "Denmark",
                  "country_subdivision_code" : "84",
                  "country_subdivision_name" : "Capital Region",
                  "lat" : 55.6455,
                  "lng" : 12.41008,
                  "name" : "Brøndby"
               },
               "geonames_id" : 2623340
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "FORCE Technology (Denmark)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03whc3s66",
               "label" : "Delta (Denmark)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04rccsv84",
               "label" : "FORCE Technology (Norway)",
               "type" : "child"
            }
         ],
         "status" : "active",
         "types" : [
            "company"
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
         "established" : 1934,
         "external_ids" : [
            {
               "all" : [
                  "grid.7324.2"
               ],
               "preferred" : "grid.7324.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0643 3659"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q128929"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00bxsm637",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.mtu.de/?fc=1"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/MTU_Aero_Engines"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BY",
                  "country_subdivision_name" : "Bavaria",
                  "lat" : 48.13743,
                  "lng" : 11.57549,
                  "name" : "Munich"
               },
               "geonames_id" : 2867714
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "MTU Aero Engines (Germany)"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "MTU München"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "company"
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
         "established" : 1949,
         "external_ids" : [
            {
               "all" : [
                  "grid.7336.1"
               ],
               "preferred" : "grid.7336.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0203 5854"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q606815"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03y5egs41",
         "links" : [
            {
               "type" : "website",
               "value" : "http://englishweb.uni-pannon.hu/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/University_of_Pannonia"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "HU",
                  "country_name" : "Hungary",
                  "country_subdivision_code" : "VE",
                  "country_subdivision_name" : "Veszprém",
                  "lat" : 47.09327,
                  "lng" : 17.91149,
                  "name" : "Veszprém"
               },
               "geonames_id" : 3042929
            }
         ],
         "names" : [
            {
               "lang" : "hu",
               "types" : [
                  "label"
               ],
               "value" : "Pannon Egyetem"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Pannonia"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "University of Veszprém"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 43168,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 36887,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 20748,
            "id" : "as",
            "title" : "Asia"
         },
         {
            "count" : 3640,
            "id" : "af",
            "title" : "Africa"
         },
         {
            "count" : 3446,
            "id" : "sa",
            "title" : "South America"
         },
         {
            "count" : 1917,
            "id" : "oc",
            "title" : "Oceania"
         },
         {
            "count" : 2,
            "id" : "an",
            "title" : "Antarctica"
         }
      ],
      "countries" : [
         {
            "count" : 31919,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 7546,
            "id" : "gb",
            "title" : "United Kingdom"
         },
         {
            "count" : 5299,
            "id" : "de",
            "title" : "Germany"
         },
         {
            "count" : 4919,
            "id" : "cn",
            "title" : "China"
         },
         {
            "count" : 4799,
            "id" : "fr",
            "title" : "France"
         },
         {
            "count" : 4007,
            "id" : "jp",
            "title" : "Japan"
         },
         {
            "count" : 3557,
            "id" : "ca",
            "title" : "Canada"
         },
         {
            "count" : 3208,
            "id" : "in",
            "title" : "India"
         },
         {
            "count" : 2804,
            "id" : "cz",
            "title" : "Czech Republic"
         },
         {
            "count" : 2137,
            "id" : "it",
            "title" : "Italy"
         }
      ],
      "statuses" : [
         {
            "count" : 109806,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 30199,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 21507,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 16948,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 14648,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 13376,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 11437,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 8643,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 7122,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 3024,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 109806,
   "time_taken" : 20
}
```
