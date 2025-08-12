---
title: Match organization names to ROR IDs
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    This document provides guidance on matching organization names to ROR IDs
    using the ROR API, including different approaches like the affiliation
    parameter and query parameter methods. It also mentions using scripts, the
    ROR data dump, OpenRefine, and third-party tools for this purpose.
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: openrefine-reconciler
      title: OpenRefine reconciler
    - type: link
      title: 'Video: Strategies for Matching Affiliation Strings to ROR IDs'
      url: https://youtu.be/Tx5y7lX030U
    - type: link
      title: 'Video: Smart matching enabled by the ROR API for the OA Switchboard'
      url: https://www.youtube.com/watch?v=MzfeUJwtDBg
    - type: link
      title: ror-utilities matching and mapping scripts
      url: https://github.com/ror-community/ror-utilities
---
If you have organization names or full affiliation strings stored in your system as text, there are several approaches to matching those text strings to ROR IDs. 

The approaches given below are for cases when you have a list, a spreadsheet, or a database with organization information as text strings, often with "extra" text such as a department or address as well as the organization's name, but no corresponding organization IDs. 

Examples of organization names and affiliation information as text strings and the corresponding ROR ID: 

* University of Haifa > [https://ror.org/02f009v59](https://ror.org/02f009v59)  
* University of ManchesterGreater Manchester Mental Health NHS Foundation TrustNational Institute for Health Research (NIHR) Greater Manchester Patient Safety Translational Research Centre > [https://ror.org/05sb89p83](https://ror.org/05sb89p83) .
* Atmospheric Sciences and Global Change Division, Pacific Northwest National Laboratory, Richland, WA, USA > [https://ror.org/05h992307](https://ror.org/05h992307) 

> 📘 Mapping names and IDs to ROR
>
> If you have organization names along with another global organization ID (Crossref Funder ID, GRID, ISNI, or Wikidata), you may find quicker, more accurate results by mapping those other IDs to ROR. See [Map other organization ID types to ROR](doc:mapping). 
>
> If you need to match ROR IDs to user input in a web application, see [Create ROR-powered typeaheads in forms](doc:forms).

## Match organization names to ROR IDs using the ROR API

### Query vs affiliation parameter

The [ROR API](doc:api-about) offers 2 ways to search ROR records, which work slightly differently and return different results:

* **[Affiliation parameter](#affiliation-parameter-approach)**  `/organizations?affiliation=`:  Searches `names` field using different search algorithms and returns the best match(es) based on matching score. Includes matching score and true/false indicator of whether the score is high enough to be considered a reliable match. Produces about 85% correct matches in our tests; individual implementations may see better or worse results. 

* **[Query parameter](#query-parameter-approach)** `/organizations?query=`: Searches `names` and `external_ids` fields in ROR records and returns all matching records. Does NOT include a matching score or true/false indicator of whether the score is high enough to be considered a reliable match. Will return the same results as the [Web search](doc:web-search).

For cases where you have relatively unique organization names or full affiliation strings, use the [affiliation parameter approach](#affiliation-parameter-approach). Works for both English and non-English name variations. Examples:

* Incorporated Research Institutions for Seismology (IRIS)
* Universitätsbibliothek der Ludwig-Maximilians-Universität München
* Department of Civil and Industrial Engineering, University of Pisa, Largo Lucio Lazzarino 2, Pisa 56126, Italy

For cases where you have very common organization names, use the [query parameter approach](#query-parameter-approach) to look for keywords from the organization's name or the exact name of the organization surrounded by double quotation marks, and consider using filters for organization type and country. Examples:

* Ministry of Health
* National Research Council
* York University

> 📘 Retrieving active and inactive organizations
>
> By default, both the affiliation parameter and the query parameter will return only records with an active [status](doc:data-structure#status): `status: "active"`. Consider whether you also want to retrieve records with an inactive status; inactive records generally represent organizations that no longer operate. See [API filtering](doc:api-filtering) for details. 
>
> Be aware too that inactive organizations may be succeeded by a new organization under a different name with a different ROR ID. If you do retrieve inactive organizations, check the `relationships` field of an inactive record to see if it has a [Successor organization](doc:data-structure#relationships).

### Affiliation parameter approach

Searches the `names` field using different search algorithms and returns the best match(es) based on matching score using the format `https://api.ror.org/v2/organizations?affiliation=<URL-encoded string>`. For the full list of fields returned, see [Fields and sub-fields](doc:fields).

```curl
curl 'https://api.ror.org/v2/organizations?affiliation=university+of+wisconsin+madison' | json_pp
```

If there's a high-confidence match, it will appear as the first result, with `"chosen": true`.  Other results and their matching score will also be returned, but if you're automating this process using a script, we recommend using results with `"chosen": true` rather than relying on the score. Results are paginated: see [API paging](doc:api-paging) for details.

```json
{
   "items" : [
      {
         "chosen" : true,
         "matching_type" : "EXACT",
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
            "established" : 1848,
            "external_ids" : [
               {
                  "all" : [
                     "100007015",
                     "100008959",
                     "100005996",
                     "100007870",
                     "100008301",
                     "100008028",
                     "100008237",
                     "100008161",
                     "100010495",
                     "100009627",
                     "100010284",
                     "100005911",
                     "100007925",
                     "100005902",
                     "100012787"
                  ],
                  "preferred" : "100007015",
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.14003.36"
                  ],
                  "preferred" : "grid.14003.36",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 2167 3675"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q838330",
                     "Q33122195",
                     "Q7662222"
                  ],
                  "preferred" : "Q838330",
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/01y2jtd41",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.wisc.edu/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/University_of_Wisconsin%E2%80%93Madison"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "WI",
                     "country_subdivision_name" : "Wisconsin",
                     "lat" : 43.07305,
                     "lng" : -89.40123,
                     "name" : "Madison"
                  },
                  "geonames_id" : 5261457
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "UW"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "UW–Madison"
               },
               {
                  "lang" : "es",
                  "types" : [
                     "label"
                  ],
                  "value" : "Universidad de Wisconsin-Madison"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "University of Wisconsin–Madison"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "label"
                  ],
                  "value" : "Université du Wisconsin à Madison"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/05cb4rb43",
                  "label" : "Morgridge Institute for Research",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04gq8q482",
                  "label" : "North Temperate Lakes Long Term Ecological Research",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02kj3rm24",
                  "label" : "Wisconsin Geological and Natural History Survey",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03ydkyb10",
                  "label" : "University of Wisconsin System",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/03b8vas82",
                  "label" : "National Atmospheric Deposition Program",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/04r3s7465",
                  "label" : "McMurdo Dry Valleys Long Term Ecological Research",
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
         "substring" : "university of wisconsin madison"
      },
      {
         "chosen" : false,
         "matching_type" : "EXACT",
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
            "established" : 1924,
            "external_ids" : [
               {
                  "all" : [
                     "grid.412647.2"
                  ],
                  "preferred" : "grid.412647.2",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0000 9209 0955"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q7896631"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/02mqqhj42",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.uwhealth.org/locations/university-hospital-170"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/University_of_Wisconsin_Hospital_and_Clinics"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "WI",
                     "country_subdivision_name" : "Wisconsin",
                     "lat" : 43.07305,
                     "lng" : -89.40123,
                     "name" : "Madison"
                  },
                  "geonames_id" : 5261457
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "UW Health University Hospital"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "UW Hospital and Clinics"
               },
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "UWHC"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "University of Wisconsin Health University Hospital"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "University of Wisconsin Hospital and Clinics"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "University of Wisconsin-Madison Hospital"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Wisconsin General Hospital"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/01e4byj08",
                  "label" : "University of Wisconsin Carbone Cancer Center",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03e3qgk42",
                  "label" : "University of Wisconsin Health",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "healthcare"
            ]
         },
         "score" : 0.87,
         "substring" : "university of wisconsin madison"
      },
      {
         "chosen" : false,
         "matching_type" : "EXACT",
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
            "established" : 1940,
            "external_ids" : [
               {
                  "all" : [
                     "100007923"
                  ],
                  "preferred" : "100007923",
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.412639.b"
                  ],
                  "preferred" : "grid.412639.b",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 2191 1477"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q7876154"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/01e4byj08",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://cancer.wisc.edu"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/University_of_Wisconsin_Carbone_Cancer_Center"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "US",
                     "country_name" : "United States",
                     "country_subdivision_code" : "WI",
                     "country_subdivision_name" : "Wisconsin",
                     "lat" : 43.07305,
                     "lng" : -89.40123,
                     "name" : "Madison"
                  },
                  "geonames_id" : 5261457
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Carbone Cancer Center"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "UW Carbone"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "UW Carbone Cancer Center"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "UW Carbone Comprehensive Cancer Center"
               },
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "UWCCC"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "UWCCC Madison"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "University of Wisconsin Cancer Center"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "University of Wisconsin Carbone Cancer Center"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "University of Wisconsin Carbone Comprehensive Cancer Center"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "University of Wisconsin Comprehensive Cancer Center"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "University of Wisconsin-Madison Carbone Cancer Center"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/02mqqhj42",
                  "label" : "UW Health University Hospital",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "funder",
               "healthcare"
            ]
         },
         "score" : 0.76,
         "substring" : "university of wisconsin madison"
      }
   ],
   "number_of_results" : 3
}
```

> 🚧 Don't automatically select the first "unchosen" result of an ?affiliation query with no `chosen: true` result
>
> When no result has `"chosen": true`, the first result is not necessarily the best match for a given affiliation string. In that case, several results may have the exact same score and/or there may be no match with a high score. Because the affiliation service breaks a given affiliation string into multiple substrings and performs searches of each substring on its own as well as in combination with other substrings, a high scoring match that is not selected as chosen may be a match to only a small portion of the entire affiliation string. In these cases, it is best to respect the absence of `"chosen: true"` and leave the string unmatched or add a layer of human or machine assignment, at your discretion.

#### Additional request examples:

Incorporated Research Institutions for Seismology (IRIS)

```curl
curl "https://api.ror.org/v2/organizations?affiliation=Incorporated%20Research%20Institutions%20for%20Seismology%20(IRIS)" | json_pp
```

Universitätsbibliothek der Ludwig-Maximilians-Universität München

```curl
curl "https://api.ror.org/v2/organizations?affiliation=Universit%C3%A4tsbibliothek%20der%20Ludwig-Maximilians-Universit%C3%A4t%20M%C3%BCnchen" | json_pp
```

Department of Civil and Industrial Engineering, University of Pisa, Largo Lucio Lazzarino 2, Pisa 56126, Italy

```curl
curl "https://api.ror.org/v2/organizations?affiliation=Department%20of%20Civil%20and%20Industrial%20Engineering%2C%20University%20of%20Pisa%2C%20Largo%20Lucio%20Lazzarino%202%2C%20Pisa%2056126%2C%20Italy" | json_pp
```

### Query parameter approach

Search all indexed fields in ROR records using the format `https://api.ror.org/v2/organizations?query=<search term>&filter=<filters>`. We strongly recommend using organization type and country [filters](doc:filtering) in order to retrieve fewer, more precise results. For more information, see the [REST API guide](doc:rest-api).

```curl
curl "https://api.ror.org/v2/organizations?query=%22Ministry+of+Health%22&filter=types:Government,country.country_code:NZ" | json_pp
```

The response is a JSON object containing full records for up to the first 20 search results. Results are paginated: see [API paging](doc:api-paging) for details. Matching scores are not included. The closest matches are at the beginning of the list, but we recommend human evaluation of these matches when using the query parameter approach.

> 📘 Query parameter searches may return many results, so some amount of human intervention may be needed to determine the matching ROR ID (if there is one).

```curl
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
         "established" : 1903,
         "external_ids" : [
            {
               "all" : [
                  "501100001504"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.415708.f"
               ],
               "preferred" : "grid.415708.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0483 5988"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q16933991"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00vjb5165",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.health.govt.nz/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Ministry_of_Health_(New_Zealand)"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "NZ",
                  "country_name" : "New Zealand",
                  "country_subdivision_code" : "WGN",
                  "country_subdivision_name" : "Wellington Region",
                  "lat" : -41.28664,
                  "lng" : 174.77557,
                  "name" : "Wellington"
               },
               "geonames_id" : 2179537
            }
         ],
         "names" : [
            {
               "lang" : "mi",
               "types" : [
                  "alias"
               ],
               "value" : "Manatū Hauora"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
            "government"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 1,
            "id" : "oc",
            "title" : "Oceania"
         }
      ],
      "countries" : [
         {
            "count" : 1,
            "id" : "nz",
            "title" : "New Zealand"
         }
      ],
      "statuses" : [
         {
            "count" : 1,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 1,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 1,
            "id" : "government",
            "title" : "government"
         }
      ]
   },
   "number_of_results" : 1,
   "time_taken" : 18
}
```

## Matching a list of organization names to ROR IDs using a script

You can write your own script to match a list of organization names to ROR IDs via the ROR API, though some manual intervention will likely be needed to make sure that matches are correct. If you are writing a script, remember the following:

* Try the [affiliation parameter approach](#affiliation-parameter-approach) first instead of the [query parameter approach](#query-parameter-approach).
* Look for "chosen" results where `"chosen": true`. 
* If no results are found, try alternate names/acronyms (if you have them) or try the query parameter approach. 
* API results are paginated: see [API paging](#api-paging) for details.
* By default, API requests will return only records with an active [status](doc:data-structure#status): `status: "active"`. Consider whether you want to retrieve records with an inactive status as well; inactive records generally represent organizations that no longer operate. See [API filtering](doc:api-filtering) for details. 

See, use, and clone examples of Python scripts that match organization names to ROR IDs in the [ror-utilities Github repository](https://github.com/ror-community/ror-utilities).

## Match organization names to ROR IDs using the data dump

Instead of using the ROR API, you can use your own scripts or processing tools on the [ROR data dump](doc:data-dump) to match organization names to ROR IDs. Advantages of this approach include:

* Fine-grained control over matching criteria
* Faster processing in cases where you have many IDs to map
* No chance of error responses due to network interruptions or API outages

## Match organization names to ROR using OpenRefine

The **ROR OpenRefine Reconciler** is a fairly labor-intensive way of matching organization names to ROR IDs, but it works well for those who have a relatively small list of organization names, those who want to have a high degree of control and oversight over the matching process, and those who do not want to write code. 

[OpenRefine](https://openrefine.org) (formerly Google Refine) is a free, open source desktop tool for cleaning up messy data stored in common formats like CSV, JSON, XML, XLS. You can even use it to connect to SQL-based databases and Google Sheets. 

See [ROR OpenRefine Reconciler](doc:openrefine-reconciler) for written usage instructions, screenshots, and a tutorial video. 

## Match organization names to ROR IDs using third-party tools

Several projects and researchers have developed scripts and/or machine learning and artificial intelligence tools that match textual organization information to ROR IDs. Several of these tools are fast and can work with large amounts of data with accuracy rates before human intervention ranging from about 85% to 95%. These tools are not officially supported by ROR, but we list them here in case you find them useful. 

* [OpenAlex Institution Parsing](https://github.com/ourresearch/openalex-institution-parsing) by OurResearch

* [S2AFF - Semantic Scholar Affiliations Linker](https://github.com/allenai/S2AFF/) by the Allen AI Institute

* [RORRetriever](https://github.com/Metadata-Game-Changers/RORRetriever) by Metadata Game Changers

* [EMBL-EBI ROR Predictor prototype](https://gitlab.ebi.ac.uk/literature-services/public-projects/ROR-proto-EMBL) by EMBL-EBI for [Project FREYA](https://www.project-freya.eu/Plone/en)

* [dataESR affiliation matcher](https://github.com/dataesr/affiliation-matcher) developed by Anne L'Hôte and Eric Jeangirard for the French Ministry of Higher Education

* [OpenAlex ROR Predictor (gpu-based)](https://github.com/adambuttrick/openalex-ror-predictor) by ROR Curation Lead Adam Buttrick

* [fastText ROR Predictor (cpu-based)](https://github.com/adambuttrick/ror-predictor-fasttext) by ROR Curation Lead Adam Buttrick

* [lr\_predictor](https://github.com/adambuttrick/lr_matches) by ROR Curation Lead Adam Buttrick

* [ROR experimental affiliation matching](https://github.com/ror-community/affiliation-matching-experimental) - A collection of data and code for training models and experimenting with automatically matching affiliation strings to ROR IDs. Not production code, and not officially supported by ROR.

## Testing and training data

ROR is currently experimenting with strategies for automatically matching affiliation strings to ROR IDs, and we have collected sets of data from Springer Nature, the American Physical Society, Crossref, and OpenAlex for testing and training matching strategies that are openly available at [https://github.com/ror-community/affiliation-matching-experimental/tree/main/test\_data](https://github.com/ror-community/affiliation-matching-experimental/tree/main/test_data). These datasets include affiliation text strings from production systems that have been matched to ROR IDs with varying levels of human review.
