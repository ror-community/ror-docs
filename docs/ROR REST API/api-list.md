---
title: Retrieve a list of records
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: Retrieve a list of ROR records
  description: Instructions for using the ROR API to retrieve a list of ROR records.
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: api-filtering
      title: Filtering
---
> 👍 ROR REST API v2
>
> This page documents v2 of the ROR REST API. For v1 documentation of the ROR REST API, see [https://ror.readme.io/v1/docs/api-list](https://ror.readme.io/v1/docs/api-list). You can also read more about ROR [API versions](doc:api-versions) and a summary of what's new in [Schema 2.0](doc:schema-v2) and [Schema 2.1](doc:schema-2-1).

> 🚧 Version 1 of the ROR schema and API will be sunset in December 2025
>
> In December 2025, version 1 of the ROR schema and API will be sunset, meaning that ROR API requests with v1 in the path will no longer return a response, v1 files will no longer be included in the ROR data dump, and v1 documentation will no longer be available. Read more in our [changelog](https://ror.readme.io/changelog/2025-07-01-sunset-of-version-1#/).

# Retrieve a list of active records with summary counts

You can retrieve a list of active organization records from ROR along with summary metadata such as counts of organization by continent, country, and type.

## Example

```curl
curl 'https://api.ror.org/v2/organizations' | json_pp
```

The response is a JSON object containing full records for the first 20 organizations in ROR in the `items` array and metadata about the entire dataset in the `metadata` array, including the count of active records in the ROR registry, the count of records for each organization type, the count of records for each continent, and the count of records for each of the 10 most common countries. See [ROR data structure](doc:ror-data-structure) for details about the fields and values in a ROR record, and see also [Paging](doc:api-paging) through results.

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
         "established" : 1887,
         "external_ids" : [
            {
               "all" : [
                  "501100001780",
                  "100008690",
                  "100010552"
               ],
               "preferred" : "501100001780",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1017.7"
               ],
               "preferred" : "grid.1017.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2163 3550"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1057890"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04ttjf776",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.rmit.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/RMIT_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "RMIT"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "RMIT University"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Royal Melbourne Institute of Technology University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/039p7nx39",
               "label" : "ARC Centre of Excellence for Automated Decision-Making and Society",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03m3ca021",
               "label" : "RMIT Europe",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/004axh929",
               "label" : "RMIT Vietnam",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/010mv7n52",
               "label" : "Austin Hospital",
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
         "established" : 1964,
         "external_ids" : [
            {
               "all" : [
                  "501100001215"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1018.8"
               ],
               "preferred" : "grid.1018.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2342 0938"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1478723"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01rxfrp27",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.latrobe.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/La_Trobe_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "La Trobe University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/010mv7n52",
               "label" : "Austin Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/0484pjq71",
               "label" : "Box Hill Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03grnna41",
               "label" : "Royal Women's Hospital",
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
         "established" : 1967,
         "external_ids" : [
            {
               "all" : [
                  "501100001790"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1023.0"
               ],
               "preferred" : "grid.1023.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2193 0854"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1053985"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/023q4bk22",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.cqu.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Central_Queensland_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "QLD",
                  "country_subdivision_name" : "Queensland",
                  "lat" : -23.38032,
                  "lng" : 150.50595,
                  "name" : "Rockhampton"
               },
               "geonames_id" : 2151437
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CQU"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "CQUniversity"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Central Queensland University"
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
         "established" : 1987,
         "external_ids" : [
            {
               "all" : [
                  "501100001789"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1033.1"
               ],
               "preferred" : "grid.1033.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0405 3820"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q892188"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/006jxzx88",
         "links" : [
            {
               "type" : "website",
               "value" : "http://bond.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bond_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "QLD",
                  "country_subdivision_name" : "Queensland",
                  "lat" : -28.00029,
                  "lng" : 153.43088,
                  "name" : "Gold Coast"
               },
               "geonames_id" : 2165087
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bond University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05eq01d13",
               "label" : "Gold Coast Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/0257s2812",
               "label" : "Robina Hospital",
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
         "established" : 1989,
         "external_ids" : [
            {
               "all" : [
                  "501100001769"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1037.5"
               ],
               "preferred" : "grid.1037.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0368 0777"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1066188"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00wfvh315",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.csu.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Charles_Sturt_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "NSW",
                  "country_subdivision_name" : "New South Wales",
                  "lat" : -33.41665,
                  "lng" : 149.5806,
                  "name" : "Bathurst"
               },
               "geonames_id" : 2176632
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CSU"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Charles Sturt University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05newpx76",
               "label" : "Wagga Wagga Base Hospital",
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
         "established" : 1986,
         "external_ids" : [
            {
               "all" : [
                  "100008561"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1056.2"
               ],
               "preferred" : "grid.1056.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2224 8486"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3151717"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05ktbsm52",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.burnet.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Burnet_Institute"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Burnet Institute"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02bfwt286",
               "label" : "Monash University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01ej9dk98",
               "label" : "University of Melbourne",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "funder",
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
         "established" : 1998,
         "external_ids" : [
            {
               "all" : [
                  "grid.1064.3"
               ],
               "preferred" : "grid.1064.3",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/00nx6aa03",
         "links" : [
            {
               "type" : "website",
               "value" : "http://research.mater.org.au/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "QLD",
                  "country_subdivision_name" : "Queensland",
                  "lat" : -27.46794,
                  "lng" : 153.02809,
                  "name" : "Brisbane"
               },
               "geonames_id" : 2174003
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Mater Research"
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
         "established" : 1951,
         "external_ids" : [
            {
               "all" : [
                  "grid.1073.5"
               ],
               "preferred" : "grid.1073.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0626 201X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7592065"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02k3cxs74",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.svi.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/St._Vincent's_Institute_of_Medical_Research"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.79839,
                  "lng" : 144.97833,
                  "name" : "Fitzroy"
               },
               "geonames_id" : 2166584
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "SVI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "St Vincents Institute of Medical Research"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/001kjn539",
               "label" : "St Vincent's Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01ej9dk98",
               "label" : "University of Melbourne",
               "type" : "related"
            }
         ],
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
         "established" : 1989,
         "external_ids" : [
            {
               "all" : [
                  "501100001208",
                  "100009021"
               ],
               "preferred" : "501100001208",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1076.0"
               ],
               "preferred" : "grid.1076.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0626 1885"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q17006364"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/046fa4y88",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.hri.org.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Heart_Research_Institute"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "NSW",
                  "country_subdivision_name" : "New South Wales",
                  "lat" : -33.89835,
                  "lng" : 151.17754,
                  "name" : "Newtown"
               },
               "geonames_id" : 2155386
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HRI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "The Heart Research Institute"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "facility",
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
         "established" : 1989,
         "external_ids" : [
            {
               "all" : [
                  "grid.1088.1"
               ],
               "preferred" : "grid.1088.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0622 6844"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q27982338"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02d439m40",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.epipsi.gr/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GR",
                  "country_name" : "Greece",
                  "country_subdivision_code" : "I",
                  "country_subdivision_name" : "Attica",
                  "lat" : 37.98376,
                  "lng" : 23.72784,
                  "name" : "Athens"
               },
               "geonames_id" : 264371
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UMHRI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University Mental Health Research Institute"
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
         "established" : 1904,
         "external_ids" : [
            {
               "all" : [
                  "501100000767"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1121.3"
               ],
               "preferred" : "grid.1121.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0396 1069"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q243278"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04h08p482",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.rolls-royce.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Rolls-Royce_Holdings"
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
                  "lat" : 52.92277,
                  "lng" : -1.47663,
                  "name" : "Derby"
               },
               "geonames_id" : 2651347
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Rolls-Royce (United Kingdom)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01x3p5p96",
               "label" : "Rolls-Royce (Canada)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05jeza980",
               "label" : "Rolls-Royce (Germany)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/018a0bk66",
               "label" : "Rolls-Royce (Norway)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/055ef6019",
               "label" : "Rolls-Royce (Sweden)",
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
         "established" : 1909,
         "external_ids" : [
            {
               "all" : [
                  "501100000775"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1236.6"
               ],
               "preferred" : "grid.1236.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0790 9434"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q152057"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01zctcs90",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.bp.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/BP"
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
                  "lat" : 51.50853,
                  "lng" : -0.12574,
                  "name" : "London"
               },
               "geonames_id" : 2643743
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "BP (United Kingdom)"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "BP Amoco"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "The British Petroleum Company"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/051177492",
               "label" : "BP (Canada)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03f9m1k43",
               "label" : "BP (France)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/048q66y67",
               "label" : "BP (Germany)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05dfvhh79",
               "label" : "BP (Spain)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/051659894",
               "label" : "BP (United States)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0503ft796",
               "label" : "Castrol (United Kingdom)",
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
         "established" : 1873,
         "external_ids" : [
            {
               "all" : [
                  "grid.1281.a"
               ],
               "preferred" : "grid.1281.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q821293"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05m7zw681",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.riotinto.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Rio_Tinto_Group"
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
                  "lat" : 51.50853,
                  "lng" : -0.12574,
                  "name" : "London"
               },
               "geonames_id" : 2643743
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Rio Tinto (United Kingdom)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03z28c516",
               "label" : "Rio Tinto (Australia)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00mv4xe50",
               "label" : "Rio Tinto (Canada)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01gykgk63",
               "label" : "Rio Tinto (France)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01z3j2f47",
               "label" : "Rio Tinto (Switzerland)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02vhqqh86",
               "label" : "Rio Tinto (United States)",
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
         "established" : 1946,
         "external_ids" : [
            {
               "all" : [
                  "grid.1403.6"
               ],
               "preferred" : "grid.1403.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0462 5657"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/03awtex73",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.arup.com/Global_locations/USA.aspx"
            },
            {
               "type" : "wikipedia",
               "value" : "https://www.arup.com/offices/united-states-of-america"
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
                  "lat" : 42.3751,
                  "lng" : -71.10561,
                  "name" : "Cambridge"
               },
               "geonames_id" : 4931972
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "Arup"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Arup Group (United States)"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "Ove N. Arup Consulting Engineers"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/057d3rj91",
               "label" : "Arup Group (United Kingdom)",
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
         "established" : 1980,
         "external_ids" : [
            {
               "all" : [
                  "501100000577",
                  "501100000842"
               ],
               "preferred" : "501100000577",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1453.3"
               ],
               "preferred" : "grid.1453.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 1091 7144"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q593786"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00kv9pj15",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.btplc.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/BT_Group"
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
                  "lat" : 51.50853,
                  "lng" : -0.12574,
                  "name" : "London"
               },
               "geonames_id" : 2643743
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "BT Group (United Kingdom)"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "British Telecommunications"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01xzpd518",
               "label" : "BT Archives",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03308db07",
               "label" : "BT Research",
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
         "established" : 1906,
         "external_ids" : [
            {
               "all" : [
                  "grid.1491.d"
               ],
               "preferred" : "grid.1491.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0642 1746"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q6786513"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03mjtdk61",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.mater.org.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Mater_Health_Services"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "QLD",
                  "country_subdivision_name" : "Queensland",
                  "lat" : -27.46794,
                  "lng" : 153.02809,
                  "name" : "Brisbane"
               },
               "geonames_id" : 2174003
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Mater Health Services"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05wqhv079",
               "label" : "Mater Adult Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04mxrtj31",
               "label" : "Mater Children's Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01k4cfw02",
               "label" : "Mater Misericordiae Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03j4rdg62",
               "label" : "Mater Mothers' Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03w94w157",
               "label" : "Mater Private Hospital",
               "type" : "child"
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
         "established" : 1826,
         "external_ids" : [
            {
               "all" : [
                  "grid.1504.0"
               ],
               "preferred" : "grid.1504.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q733487"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04yyp8h20",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.pilkington.com/en-GB/uk"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Pilkington"
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
                  "lat" : 53.45,
                  "lng" : -2.73333,
                  "name" : "St Helens"
               },
               "geonames_id" : 2638785
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Pilkington (United Kingdom)"
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
         "established" : 1976,
         "external_ids" : [
            {
               "all" : [
                  "grid.1543.3"
               ],
               "preferred" : "grid.1543.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0522 6113"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/022rkxt86",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.trojantechnologies.com/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "CA",
                  "country_name" : "Canada",
                  "country_subdivision_code" : "ON",
                  "country_subdivision_name" : "Ontario",
                  "lat" : 42.98339,
                  "lng" : -81.23304,
                  "name" : "London"
               },
               "geonames_id" : 6058560
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Trojan Technologies (Canada)"
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
         "established" : 1871,
         "external_ids" : [
            {
               "all" : [
                  "grid.1623.6"
               ],
               "preferred" : "grid.1623.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0432 511X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7713086"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01wddqe20",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.alfredhealth.org.au/the-alfred"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/The_Alfred_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "The Alfred"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "The Alfred Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04scfb908",
               "label" : "Alfred Health",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02bfwt286",
               "label" : "Monash University",
               "type" : "related"
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
         "established" : 1989,
         "external_ids" : [
            {
               "all" : [
                  "grid.1694.a"
               ],
               "preferred" : "grid.1694.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q8031120"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03kwrfk72",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.wch.sa.gov.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Women's_and_Children's_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "SA",
                  "country_subdivision_name" : "South Australia",
                  "lat" : -34.92866,
                  "lng" : 138.59863,
                  "name" : "Adelaide"
               },
               "geonames_id" : 2078025
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "WCH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Women's and Children's Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01kpzv902",
               "label" : "Flinders University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00892tw58",
               "label" : "University of Adelaide",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01p93h210",
               "label" : "University of South Australia",
               "type" : "related"
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
\
```

> ❗️ It is not possible to retrieve all ROR records from the API
>
> The API is best for use cases that involve querying or retrieving individual records. The maximum number of results that can be retrieved via the API is 10,000, which means that it is currently not possible to retrieve all 100,000+ records from the ROR API, even with [Paging](doc:api-paging). If you need to use the entire ROR dataset in your application, please download the [data dump](doc:data-dump).

# Retrieve a list of records with all statuses

As of 1 Dec 2022 the ROR API will return only records with a status of _active_ by default: [see release notes](https://ror.readme.io/changelog/2022-12-01-organization-status-changes).

Records with _active_ status indicate organizations that maintain current operations, while records with _inactive_ status indicate organizations that have ceased functioning, and records with _withdrawn_ status indicate records that were added to ROR in error (e.g., duplicate records and out-of-scope organizations). See [ROR data structure](doc:data-structure#status) for more information about record status.

Add the query parameter `all_status` to return records with all statuses, including _active_, _inactive_, and _withdrawn_).

## Example

```curl
curl 'https://api.ror.org/v2/organizations?all_status' | json_pp
```

```json
{
   "items" : [
      {
         "admin" : {
            "created" : {
               "date" : "2019-02-17",
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
                  "grid.503348.9"
               ],
               "preferred" : "grid.503348.9",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0620 5541"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q51780760"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03bbjky47",
         "links" : [
            {
               "type" : "website",
               "value" : "http://carmen.univ-lyon1.fr/"
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
                  "lat" : 45.71404,
                  "lng" : 4.80755,
                  "name" : "Oullins"
               },
               "geonames_id" : 2988998
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CarMeN"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratoire CarMeN"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Laboratoire de Recherche en Cardiovasculaire, Métabolisme, Diabétologie et Nutrition"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/029brtt94",
               "label" : "Université Claude Bernard Lyon 1",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02vjkv261",
               "label" : "Inserm",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/050jn9y42",
               "label" : "Institut National des Sciences Appliquées de Lyon",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/003vg9w96",
               "label" : "Institut National de Recherche pour l'Agriculture, l'Alimentation et l'Environnement",
               "type" : "parent"
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
         "established" : 2012,
         "external_ids" : [
            {
               "all" : [
                  "grid.462942.f"
               ],
               "preferred" : "grid.462942.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0822 6088"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30261606"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02ek9wp67",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.greqam.fr/en"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "PAC",
                  "country_subdivision_name" : "Provence-Alpes-Côte d'Azur",
                  "lat" : 43.29695,
                  "lng" : 5.38107,
                  "name" : "Marseille"
               },
               "geonames_id" : 2995469
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "GREQAM"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Groupement de Recherche en Économie Quantitative d’Aix-Marseille"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/035xkbk20",
               "label" : "Aix-Marseille Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/040baw385",
               "label" : "Centrale Marseille",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/04b0z7q78",
               "label" : "Institut des Sciences Humaines et Sociales",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02d9dg697",
               "label" : "École des hautes études en sciences sociales",
               "type" : "parent"
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
               "date" : "2024-06-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "cbnbl.org"
         ],
         "established" : 1991,
         "external_ids" : [],
         "id" : "https://ror.org/001q9w183",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.cbnbl.org"
            },
            {
               "type" : "wikipedia",
               "value" : "https://fr.wikipedia.org/wiki/Conservatoire_botanique_national_de_Bailleul"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "HDF",
                  "country_subdivision_name" : "Hauts-de-France",
                  "lat" : 50.73592,
                  "lng" : 2.73594,
                  "name" : "Bailleul"
               },
               "geonames_id" : 3035359
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "acronym"
               ],
               "value" : "CBNBL"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Centre régional de phytosociologie"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Conservatoire botanique national de Bailleul"
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
         "established" : 1960,
         "external_ids" : [
            {
               "all" : [
                  "grid.483349.1"
               ],
               "preferred" : "grid.483349.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0382 3475"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q2945466"
               ],
               "preferred" : "Q2945466",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0266y7j75",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.sciencespo.fr/cevipof/"
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
               "value" : "CEVIPOF"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Centre de Recherches Politiques"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Centre de Recherches Politiques de Sciences Po"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Centre for Political Research at Sciences"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Centre for Political Research at Sciences Po"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04b0z7q78",
               "label" : "Institut des Sciences Humaines et Sociales",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05fe7ax82",
               "label" : "Institut d'Etudes Politiques de Paris",
               "type" : "parent"
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
               "date" : "2024-06-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2019,
         "external_ids" : [
            {
               "all" : [
                  "Q105342636"
               ],
               "preferred" : "Q105342636",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03kfrvm58",
         "links" : [
            {
               "type" : "website",
               "value" : "https://sms-institute.com"
            },
            {
               "type" : "wikipedia",
               "value" : "https://ru.wikipedia.org/wiki/%D0%98%D0%BD%D1%81%D1%82%D0%B8%D1%82%D1%83%D1%82_%D1%81%D0%BE%D1%86%D0%B8%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D1%85_%D0%B8_%D0%BC%D0%B5%D0%B4%D0%B8%D0%B0_%D0%B8%D1%81%D1%81%D0%BB%D0%B5%D0%B4%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B9"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "MA",
                  "country_name" : "Morocco",
                  "country_subdivision_code" : "06",
                  "country_subdivision_name" : "Casablanca-Settat",
                  "lat" : 33.58831,
                  "lng" : -7.61138,
                  "name" : "Casablanca"
               },
               "geonames_id" : 2553604
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Institut d’Etudes Sociales et Médiatiques"
            },
            {
               "lang" : "en",
               "types" : [
                  "acronym"
               ],
               "value" : "SMSI"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Social and Media Studies Institute"
            },
            {
               "lang" : "ar",
               "types" : [
                  "label"
               ],
               "value" : "معهدالدراسات الاجتماعية و الإعلامية"
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
         "established" : 1981,
         "external_ids" : [
            {
               "all" : [
                  "grid.462261.5"
               ],
               "preferred" : "grid.462261.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0452 2471"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/0075hvk77",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.hds.utc.fr/?lang=en"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "HDF",
                  "country_subdivision_name" : "Hauts-de-France",
                  "lat" : 49.41794,
                  "lng" : 2.82606,
                  "name" : "Compiègne"
               },
               "geonames_id" : 3024066
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HEUDIASYC"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Heuristics and Diagnostics for Complex Systems"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Heuristique et Diagnostic des Systèmes Complexes"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04z22qz54",
               "label" : "Institut des Sciences de l'Information et de leurs Interactions",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/04y5kwa70",
               "label" : "Université de Technologie de Compiègne",
               "type" : "parent"
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
               "date" : "2024-06-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "dem.org.tr"
         ],
         "established" : 2003,
         "external_ids" : [
            {
               "all" : [
                  "Q74530700"
               ],
               "preferred" : "Q74530700",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02trpr211",
         "links" : [
            {
               "type" : "website",
               "value" : "https://dem.org.tr"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "TR",
                  "country_name" : "Türkiye",
                  "country_subdivision_code" : "34",
                  "country_subdivision_name" : "Istanbul",
                  "lat" : 41.01384,
                  "lng" : 28.94966,
                  "name" : "Istanbul"
               },
               "geonames_id" : 745044
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "acronym"
               ],
               "value" : "CVE"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Center for Values Education"
            },
            {
               "lang" : "tr",
               "types" : [
                  "acronym"
               ],
               "value" : "DEM"
            },
            {
               "lang" : "tr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Değerler Eğitimi Merkezi"
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
         "established" : 2004,
         "external_ids" : [
            {
               "all" : [
                  "grid.464083.d"
               ],
               "preferred" : "grid.464083.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0384 1227"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/04qq0qp34",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.lof.cnrs.fr/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "NAQ",
                  "country_subdivision_name" : "Nouvelle-Aquitaine",
                  "lat" : 44.80565,
                  "lng" : -0.6324,
                  "name" : "Pessac"
               },
               "geonames_id" : 2987805
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LOF"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratoire du Futur"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/054qv7y42",
               "label" : "Institut Polytechnique de Bordeaux",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02cte4b68",
               "label" : "Institut de Chimie",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/01jyh4590",
               "label" : "Solvay (France)",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/057qpr032",
               "label" : "Université de Bordeaux",
               "type" : "parent"
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
               "date" : "2019-02-17",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2015,
         "external_ids" : [
            {
               "all" : [
                  "grid.503376.4"
               ],
               "preferred" : "grid.503376.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q51784880"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05qdnns64",
         "links" : [
            {
               "type" : "website",
               "value" : "http://maiage.jouy.inra.fr"
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
                  "lat" : 48.75909,
                  "lng" : 2.16966,
                  "name" : "Jouy-en-Josas"
               },
               "geonames_id" : 3012165
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Applied Mathematics and Computer Science, from Genomes to the Environment"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "MAIAGE"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Mathématiques et Informatique Appliquées du Génome à l'Environnement"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00gtg0p11",
               "label" : "Centre Île-de-France - Jouy-en-Josas - Antony",
               "type" : "parent"
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
         "established" : 2004,
         "external_ids" : [
            {
               "all" : [
                  "grid.464121.4"
               ],
               "preferred" : "grid.464121.4",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/03s92mv58",
         "links" : [
            {
               "type" : "website",
               "value" : "http://geops.geol.u-psud.fr/"
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
                  "lat" : 48.69572,
                  "lng" : 2.18727,
                  "name" : "Orsay"
               },
               "geonames_id" : 2989204
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "GEOPS"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Geosciences Paris Sud"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04kdfz702",
               "label" : "Institut National des Sciences de l'Univers",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/028rypz17",
               "label" : "Université Paris-Sud",
               "type" : "parent"
            }
         ],
         "status" : "inactive",
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
         "established" : 1923,
         "external_ids" : [
            {
               "all" : [
                  "grid.426470.3"
               ],
               "preferred" : "grid.426470.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 1939 8045"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q8475"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02cyaqf66",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.interpol.int/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Interpol"
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
                  "lat" : 45.74846,
                  "lng" : 4.84671,
                  "name" : "Lyon"
               },
               "geonames_id" : 2996944
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ICPC"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ICPO"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "International Criminal Police Commission"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "International Criminal Police Organization"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Interpol"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "OIPC"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Organisation internationale de police criminelle"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
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
         "established" : 1897,
         "external_ids" : [
            {
               "all" : [
                  "grid.414438.e"
               ],
               "preferred" : "grid.414438.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9834 707X"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/0338wkj94",
         "links" : [
            {
               "type" : "website",
               "value" : "http://fr.ap-hm.fr/nos-hopitaux/hopitaux-sud"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "PAC",
                  "country_subdivision_name" : "Provence-Alpes-Côte d'Azur",
                  "lat" : 43.29695,
                  "lng" : 5.38107,
                  "name" : "Marseille"
               },
               "geonames_id" : 2995469
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Hôpital Sainte-Marguerite"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/002cp4060",
               "label" : "Assistance Publique Hôpitaux de Marseille",
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
               "date" : "2023-11-08",
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
                  "0000 0004 9334 1831"
               ],
               "preferred" : "0000 0004 9334 1831",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/0502xk698",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.navsea.navy.mil/Home/Warfare-Centers/NSWC-Philadelphia/"
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
               "value" : "NSWC Philadelphia"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "NSWC Philadelphia Division"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "NSWCPD"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Naval Surface Warfare Center Philadelphia Division"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03d4ecn10",
               "label" : "Naval Surface Warfare Center",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "facility",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.420339.f"
               ],
               "preferred" : "grid.420339.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0464 6124"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30282483"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0454zjr22",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www6.val-de-loire.inrae.fr/infectiologie-santepublique"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "CVL",
                  "country_subdivision_name" : "Centre-Val de Loire",
                  "lat" : 47.54499,
                  "lng" : 0.74623,
                  "name" : "Nouzilly"
               },
               "geonames_id" : 2989945
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Infectiologie Animale et Santé Publique"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02wwzvj46",
               "label" : "Université de Tours",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/003vg9w96",
               "label" : "Institut National de Recherche pour l'Agriculture, l'Alimentation et l'Environnement",
               "type" : "parent"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "501100006488"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.414548.8"
               ],
               "preferred" : "grid.414548.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2169 1988"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1665106"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01x3gbx83",
         "links" : [
            {
               "type" : "website",
               "value" : "http://institut.inra.fr/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Institut_national_de_la_recherche_agronomique"
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
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "French National Institute for Agricultural Research"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "INRA"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Institut National de la Recherche Agronomique"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/003vg9w96",
               "label" : "Institut National de Recherche pour l'Agriculture, l'Alimentation et l'Environnement",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
         "established" : 2012,
         "external_ids" : [
            {
               "all" : [
                  "grid.463924.e"
               ],
               "preferred" : "grid.463924.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0452 4645"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30262302"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04kgf6p94",
         "links" : [
            {
               "type" : "website",
               "value" : "http://piim.univ-amu.fr/?lang=en"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "PAC",
                  "country_subdivision_name" : "Provence-Alpes-Côte d'Azur",
                  "lat" : 43.29695,
                  "lng" : 5.38107,
                  "name" : "Marseille"
               },
               "geonames_id" : 2995469
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "P2IM"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Physics of Ionic and Molecular Interactions"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Physique des interactions ioniques et moléculaires"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/035xkbk20",
               "label" : "Aix-Marseille Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/00s19x989",
               "label" : "CNRS Ingénierie",
               "type" : "parent"
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
               "date" : "2023-06-22",
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
                  "0000 0001 1456 6679"
               ],
               "preferred" : "0000 0001 1456 6679",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/05wfw4946",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.enseeiht.fr"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "OCC",
                  "country_subdivision_name" : "Occitanie",
                  "lat" : 43.60426,
                  "lng" : 1.44367,
                  "name" : "Toulouse"
               },
               "geonames_id" : 2972315
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ENSEEIHT"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Escuela Nacional Superior de Electrotécnica, Electrónica, Informática, Hidráulica y Telecomunicaciones de Toulouse"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "INP-ENSEEIHT"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "National Institute of Electrical Engineering, Electronics, Computer Science,Fluid Mechanics & Telecommunications and Networks"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "École Nationale Supérieure d'Électrotechnique, d'Électronique, d'Informatique, d'Hydraulique et des Télécommunications"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/033p9g875",
               "label" : "Institut National Polytechnique de Toulouse",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/025vp2923",
               "label" : "Institut Mines-Télécom",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05hy3tk52",
               "label" : "École Polytechnique",
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
               "date" : "2023-03-30",
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
                  "0000 0001 2253 9574"
               ],
               "preferred" : "0000 0001 2253 9574",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/045q3tp69",
         "links" : [
            {
               "type" : "website",
               "value" : "https://sites-recherche.univ-rennes2.fr/cellam/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "BRE",
                  "country_subdivision_name" : "Brittany",
                  "lat" : 48.11198,
                  "lng" : -1.67429,
                  "name" : "Rennes"
               },
               "geonames_id" : 2983990
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CELLAM"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Centre d’études des langues et littératures anciennes et modernes"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UR CELLAM"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01m84wm78",
               "label" : "Université Rennes 2",
               "type" : "parent"
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
         "external_ids" : [
            {
               "all" : [
                  "Q6590703"
               ],
               "preferred" : "Q6590703",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00cm89a33",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.vet.bg.ac.rs"
            },
            {
               "type" : "wikipedia",
               "value" : "https://sr.wikipedia.org/wiki/%D0%A4%D0%B0%D0%BA%D1%83%D0%BB%D1%82%D0%B5%D1%82_%D0%B2%D0%B5%D1%82%D0%B5%D1%80%D0%B8%D0%BD%D0%B0%D1%80%D1%81%D0%BA%D0%B5_%D0%BC%D0%B5%D0%B4%D0%B8%D1%86%D0%B8%D0%BD%D0%B5_%D0%A3%D0%BD%D0%B8%D0%B2%D0%B5%D1%80%D0%B7%D0%B8%D1%82%D0%B5%D1%82%D0%B0_%D1%83_%D0%91%D0%B5%D0%BE%D0%B3%D1%80%D0%B0%D0%B4%D1%83"
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
               "lang" : "sr",
               "types" : [
                  "acronym"
               ],
               "value" : "FVM"
            },
            {
               "lang" : "sr",
               "types" : [
                  "alias"
               ],
               "value" : "Fakultet Veterinarske medicine Univerziteta u Beogradu"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "University of Belgrade – Faculty of Veterinary Medicine"
            },
            {
               "lang" : "sr",
               "types" : [
                  "label"
               ],
               "value" : "Univerzitet u Beogradu – Fakultet veterinarske medicine"
            },
            {
               "lang" : "sr",
               "types" : [
                  "alias"
               ],
               "value" : "Veterinarski fakultet Univerziteta u Beogradu"
            },
            {
               "lang" : "sr",
               "types" : [
                  "label"
               ],
               "value" : "Универзитет у Београду – Факултет ветеринарске медицине"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02qsmb048",
               "label" : "University of Belgrade",
               "type" : "parent"
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
         "established" : 2007,
         "external_ids" : [
            {
               "all" : [
                  "grid.482748.5"
               ],
               "preferred" : "grid.482748.5",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/048ryj409",
         "links" : [
            {
               "type" : "website",
               "value" : "http://passagesxx-xxi.univ-lyon2.fr/"
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
                  "lat" : 45.74846,
                  "lng" : 4.84671,
                  "name" : "Lyon"
               },
               "geonames_id" : 2996944
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratoire Passages XX_XXI"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03rth4p18",
               "label" : "Université Lumière Lyon 2",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "facility"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 44162,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 37568,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 20975,
            "id" : "as",
            "title" : "Asia"
         },
         {
            "count" : 3679,
            "id" : "af",
            "title" : "Africa"
         },
         {
            "count" : 3460,
            "id" : "sa",
            "title" : "South America"
         },
         {
            "count" : 1964,
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
            "count" : 32539,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 7726,
            "id" : "gb",
            "title" : "United Kingdom"
         },
         {
            "count" : 5417,
            "id" : "de",
            "title" : "Germany"
         },
         {
            "count" : 5031,
            "id" : "fr",
            "title" : "France"
         },
         {
            "count" : 4965,
            "id" : "cn",
            "title" : "China"
         },
         {
            "count" : 4083,
            "id" : "jp",
            "title" : "Japan"
         },
         {
            "count" : 3611,
            "id" : "ca",
            "title" : "Canada"
         },
         {
            "count" : 3223,
            "id" : "in",
            "title" : "India"
         },
         {
            "count" : 2823,
            "id" : "cz",
            "title" : "Czech Republic"
         },
         {
            "count" : 2179,
            "id" : "it",
            "title" : "Italy"
         }
      ],
      "statuses" : [
         {
            "count" : 109806,
            "id" : "active",
            "title" : "active"
         },
         {
            "count" : 1244,
            "id" : "withdrawn",
            "title" : "withdrawn"
         },
         {
            "count" : 758,
            "id" : "inactive",
            "title" : "inactive"
         }
      ],
      "types" : [
         {
            "count" : 30776,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 22029,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 17115,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 14747,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 13562,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 11750,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 8794,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 7274,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 3040,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 111808,
   "time_taken" : 19
}
```
