---
title: Filtering
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR API filtering
  description: Instructions for filtering lists of ROR records retrieved from the ROR API.
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: api-paging
      title: Paging
---
> 👍 ROR REST API v2
> 
> This page documents v2 of the ROR REST API. For v1 documentation of the ROR REST API, see <https://ror.readme.io/v1/docs/api-filtering>. You can also read more about ROR [API versions](doc:api-versions) and a summary of what's new in [Schema 2.0](doc:schema-v2) and [Schema 2.1](doc:schema-2-1).

> 🚧 Changes to the ROR API begin the week of July 28, 2025
> 
> Beginning the week of July 28, 2025, **ROR API requests with no version in the path will default to responses that use version 2 of the ROR schema instead of version 1**. Read more in our [changelog](https://ror.readme.io/changelog/2025-07-01-sunset-of-version-1).

# About filtering

Results can be filtered by record status, organization type, [ISO 3166](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code, country name, continent code, and continent name. 

- `status` - filter records by record status
- `types` - filter records by organization type 
- `country.country_code` and `locations.geonames_details.country_code` - filter records by country code
- `country.country_name` and `locations.geonames_details.country_name` - filter records by country name
- `locations.geonames_details.continent_code` - filter records by continent code
- `locations.geonames_details.continent_name` - filter records by continent name

See [Data structure](doc:ror-data-structure) and [Fields and sub-fields](doc:fields) for more details about these fields.

Note that in version 2 of the ROR API and schema, values formerly in the v1 field `country.country_code` are now in the field `locations.geonames_details.country_code`and values formerly in the v1 field `country.country_name` are now in the field `locations.geonames_details.country_name`. To maintain continuity for v1 users, we have ensured that the v1 and v2 field names for country names and country codes can be used interchangeably as filters in v2 of the ROR API.

> 📘 Filter format
> 
> `https://api.ror.org/v2/organizations?filter=[filter]:[value]`

# Filter by record status

Available statuses: _active_, _inactive_, and _withdrawn_. Requests for a specific record by its exact ROR ID will always return the record regardless of its status without need for the [all_status parameter](doc:api-list#retrieve-a-list-of-records-with-all-statuses) or for status filters.

## Example

```curl
curl 'https://api.ror.org/v2/organizations?filter=status:inactive' | json_pp
```

Retrieves a list of records for inactive research organizations. 

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
         "established" : 1997,
         "external_ids" : [
            {
               "all" : [
                  "grid.464074.5"
               ],
               "preferred" : "grid.464074.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0384 8293"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/00x5jmr79",
         "links" : [
            {
               "type" : "website",
               "value" : "http://inac.cea.fr/spram/"
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
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "SPrAM"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Structure et Propriétés d'Architectures Moléculaires"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00jjx8s55",
               "label" : "Commissariat à l'Énergie Atomique et aux Énergies Alternatives",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/00z54nq84",
               "label" : "Institut de Physique",
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
               "date" : "2023-04-12",
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
                  "0000 0001 2097 0141"
               ],
               "preferred" : "0000 0001 2097 0141",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q902688"
               ],
               "preferred" : "Q902688",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00wh4bp03",
         "links" : [],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "OCC",
                  "country_subdivision_name" : "Occitanie",
                  "lat" : 43.61093,
                  "lng" : 3.87635,
                  "name" : "Montpellier"
               },
               "geonames_id" : 2992166
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Montpellier 2 University"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Montpellier II University"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "University of Montpellier 2"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Université Montpellier 2"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Université de Montpellier II"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/051escj72",
               "label" : "Université de Montpellier",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
         "established" : 1838,
         "external_ids" : [
            {
               "all" : [
                  "grid.431175.5"
               ],
               "preferred" : "grid.431175.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0431 3522"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q6304221"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04z066j96",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.judson.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Judson_College_(Alabama)"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "AL",
                  "country_subdivision_name" : "Alabama",
                  "lat" : 32.63235,
                  "lng" : -87.31917,
                  "name" : "Marion"
               },
               "geonames_id" : 4074673
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Judson College"
            }
         ],
         "relationships" : [],
         "status" : "inactive",
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
                  "501100006459"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.412143.1"
               ],
               "preferred" : "grid.412143.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2359 716X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3551779"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/033c46s90",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.ueb.eu/versionAnglaiseV2/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/European_University_of_Brittany"
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
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "European University of Brittany"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UEB"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Université Européenne de Bretagne"
            }
         ],
         "relationships" : [],
         "status" : "inactive",
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
         "established" : 2012,
         "external_ids" : [
            {
               "all" : [
                  "grid.460792.e"
               ],
               "preferred" : "grid.460792.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q3247880"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0033vhx30",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.universiteparisseine.fr/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/University_of_Paris-Seine"
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
                  "lat" : 49.03645,
                  "lng" : 2.07613,
                  "name" : "Cergy"
               },
               "geonames_id" : 3027883
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "University of Paris-Seine"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Université Paris-Seine"
            }
         ],
         "relationships" : [],
         "status" : "inactive",
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
         "established" : 1968,
         "external_ids" : [
            {
               "all" : [
                  "501100000617",
                  "501100020171"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.421514.7"
               ],
               "preferred" : "grid.421514.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0421 7848"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/02vtmh618",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.gov.uk/government/organisations/foreign-commonwealth-office"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Foreign_and_Commonwealth_Office"
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
                  "acronym"
               ],
               "value" : "FCO"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Foreign Office"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Foreign and Commonwealth Office"
            },
            {
               "lang" : "cy",
               "types" : [
                  "label"
               ],
               "value" : "Y Swyddfa Dramor a Chymanwlad"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/009twqt47",
               "label" : "Official Development Assistance",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05wnh3t63",
               "label" : "Government of the United Kingdom",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/037wke960",
               "label" : "Foreign, Commonwealth & Development Office",
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
               "date" : "2023-04-12",
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
                  "0000 0001 2186 5845"
               ],
               "preferred" : "0000 0001 2186 5845",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q903888"
               ],
               "preferred" : "Q903888",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0227fbx80",
         "links" : [
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/University_of_Montpellier_1"
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
                  "lat" : 43.61093,
                  "lng" : 3.87635,
                  "name" : "Montpellier"
               },
               "geonames_id" : 2992166
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Montpellier 1 University"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Montpellier I University"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "University of Montpellier 1"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Université Montpellier 1"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Université de Montpellier I"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/051escj72",
               "label" : "Université de Montpellier",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
         "established" : 2005,
         "external_ids" : [
            {
               "all" : [
                  "grid.488279.8"
               ],
               "preferred" : "grid.488279.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1798 7163"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/01zss5v68",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.icloire.com/"
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
                  "lat" : 45.4739,
                  "lng" : 4.37678,
                  "name" : "Saint-Priest-en-Jarez"
               },
               "geonames_id" : 2977350
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ICLN"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institute Cancer De La Loire Lucien Neuwirth"
            }
         ],
         "relationships" : [],
         "status" : "inactive",
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
         "established" : 1949,
         "external_ids" : [
            {
               "all" : [
                  "grid.420049.b"
               ],
               "preferred" : "grid.420049.b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0152 412X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q8039905"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02zeryz43",
         "links" : [
            {
               "type" : "website",
               "value" : "https://kbr.com/markets/government/kbr-wyle/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Wyle_Laboratories"
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
                  "lat" : 33.91918,
                  "lng" : -118.41647,
                  "name" : "El Segundo"
               },
               "geonames_id" : 5345860
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "KBRwyle"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Wyle (United States)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01g1xae87",
               "label" : "KBR (United States)",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
         "types" : [
            "company"
         ]
      },
      {
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
         "established" : 1929,
         "external_ids" : [
            {
               "all" : [
                  "0000 0000 9563 1913"
               ],
               "preferred" : "0000 0000 9563 1913",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q16987718"
               ],
               "preferred" : "Q16987718",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04neq5659",
         "links" : [
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Avco"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CT",
                  "country_subdivision_name" : "Connecticut",
                  "lat" : 41.02649,
                  "lng" : -73.62846,
                  "name" : "Greenwich"
               },
               "geonames_id" : 4835395
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "Avco"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Avco (United States)"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "Aviation Corporation"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04h6azc67",
               "label" : "Textron (United States)",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
         "established" : 1997,
         "external_ids" : [
            {
               "all" : [
                  "501100000278",
                  "501100002992"
               ],
               "preferred" : "501100000278",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.433527.4"
               ],
               "preferred" : "grid.433527.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 1018 290X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q2748702"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05rf29967",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.gov.uk/government/organisations/department-for-international-development"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Department_for_International_Development"
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
                  "acronym"
               ],
               "value" : "DFID"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Department for International Development"
            },
            {
               "lang" : "cy",
               "types" : [
                  "label"
               ],
               "value" : "Yr Adran dros Ddatblygu Rhyngwladol"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05wnh3t63",
               "label" : "Government of the United Kingdom",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/037wke960",
               "label" : "Foreign, Commonwealth & Development Office",
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
               "date" : "2024-07-08",
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
         "id" : "https://ror.org/05x4mwp36",
         "links" : [
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bethany_College_%28Saskatchewan%29"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "CA",
                  "country_name" : "Canada",
                  "country_subdivision_code" : "SK",
                  "country_subdivision_name" : "Saskatchewan",
                  "lat" : 52.52431,
                  "lng" : -106.7311,
                  "name" : "Hepburn"
               },
               "geonames_id" : 12822874
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Bethany College"
            }
         ],
         "relationships" : [],
         "status" : "inactive",
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
         "domains" : [
            "usk.edu"
         ],
         "established" : 2010,
         "external_ids" : [
            {
               "all" : [
                  "grid.469262.d"
               ],
               "preferred" : "grid.469262.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 4652 9882"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q16900576"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05f1eky95",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.usk.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/St._Katherine_College"
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
                  "lat" : 33.14337,
                  "lng" : -117.16614,
                  "name" : "San Marcos"
               },
               "geonames_id" : 5392368
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "SKC"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Saint Katherine College"
            },
            {
               "lang" : "en",
               "types" : [
                  "acronym"
               ],
               "value" : "USK"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "University of Saint Katherine"
            }
         ],
         "relationships" : [],
         "status" : "inactive",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.448010.9"
               ],
               "preferred" : "grid.448010.9",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/0436nxt45",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.hbu.cas.cz/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "CZ",
                  "country_name" : "Czechia",
                  "country_subdivision_code" : "31",
                  "country_subdivision_name" : "Jihočeský kraj",
                  "lat" : 48.97447,
                  "lng" : 14.47434,
                  "name" : "České Budějovice"
               },
               "geonames_id" : 3077916
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Academy of Sciences of the Czech Republic Institute of Hydrobiology"
            },
            {
               "lang" : "cs",
               "types" : [
                  "alias"
               ],
               "value" : "Akademie Ved Ceske Republiky Hydrobiologicky Ustav"
            },
            {
               "lang" : "cs",
               "types" : [
                  "label"
               ],
               "value" : "Hydrobiologický Ústav Biologické Centrum AV ČR"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institute of Hydrobiology, Biology Centre, Academy of Sciences of the Czech Republic"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05pq4yn02",
               "label" : "Czech Academy of Sciences, Biology Centre",
               "type" : "successor"
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
         "established" : 1889,
         "external_ids" : [
            {
               "all" : [
                  "grid.466958.0"
               ],
               "preferred" : "grid.466958.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9413 8836"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q14942414"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02p4sx080",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.northamptonshire.gov.uk/en/Pages/HomePage.aspx"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Northamptonshire_County_Council"
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
               "value" : "Northamptonshire County Council"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00b03qn53",
               "label" : "North Northamptonshire Council",
               "type" : "successor"
            },
            {
               "id" : "https://ror.org/00s2fy654",
               "label" : "West Northamptonshire Council",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
         "established" : 1991,
         "external_ids" : [
            {
               "all" : [
                  "grid.416057.1"
               ],
               "preferred" : "grid.416057.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 8764 9988"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q6898230"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05p96ba03",
         "links" : [
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Monash_Institute_of_Medical_Research"
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
                  "lat" : -37.91667,
                  "lng" : 145.11667,
                  "name" : "Clayton"
               },
               "geonames_id" : 2171400
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "MIMR"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Monash Institute of Medical Research"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02bfwt286",
               "label" : "Monash University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/0083mf965",
               "label" : "Hudson Institute of Medical Research",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
         "established" : 1962,
         "external_ids" : [
            {
               "all" : [
                  "grid.447761.7"
               ],
               "preferred" : "grid.447761.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0396 9503"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/039nazg33",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.entu.cas.cz/en/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "CZ",
                  "country_name" : "Czechia",
                  "country_subdivision_code" : "31",
                  "country_subdivision_name" : "Jihočeský kraj",
                  "lat" : 48.97447,
                  "lng" : 14.47434,
                  "name" : "České Budějovice"
               },
               "geonames_id" : 3077916
            }
         ],
         "names" : [
            {
               "lang" : "cs",
               "types" : [
                  "alias"
               ],
               "value" : "Entomologický Ústav"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institute of Entomology"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/053avzc18",
               "label" : "Czech Academy of Sciences",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05pq4yn02",
               "label" : "Czech Academy of Sciences, Biology Centre",
               "type" : "successor"
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
         "established" : 1986,
         "external_ids" : [
            {
               "all" : [
                  "grid.448363.e"
               ],
               "preferred" : "grid.448363.e",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/02tz8r820",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.upb.cas.cz/en"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "CZ",
                  "country_name" : "Czechia",
                  "country_subdivision_code" : "31",
                  "country_subdivision_name" : "Jihočeský kraj",
                  "lat" : 48.97447,
                  "lng" : 14.47434,
                  "name" : "České Budějovice"
               },
               "geonames_id" : 3077916
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ISB"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institute of Soil Biology"
            },
            {
               "lang" : "cs",
               "types" : [
                  "alias"
               ],
               "value" : "Ústav Půdní Biologie"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/053avzc18",
               "label" : "Czech Academy of Sciences",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05pq4yn02",
               "label" : "Czech Academy of Sciences, Biology Centre",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
         "types" : [
            "facility"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 446,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 186,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 89,
            "id" : "as",
            "title" : "Asia"
         },
         {
            "count" : 33,
            "id" : "oc",
            "title" : "Oceania"
         },
         {
            "count" : 3,
            "id" : "af",
            "title" : "Africa"
         },
         {
            "count" : 1,
            "id" : "sa",
            "title" : "South America"
         }
      ],
      "countries" : [
         {
            "count" : 185,
            "id" : "fr",
            "title" : "France"
         },
         {
            "count" : 163,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 70,
            "id" : "gb",
            "title" : "United Kingdom"
         },
         {
            "count" : 45,
            "id" : "es",
            "title" : "Spain"
         },
         {
            "count" : 43,
            "id" : "jp",
            "title" : "Japan"
         },
         {
            "count" : 30,
            "id" : "au",
            "title" : "Australia"
         },
         {
            "count" : 26,
            "id" : "de",
            "title" : "Germany"
         },
         {
            "count" : 22,
            "id" : "ca",
            "title" : "Canada"
         },
         {
            "count" : 17,
            "id" : "ie",
            "title" : "Ireland"
         },
         {
            "count" : 12,
            "id" : "it",
            "title" : "Italy"
         }
      ],
      "statuses" : [
         {
            "count" : 758,
            "id" : "inactive",
            "title" : "inactive"
         }
      ],
      "types" : [
         {
            "count" : 212,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 173,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 162,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 156,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 104,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 52,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 41,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 26,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 1,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 758,
   "time_taken" : 4
}
```

# Filter by organization type

Available organization types: 

- education 
- funder
- healthcare 
- company 
- archive 
- nonprofit 
- government 
- facility 
- other

## Example

```curl
curl 'https://api.ror.org/v2/organizations?filter=types:facility' | json_pp
```

Retrieves a list of research facilities.  

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
      },
      {
         "admin" : {
            "created" : {
               "date" : "2023-08-17",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2014,
         "external_ids" : [
            {
               "all" : [
                  "Q51781870"
               ],
               "preferred" : "Q51781870",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05hqep952",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.iame-research.center"
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
               "value" : "IAME"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Infection, Anti-microbiens, Modélisation, Evolution"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Infection, Antimicrobials, Modelling, Evolution"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Infection, anti-microbien, modélisation, évolution"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02vjkv261",
               "label" : "Inserm",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05f82e368",
               "label" : "Université Paris Cité",
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
               "date" : "2021-04-06",
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
                  "grid.510388.7"
               ],
               "preferred" : "grid.510388.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 8342 6081"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q16508572"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01xd0ys45",
         "links" : [
            {
               "type" : "website",
               "value" : "http://lcfc.ensam.eu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Laboratory_of_design,_manufacturing_and_control"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "GES",
                  "country_subdivision_name" : "Grand Est",
                  "lat" : 49.11911,
                  "lng" : 6.17269,
                  "name" : "Metz"
               },
               "geonames_id" : 2994160
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LCFC"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratoire de Conception Fabrication Commande"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Laboratory of design, manufacturing and control"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/018pp1107",
               "label" : "École nationale supérieure d'arts et métiers",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/04vfs2w97",
               "label" : "Université de Lorraine",
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
               "date" : "2023-08-17",
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
                  "Q51784976"
               ],
               "preferred" : "Q51784976",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04fa3ke77",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www6.rennes.inra.fr/smart"
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
               "value" : "SMART"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Structures et Marchés Agricoles, Ressources et Territoires"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/003vg9w96",
               "label" : "Institut National de Recherche pour l'Agriculture, l'Alimentation et l'Environnement",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/01s3fs709",
               "label" : "Institut Agro Rennes-Angers",
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
               "date" : "2023-08-17",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2014,
         "external_ids" : [
            {
               "all" : [
                  "Q51781869"
               ],
               "preferred" : "Q51781869",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02k7ask46",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.calmip.univ-toulouse.fr"
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
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "CALMIP"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Calcul en Midi-Pyrénées"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01h8pf755",
               "label" : "Institut National des Sciences Appliquées de Toulouse",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/033p9g875",
               "label" : "Institut National Polytechnique de Toulouse",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02v6kpv12",
               "label" : "Université Toulouse III - Paul Sabatier",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02feahw73",
               "label" : "Centre National de la Recherche Scientifique",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/004raaa70",
               "label" : "Université de Toulouse",
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
         "established" : 1954,
         "external_ids" : [
            {
               "all" : [
                  "grid.461902.8"
               ],
               "preferred" : "grid.461902.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0726 3901"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3151724"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05whrjc31",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www-ics.u-strasbg.fr/?lang=en#&panel1-3"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Institut_Charles_Sadron"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "GES",
                  "country_subdivision_name" : "Grand Est",
                  "lat" : 48.58392,
                  "lng" : 7.74553,
                  "name" : "Strasbourg"
               },
               "geonames_id" : 2973783
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ICS"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut Charles Sadron"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Institut charles-sadron"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02cte4b68",
               "label" : "Institut de Chimie",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/00pg6eq24",
               "label" : "Université de Strasbourg",
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
                  "100014050"
               ],
               "preferred" : "100014050",
               "type" : "fundref"
            },
            {
               "all" : [
                  "Q105169380"
               ],
               "preferred" : "Q105169380",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/003mzyg61",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.erdc.usace.army.mil/Locations/CERL/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MS",
                  "country_subdivision_name" : "Mississippi",
                  "lat" : 32.35265,
                  "lng" : -90.87788,
                  "name" : "Vicksburg"
               },
               "geonames_id" : 4449620
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "acronym"
               ],
               "value" : "CERLen"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Construction Engineering Research Laboratory"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/027mhn368",
               "label" : "U.S. Army Engineer Research and Development Center",
               "type" : "parent"
            }
         ],
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
         "established" : 1946,
         "external_ids" : [
            {
               "all" : [
                  "grid.424661.3"
               ],
               "preferred" : "grid.424661.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2173 3068"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/05y503v71",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.inra.org.ma/accueil1.asp?codelangue=23&po=2"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Institut_national_de_la_recherche_agronomique"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "MA",
                  "country_name" : "Morocco",
                  "country_subdivision_code" : "04",
                  "country_subdivision_name" : "Rabat-Salé-Kénitra",
                  "lat" : 34.01325,
                  "lng" : -6.83255,
                  "name" : "Rabat"
               },
               "geonames_id" : 2538475
            }
         ],
         "names" : [
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
                  "ror_display",
                  "label"
               ],
               "value" : "Institut National de la Recherche Agronomique"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/028cef883",
               "label" : "Laboratoire Chrono-Environnement",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/056ms9a36",
               "label" : "GEVES",
               "type" : "child"
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
         "established" : 1995,
         "external_ids" : [
            {
               "all" : [
                  "grid.461864.9"
               ],
               "preferred" : "grid.461864.9",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 8523 0913"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/04fhrs205",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.cerco.ups-tlse.fr"
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
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Brain and Cognition Research Center"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CERCO"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Centre de recherche cerveau et cognition"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00rydyx93",
               "label" : "Institut des Sciences Biologiques",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02v6kpv12",
               "label" : "Université Toulouse III - Paul Sabatier",
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
               "date" : "2023-12-07",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2014,
         "external_ids" : [
            {
               "all" : [
                  "Q51784461"
               ],
               "preferred" : "Q51784461",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02y2c2646",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ibps.sorbonne-universite.fr/fr/Recherche/umr-8256"
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
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Adaptation Biologique et Vieillissement"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "B2A"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02feahw73",
               "label" : "Centre National de la Recherche Scientifique",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02vjkv261",
               "label" : "Inserm",
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
            "count" : 6465,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 2496,
            "id" : "as",
            "title" : "Asia"
         },
         {
            "count" : 1621,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 328,
            "id" : "sa",
            "title" : "South America"
         },
         {
            "count" : 286,
            "id" : "af",
            "title" : "Africa"
         },
         {
            "count" : 240,
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
            "count" : 2241,
            "id" : "fr",
            "title" : "France"
         },
         {
            "count" : 1252,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 1239,
            "id" : "cn",
            "title" : "China"
         },
         {
            "count" : 812,
            "id" : "ru",
            "title" : "Russia"
         },
         {
            "count" : 777,
            "id" : "de",
            "title" : "Germany"
         },
         {
            "count" : 383,
            "id" : "in",
            "title" : "India"
         },
         {
            "count" : 344,
            "id" : "es",
            "title" : "Spain"
         },
         {
            "count" : 329,
            "id" : "it",
            "title" : "Italy"
         },
         {
            "count" : 307,
            "id" : "jp",
            "title" : "Japan"
         },
         {
            "count" : 305,
            "id" : "gb",
            "title" : "United Kingdom"
         }
      ],
      "statuses" : [
         {
            "count" : 11437,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 11437,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 1613,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 139,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 18,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 2,
            "id" : "archive",
            "title" : "archive"
         },
         {
            "count" : 1,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 1,
            "id" : "nonprofit",
            "title" : "nonprofit"
         }
      ]
   },
   "number_of_results" : 11437,
   "time_taken" : 5
}
```

# Filter by country code

Available country codes are those in the [ISO 3166 alpha-2 list](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).

## Example

```curl
curl 'https://api.ror.org/v2/organizations?filter=country.country_code:ZA' | json_pp
```

Returns a list of research organizations in South Africa. 

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
         "established" : 1879,
         "external_ids" : [
            {
               "all" : [
                  "grid.413302.7"
               ],
               "preferred" : "grid.413302.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0044 1330"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30254227"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01vx7zt03",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.kznhealth.gov.za/addingtonhospital.htm"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "KZN",
                  "country_subdivision_name" : "KwaZulu-Natal",
                  "lat" : -29.8579,
                  "lng" : 31.0292,
                  "name" : "Durban"
               },
               "geonames_id" : 1007311
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Addington Hospital"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "The Bayside Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04qzfn040",
               "label" : "University of KwaZulu-Natal",
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
         "established" : 1855,
         "external_ids" : [
            {
               "all" : [
                  "grid.413331.7"
               ],
               "preferred" : "grid.413331.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0635 1477"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30254236"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00xnmcq32",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.kznhealth.gov.za/greyshospital.htm"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "KZN",
                  "country_subdivision_name" : "KwaZulu-Natal",
                  "lat" : -29.61679,
                  "lng" : 30.39278,
                  "name" : "Pietermaritzburg"
               },
               "geonames_id" : 965301
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Grey's Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04qzfn040",
               "label" : "University of KwaZulu-Natal",
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
         "established" : 1938,
         "external_ids" : [
            {
               "all" : [
                  "grid.413335.3"
               ],
               "preferred" : "grid.413335.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0635 1506"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q368400"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00c879s84",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.westerncape.gov.za/your_gov/163"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Groote_Schuur_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "WC",
                  "country_subdivision_name" : "Western Cape",
                  "lat" : -33.92584,
                  "lng" : 18.42322,
                  "name" : "Cape Town"
               },
               "geonames_id" : 3369157
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Groote Schuur Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02nys7898",
               "label" : "Western Cape Department of Health",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03p74gp79",
               "label" : "University of Cape Town",
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
         "established" : 1884,
         "external_ids" : [
            {
               "all" : [
                  "grid.413821.d"
               ],
               "preferred" : "grid.413821.d",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/000vg5909",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.samj.org.za/index.php/samj"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "WC",
                  "country_subdivision_name" : "Western Cape",
                  "lat" : -33.92584,
                  "lng" : 18.42322,
                  "name" : "Cape Town"
               },
               "geonames_id" : 3369157
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Health & Medical Publishing Group"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "SAMJ"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "South African Medical Journal"
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
                  "grid.414087.e"
               ],
               "preferred" : "grid.414087.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0635 7844"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/01tcy5w98",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.auruminstitute.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "GP",
                  "country_subdivision_name" : "Gauteng",
                  "lat" : -26.20227,
                  "lng" : 28.04363,
                  "name" : "Johannesburg"
               },
               "geonames_id" : 993800
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Aurum Institute"
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
         "established" : 1999,
         "external_ids" : [
            {
               "all" : [
                  "grid.414191.8"
               ],
               "preferred" : "grid.414191.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30279920"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04ap0ew90",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.kznhealth.gov.za/hlabisahospital.htm"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "KZN",
                  "country_subdivision_name" : "KwaZulu-Natal",
                  "lat" : -28.14519,
                  "lng" : 31.87719,
                  "name" : "Hlabisa"
               },
               "geonames_id" : 996377
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Hlabisa Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04qzfn040",
               "label" : "University of KwaZulu-Natal",
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
         "established" : 1941,
         "external_ids" : [
            {
               "all" : [
                  "grid.414240.7"
               ],
               "preferred" : "grid.414240.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0367 6954"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1077277"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02g48bh60",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.chrishanibaragwanathhospital.co.za/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Chris_Hani_Baragwanath_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "GP",
                  "country_subdivision_name" : "Gauteng",
                  "lat" : -26.20227,
                  "lng" : 28.04363,
                  "name" : "Johannesburg"
               },
               "geonames_id" : 993800
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CHBH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Chris Hani Baragwanath Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03rp50x72",
               "label" : "University of the Witwatersrand",
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
         "established" : 1954,
         "external_ids" : [
            {
               "all" : [
                  "grid.414386.c"
               ],
               "preferred" : "grid.414386.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0576 7753"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5336985"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03gvpk069",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.kznhealth.gov.za/edendalehospital.htm"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Edendale_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "KZN",
                  "country_subdivision_name" : "KwaZulu-Natal",
                  "lat" : -29.61679,
                  "lng" : 30.39278,
                  "name" : "Pietermaritzburg"
               },
               "geonames_id" : 965301
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Edendale Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04qzfn040",
               "label" : "University of KwaZulu-Natal",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.414707.1"
               ],
               "preferred" : "grid.414707.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0364 9292"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5086067"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/047x96110",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.johannesburghospital.org.za/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Charlotte_Maxeke_Johannesburg_Academic_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "GP",
                  "country_subdivision_name" : "Gauteng",
                  "lat" : -26.20227,
                  "lng" : 28.04363,
                  "name" : "Johannesburg"
               },
               "geonames_id" : 993800
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Charlotte Maxeke Johannesburg Academic Hospital"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Johannesburg General Hospital"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Johannesburg Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03rp50x72",
               "label" : "University of the Witwatersrand",
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
         "established" : 1969,
         "external_ids" : [
            {
               "all" : [
                  "501100001322"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.415021.3"
               ],
               "preferred" : "grid.415021.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9155 0024"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7565733"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05q60vz69",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.mrc.ac.za/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/South_African_Medical_Research_Council"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "WC",
                  "country_subdivision_name" : "Western Cape",
                  "lat" : -33.92584,
                  "lng" : 18.42322,
                  "name" : "Cape Town"
               },
               "geonames_id" : 3369157
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "SAMRC"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "South African Medical Research Council"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
            "other"
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
         "established" : 1881,
         "external_ids" : [
            {
               "all" : [
                  "grid.415061.7"
               ],
               "preferred" : "grid.415061.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 8669 9369"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5502892"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/044sjfg03",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.fh.echealth.gov.za/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Frere_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "EC",
                  "country_subdivision_name" : "Eastern Cape",
                  "lat" : -33.01529,
                  "lng" : 27.91162,
                  "name" : "East London"
               },
               "geonames_id" : 1006984
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Frere Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02svzjn28",
               "label" : "Walter Sisulu University",
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
         "established" : 1936,
         "external_ids" : [
            {
               "all" : [
                  "grid.415293.8"
               ],
               "preferred" : "grid.415293.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0383 9602"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30280419"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03r56rv89",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.kznhealth.gov.za/kingedwardhospital.htm"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "KZN",
                  "country_subdivision_name" : "KwaZulu-Natal",
                  "lat" : -29.8579,
                  "lng" : 31.0292,
                  "name" : "Durban"
               },
               "geonames_id" : 1007311
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "KEH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "King Edward VIII Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04qzfn040",
               "label" : "University of KwaZulu-Natal",
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
         "established" : 1967,
         "external_ids" : [
            {
               "all" : [
                  "grid.415447.7"
               ],
               "preferred" : "grid.415447.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30280483"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04nrs5002",
         "links" : [
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Helen_Joseph_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "GP",
                  "country_subdivision_name" : "Gauteng",
                  "lat" : -26.20227,
                  "lng" : 28.04363,
                  "name" : "Johannesburg"
               },
               "geonames_id" : 993800
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Helen Joseph Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03rp50x72",
               "label" : "University of the Witwatersrand",
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
         "established" : 1996,
         "external_ids" : [
            {
               "all" : [
                  "grid.415523.4"
               ],
               "preferred" : "grid.415523.4",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/00f9c2f73",
         "links" : [],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "WC",
                  "country_subdivision_name" : "Western Cape",
                  "lat" : -33.92584,
                  "lng" : 18.42322,
                  "name" : "Cape Town"
               },
               "geonames_id" : 3369157
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "GF Jooste Hospital"
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
         "established" : 1969,
         "external_ids" : [
            {
               "all" : [
                  "grid.415623.7"
               ],
               "preferred" : "grid.415623.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0576 6654"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30280568"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04xz2c768",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.kznhealth.gov.za/rkkhanhospital.htm"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "KZN",
                  "country_subdivision_name" : "KwaZulu-Natal",
                  "lat" : -29.8579,
                  "lng" : 31.0292,
                  "name" : "Durban"
               },
               "geonames_id" : 1007311
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "R. K. Khan Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04qzfn040",
               "label" : "University of KwaZulu-Natal",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.415666.6"
               ],
               "preferred" : "grid.415666.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0634 9246"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q6860699"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03wwnw466",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.netcare.co.za/live/netcare_content.php?Item_ID=5585"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Milpark_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "GP",
                  "country_subdivision_name" : "Gauteng",
                  "lat" : -26.20227,
                  "lng" : 28.04363,
                  "name" : "Johannesburg"
               },
               "geonames_id" : 993800
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Milpark Hospital"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Netcare Milpark Hospital"
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
         "established" : 1956,
         "external_ids" : [
            {
               "all" : [
                  "grid.415742.1"
               ],
               "preferred" : "grid.415742.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2296 3850"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7303965"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04d6eav07",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.westerncape.gov.za/your_gov/149"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Red_Cross_War_Memorial_Children%27s_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "WC",
                  "country_subdivision_name" : "Western Cape",
                  "lat" : -33.92584,
                  "lng" : 18.42322,
                  "name" : "Cape Town"
               },
               "geonames_id" : 3369157
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Red Cross War Memorial Children's Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02nys7898",
               "label" : "Western Cape Department of Health",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05bk57929",
               "label" : "Stellenbosch University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03p74gp79",
               "label" : "University of Cape Town",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00h2vm590",
               "label" : "University of the Western Cape",
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
         "established" : 1912,
         "external_ids" : [
            {
               "all" : [
                  "grid.416148.e"
               ],
               "preferred" : "grid.416148.e",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/05q7jp496",
         "links" : [],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "GP",
                  "country_subdivision_name" : "Gauteng",
                  "lat" : -26.20227,
                  "lng" : 28.04363,
                  "name" : "Johannesburg"
               },
               "geonames_id" : 993800
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "SAIMR"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "South African Institute for Medical Research"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00znvbk37",
               "label" : "National Health Laboratory Service",
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
         "established" : 1950,
         "external_ids" : [
            {
               "all" : [
                  "grid.416583.d"
               ],
               "preferred" : "grid.416583.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0635 2963"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/04hzm4679",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.nioh.ac.za/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "GP",
                  "country_subdivision_name" : "Gauteng",
                  "lat" : -26.20227,
                  "lng" : 28.04363,
                  "name" : "Johannesburg"
               },
               "geonames_id" : 993800
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "NIOH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "National Institute for Occupational Health"
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
                  "grid.416734.3"
               ],
               "preferred" : "grid.416734.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0400 8421"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/05g42cb02",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.netcare.co.za/71/netcare-sunninghill-hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "GP",
                  "country_subdivision_name" : "Gauteng",
                  "lat" : -26.20227,
                  "lng" : 28.04363,
                  "name" : "Johannesburg"
               },
               "geonames_id" : 993800
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Netcare Sunninghill Hospital"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 515,
            "id" : "af",
            "title" : "Africa"
         }
      ],
      "countries" : [
         {
            "count" : 515,
            "id" : "za",
            "title" : "South Africa"
         }
      ],
      "statuses" : [
         {
            "count" : 515,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 132,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 118,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 94,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 62,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 60,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 59,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 40,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 35,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 10,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 515,
   "time_taken" : 2
}
```

# Filter by country name

Available country names are those [provided by Geonames](https://www.geonames.org/countries/).

## Example

```curl
curl 'https://api.ror.org/v2/organizations?filter=locations.geonames_details.country_name:Djibouti' | json_pp
```

Returns a list of research organizations in Djibouti. 

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
         "established" : 2006,
         "external_ids" : [
            {
               "all" : [
                  "grid.449656.c"
               ],
               "preferred" : "grid.449656.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 8895 9179"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3551582"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04wk53b95",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.univ.edu.dj/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/University_of_Djibouti"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "DJ",
                  "country_name" : "Djibouti",
                  "country_subdivision_code" : "DJ",
                  "country_subdivision_name" : "Djibouti",
                  "lat" : 11.58901,
                  "lng" : 43.14503,
                  "name" : "Djibouti"
               },
               "geonames_id" : 223817
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UD"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Djibouti"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Université de djibouti"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.473221.1"
               ],
               "preferred" : "grid.473221.1",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/045rh7r61",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.cerd.dj/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "DJ",
                  "country_name" : "Djibouti",
                  "country_subdivision_code" : "DJ",
                  "country_subdivision_name" : "Djibouti",
                  "lat" : 11.58901,
                  "lng" : 43.14503,
                  "name" : "Djibouti"
               },
               "geonames_id" : 223817
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CERD"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Centre d'Étude et de Recherche de Djibouti"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "facility"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 2,
            "id" : "af",
            "title" : "Africa"
         }
      ],
      "countries" : [
         {
            "count" : 2,
            "id" : "dj",
            "title" : "Djibouti"
         }
      ],
      "statuses" : [
         {
            "count" : 2,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 1,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 1,
            "id" : "facility",
            "title" : "facility"
         }
      ]
   },
   "number_of_results" : 2,
   "time_taken" : 1
}
```

# Filter by continent code

Continent codes and names are provided by [GeoNames](https://geonames.org): AF (Africa), AN (Antarctica), AS (Asia), EU (Europe), NA (North America), OC (Oceania), and SA (South America).  

## Example

```curl
curl 'https://api.ror.org/v2/organizations?filter=locations.geonames_details.continent_code:AF' | json_pp
```

The response returns a list of research organizations in Africa. 

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
         "established" : 1964,
         "external_ids" : [
            {
               "all" : [
                  "grid.7749.d"
               ],
               "preferred" : "grid.7749.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0723 7738"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q719925"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/003vfy751",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.ub.edu.bi/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/University_of_Burundi"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "BI",
                  "country_name" : "Burundi",
                  "country_subdivision_code" : "BM",
                  "country_subdivision_name" : "Bujumbura Mairie",
                  "lat" : -3.38193,
                  "lng" : 29.36142,
                  "name" : "Bujumbura"
               },
               "geonames_id" : 425378
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "University of Burundi"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Université du Burundi"
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
         "established" : 1908,
         "external_ids" : [
            {
               "all" : [
                  "501100002386",
                  "501100002387",
                  "501100002375",
                  "501100002377",
                  "501100002378",
                  "501100006205"
               ],
               "preferred" : "501100002386",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.7776.1"
               ],
               "preferred" : "grid.7776.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0639 9286"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q194445"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03q21mh05",
         "links" : [
            {
               "type" : "website",
               "value" : "http://cu.edu.eg/Home"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Cairo_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "EG",
                  "country_name" : "Egypt",
                  "country_subdivision_code" : "GZ",
                  "country_subdivision_name" : "Giza",
                  "lat" : 30.00944,
                  "lng" : 31.20861,
                  "name" : "Giza"
               },
               "geonames_id" : 360995
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cairo University"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "King Fuad I University"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Université du Caire"
            },
            {
               "lang" : "ar",
               "types" : [
                  "label"
               ],
               "value" : "جامعة القاهرة"
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
         "established" : 1957,
         "external_ids" : [
            {
               "all" : [
                  "501100005757"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.8191.1"
               ],
               "preferred" : "grid.8191.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2186 9619"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q671363"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04je6yw13",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.ucad.sn/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Cheikh_Anta_Diop_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cheikh Anta Diop University"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UCAD"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "University of Dakar"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Université Cheikh Anta Diop"
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
         "established" : 1962,
         "external_ids" : [
            {
               "all" : [
                  "grid.8295.6"
               ],
               "preferred" : "grid.8295.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0943 5818"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q890884"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05n8n9378",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.uem.mz/index.php"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Eduardo_Mondlane_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "MZ",
                  "country_name" : "Mozambique",
                  "country_subdivision_code" : "MPM",
                  "country_subdivision_name" : "Maputo City",
                  "lat" : -25.96553,
                  "lng" : 32.58322,
                  "name" : "Maputo"
               },
               "geonames_id" : 1040652
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Eduardo Mondlane University"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UEM"
            },
            {
               "lang" : "pt",
               "types" : [
                  "label"
               ],
               "value" : "Universidade Eduardo Mondlane"
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
         "established" : 1997,
         "external_ids" : [
            {
               "all" : [
                  "grid.412898.e"
               ],
               "preferred" : "grid.412898.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0648 0439"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q2459595"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0511zqc76",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.makumira.ac.tz/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Tumaini_University_Makumira"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "TZ",
                  "country_name" : "Tanzania",
                  "country_subdivision_code" : "01",
                  "country_subdivision_name" : "Arusha",
                  "lat" : -3.36667,
                  "lng" : 36.68333,
                  "name" : "Arusha"
               },
               "geonames_id" : 161325
            }
         ],
         "names" : [
            {
               "lang" : "sw",
               "types" : [
                  "label"
               ],
               "value" : "Chuo Kikuu cha Tumaini"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Lutheran Theological College Makumira"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "TUMA"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Tumaini University"
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
         "established" : 1994,
         "external_ids" : [
            {
               "all" : [
                  "grid.412962.a"
               ],
               "preferred" : "grid.412962.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1764 9404"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30254141"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03fr85h91",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.uuthuyo.net/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/University_of_Uyo_Teaching_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "NG",
                  "country_name" : "Nigeria",
                  "country_subdivision_code" : "AK",
                  "country_subdivision_name" : "Akwa Ibom State",
                  "lat" : 5.05127,
                  "lng" : 7.9335,
                  "name" : "Uyo"
               },
               "geonames_id" : 2319480
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UUTH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Uyo Teaching Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0127mpp72",
               "label" : "University of Uyo",
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
         "established" : 1980,
         "external_ids" : [
            {
               "all" : [
                  "grid.412975.c"
               ],
               "preferred" : "grid.412975.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 8878 5287"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/045vatr18",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.uith.org/#"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "NG",
                  "country_name" : "Nigeria",
                  "country_subdivision_code" : "KW",
                  "country_subdivision_name" : "Kwara State",
                  "lat" : 8.49664,
                  "lng" : 4.54214,
                  "name" : "Ilorin"
               },
               "geonames_id" : 2337639
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UITH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Ilorin Teaching Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/032kdwk38",
               "label" : "University of Ilorin",
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
         "established" : 1975,
         "external_ids" : [
            {
               "all" : [
                  "grid.413017.0"
               ],
               "preferred" : "grid.413017.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9001 9645"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3509668"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/016na8197",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.unimaid.edu.ng/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/University_of_Maiduguri"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "NG",
                  "country_name" : "Nigeria",
                  "country_subdivision_code" : "BO",
                  "country_subdivision_name" : "Borno State",
                  "lat" : 11.84692,
                  "lng" : 13.15712,
                  "name" : "Maiduguri"
               },
               "geonames_id" : 2331447
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UNIMAID"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Maiduguri"
            },
            {
               "lang" : "yo",
               "types" : [
                  "label"
               ],
               "value" : "Yunifásítì ìlú Màídúgùri"
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
         "established" : 1973,
         "external_ids" : [
            {
               "all" : [
                  "grid.413070.1"
               ],
               "preferred" : "grid.413070.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0806 7267"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30254157"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01hhczc28",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.ubth.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/University_of_Benin_Teaching_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "NG",
                  "country_name" : "Nigeria",
                  "country_subdivision_code" : "ED",
                  "country_subdivision_name" : "Edo State",
                  "lat" : 6.33815,
                  "lng" : 5.62575,
                  "name" : "Benin City"
               },
               "geonames_id" : 2347283
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UBTH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Benin Teaching Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04mznrw11",
               "label" : "University of Benin",
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
         "established" : 1975,
         "external_ids" : [
            {
               "all" : [
                  "grid.413097.8"
               ],
               "preferred" : "grid.413097.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0291 6387"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3509289"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05qderh61",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.unical.edu.ng/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/University_of_Calabar"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "NG",
                  "country_name" : "Nigeria",
                  "country_subdivision_code" : "CR",
                  "country_subdivision_name" : "Cross River State",
                  "lat" : 4.95893,
                  "lng" : 8.32695,
                  "name" : "Calabar"
               },
               "geonames_id" : 2346229
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UNICAL"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Calabar"
            },
            {
               "lang" : "yo",
               "types" : [
                  "label"
               ],
               "value" : "Yunifásítì ìlú Calabar"
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
         "established" : 1971,
         "external_ids" : [
            {
               "all" : [
                  "grid.413123.6"
               ],
               "preferred" : "grid.413123.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0455 9733"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/05h7pem82",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.bugandomedicalcentre.go.tz/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "TZ",
                  "country_name" : "Tanzania",
                  "country_subdivision_code" : "18",
                  "country_subdivision_name" : "Mwanza",
                  "lat" : -2.51667,
                  "lng" : 32.9,
                  "name" : "Mwanza"
               },
               "geonames_id" : 152224
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BMC"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bugando Medical Centre"
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
         "established" : 2007,
         "external_ids" : [
            {
               "all" : [
                  "grid.413131.5"
               ],
               "preferred" : "grid.413131.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9161 1296"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/05fx5mz56",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.unthportal.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "NG",
                  "country_name" : "Nigeria",
                  "country_subdivision_code" : "EN",
                  "country_subdivision_name" : "Enugu State",
                  "lat" : 6.44132,
                  "lng" : 7.49883,
                  "name" : "Enugu"
               },
               "geonames_id" : 2343279
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UNTH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Nigeria Teaching Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01sn1yx84",
               "label" : "University of Nigeria",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.413207.3"
               ],
               "preferred" : "grid.413207.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q12242646"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04w40b524",
         "links" : [],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "TN",
                  "country_name" : "Tunisia",
                  "country_subdivision_code" : "12",
                  "country_subdivision_name" : "Ariana Governorate",
                  "lat" : 36.86012,
                  "lng" : 10.19337,
                  "name" : "Aryanah"
               },
               "geonames_id" : 2473247
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Abderrahmane Mami Hospital"
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
         "established" : 1968,
         "external_ids" : [
            {
               "all" : [
                  "grid.413221.7"
               ],
               "preferred" : "grid.413221.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 4688 7583"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30254204"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03237y496",
         "links" : [
            {
               "type" : "website",
               "value" : "http://abuth.org.ng/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "NG",
                  "country_name" : "Nigeria",
                  "country_subdivision_code" : "KD",
                  "country_subdivision_name" : "Kaduna State",
                  "lat" : 11.11128,
                  "lng" : 7.7227,
                  "name" : "Zaria"
               },
               "geonames_id" : 2317765
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ABUTH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ahmadu Bello University Teaching Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/019apvn83",
               "label" : "Ahmadu Bello University",
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
         "established" : 1879,
         "external_ids" : [
            {
               "all" : [
                  "grid.413302.7"
               ],
               "preferred" : "grid.413302.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0044 1330"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30254227"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01vx7zt03",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.kznhealth.gov.za/addingtonhospital.htm"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "KZN",
                  "country_subdivision_name" : "KwaZulu-Natal",
                  "lat" : -29.8579,
                  "lng" : 31.0292,
                  "name" : "Durban"
               },
               "geonames_id" : 1007311
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Addington Hospital"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "The Bayside Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04qzfn040",
               "label" : "University of KwaZulu-Natal",
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
         "established" : 1855,
         "external_ids" : [
            {
               "all" : [
                  "grid.413331.7"
               ],
               "preferred" : "grid.413331.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0635 1477"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30254236"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00xnmcq32",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.kznhealth.gov.za/greyshospital.htm"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "KZN",
                  "country_subdivision_name" : "KwaZulu-Natal",
                  "lat" : -29.61679,
                  "lng" : 30.39278,
                  "name" : "Pietermaritzburg"
               },
               "geonames_id" : 965301
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Grey's Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04qzfn040",
               "label" : "University of KwaZulu-Natal",
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
         "established" : 1938,
         "external_ids" : [
            {
               "all" : [
                  "grid.413335.3"
               ],
               "preferred" : "grid.413335.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0635 1506"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q368400"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00c879s84",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.westerncape.gov.za/your_gov/163"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Groote_Schuur_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "WC",
                  "country_subdivision_name" : "Western Cape",
                  "lat" : -33.92584,
                  "lng" : 18.42322,
                  "name" : "Cape Town"
               },
               "geonames_id" : 3369157
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Groote Schuur Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02nys7898",
               "label" : "Western Cape Department of Health",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03p74gp79",
               "label" : "University of Cape Town",
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
         "established" : 1957,
         "external_ids" : [
            {
               "all" : [
                  "grid.413353.3"
               ],
               "preferred" : "grid.413353.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0621 4210"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q384103"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00qpv3w06",
         "links" : [
            {
               "type" : "website",
               "value" : "http://amref.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Amref_Health_Africa"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "KE",
                  "country_name" : "Kenya",
                  "country_subdivision_code" : "30",
                  "country_subdivision_name" : "Nairobi County",
                  "lat" : -1.28333,
                  "lng" : 36.81667,
                  "name" : "Nairobi"
               },
               "geonames_id" : 184745
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "African Medical and Research Foundation"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "Amref"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Amref Health Africa"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.413399.5"
               ],
               "preferred" : "grid.413399.5",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04sz7qx51",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.chisrabat.ma/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "MA",
                  "country_name" : "Morocco",
                  "country_subdivision_code" : "04",
                  "country_subdivision_name" : "Rabat-Salé-Kénitra",
                  "lat" : 34.0531,
                  "lng" : -6.79846,
                  "name" : "Salé"
               },
               "geonames_id" : 2537763
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Centre Hospitalier Universitaire de Rabat-Salé"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 3640,
            "id" : "af",
            "title" : "Africa"
         }
      ],
      "countries" : [
         {
            "count" : 515,
            "id" : "za",
            "title" : "South Africa"
         },
         {
            "count" : 353,
            "id" : "ng",
            "title" : "Nigeria"
         },
         {
            "count" : 270,
            "id" : "ke",
            "title" : "Kenya"
         },
         {
            "count" : 239,
            "id" : "eg",
            "title" : "Egypt"
         },
         {
            "count" : 222,
            "id" : "ug",
            "title" : "Uganda"
         },
         {
            "count" : 191,
            "id" : "gh",
            "title" : "Ghana"
         },
         {
            "count" : 190,
            "id" : "ma",
            "title" : "Morocco"
         },
         {
            "count" : 147,
            "id" : "tn",
            "title" : "Tunisia"
         },
         {
            "count" : 141,
            "id" : "tz",
            "title" : "Tanzania"
         },
         {
            "count" : 131,
            "id" : "dz",
            "title" : "Algeria"
         }
      ],
      "statuses" : [
         {
            "count" : 3640,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 1478,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 599,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 452,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 366,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 339,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 286,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 270,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 146,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 45,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 3640,
   "time_taken" : 4
}
```

# Filter by continent name

Continent codes and names are provided by [GeoNames](https://geonames.org): AF (Africa), AN (Antarctica), AS (Asia), EU (Europe), NA (North America), OC (Oceania), and SA (South America).

## Example

```curl
curl 'https://api.ror.org/v2/organizations?filter=locations.geonames_details.continent_name:South+America' | json_pp
```

The response is a list of research organizations in South America.

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
         "established" : 2006,
         "external_ids" : [
            {
               "all" : [
                  "501100016217"
               ],
               "preferred" : "501100016217",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.418532.9"
               ],
               "preferred" : "grid.418532.9",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0403 6035"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5917646"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04dpm2z73",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.pasteur.edu.uy/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "UY",
                  "country_name" : "Uruguay",
                  "country_subdivision_code" : "MO",
                  "country_subdivision_name" : "Montevideo Department",
                  "lat" : -34.90328,
                  "lng" : -56.18816,
                  "name" : "Montevideo"
               },
               "geonames_id" : 3441575
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "alias"
               ],
               "value" : "IP Montevideo"
            },
            {
               "lang" : "es",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut Pasteur de Montevideo"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
               "type" : "related"
            }
         ],
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
         "domains" : [
            "fumc.edu.co"
         ],
         "established" : 1987,
         "external_ids" : [
            {
               "all" : [
                  "grid.441812.a"
               ],
               "preferred" : "grid.441812.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0415 8286"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5871656"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03cvv1m29",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.fumc.edu.co"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "CO",
                  "country_name" : "Colombia",
                  "country_subdivision_code" : "ANT",
                  "country_subdivision_name" : "Antioquia",
                  "lat" : 6.25184,
                  "lng" : -75.56359,
                  "name" : "Medellín"
               },
               "geonames_id" : 3674962
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Fundación Universitaria María Cano"
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
               "date" : "2024-07-08",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "aesga.edu.br"
         ],
         "established" : 1985,
         "external_ids" : [],
         "id" : "https://ror.org/02pt9a543",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.aesga.edu.br"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "PE",
                  "country_subdivision_name" : "Pernambuco",
                  "lat" : -8.9308,
                  "lng" : -36.50767,
                  "name" : "Garanhuns"
               },
               "geonames_id" : 6320507
            }
         ],
         "names" : [
            {
               "lang" : "pt",
               "types" : [
                  "acronym"
               ],
               "value" : "AESGA"
            },
            {
               "lang" : "pt",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Autarquia de Ensino Superior de Garanhuns"
            },
            {
               "lang" : "pt",
               "types" : [
                  "alias"
               ],
               "value" : "Autarquia de Ensino Superior de Garanhuns - AESGA"
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
         "established" : 1961,
         "external_ids" : [
            {
               "all" : [
                  "grid.441990.1"
               ],
               "preferred" : "grid.441990.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2226 7599"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5053268"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/027ryxs60",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ucsm.edu.pe/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Catholic_University_of_Santa_Mar%C3%ADa"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "PE",
                  "country_name" : "Peru",
                  "country_subdivision_code" : "ARE",
                  "country_subdivision_name" : "Arequipa",
                  "lat" : -16.39889,
                  "lng" : -71.535,
                  "name" : "Arequipa"
               },
               "geonames_id" : 3947322
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Catholic University of Santa María"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UCSM"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Universidad Católica de Santa María"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01j6ykv32",
               "label" : "Instituto de Investigación e Innovación en Energías Renovables y Medio Ambiente",
               "type" : "child"
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
               "date" : "2024-07-08",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "fatecsp.br"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "Q18477197"
               ],
               "preferred" : "Q18477197",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03jbcxj66",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.fatecsp.br"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SP",
                  "country_subdivision_name" : "São Paulo",
                  "lat" : -23.5475,
                  "lng" : -46.63611,
                  "name" : "São Paulo"
               },
               "geonames_id" : 3448439
            }
         ],
         "names" : [
            {
               "lang" : "pt",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Faculdade de Tecnologia de São Paulo"
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
               "date" : "2024-07-08",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "cataventoinstituto.com.br"
         ],
         "established" : 2021,
         "external_ids" : [],
         "id" : "https://ror.org/02zm1vy19",
         "links" : [
            {
               "type" : "website",
               "value" : "http://cataventoinstituto.com.br"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "BA",
                  "country_subdivision_name" : "Bahia",
                  "lat" : -12.97563,
                  "lng" : -38.49096,
                  "name" : "Salvador"
               },
               "geonames_id" : 3450554
            }
         ],
         "names" : [
            {
               "lang" : "pt",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Catavento Instituto"
            },
            {
               "lang" : "pt",
               "types" : [
                  "alias"
               ],
               "value" : "Catavento Instituto - Ensino para melhor assistência em Psicologia Hospitalar"
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
         "domains" : [
            "upla.cl"
         ],
         "established" : 1948,
         "external_ids" : [
            {
               "all" : [
                  "501100009547"
               ],
               "preferred" : "501100009547",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.441843.e"
               ],
               "preferred" : "grid.441843.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0694 2144"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q634259"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0171wr661",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.upla.cl"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Playa_Ancha_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "CL",
                  "country_name" : "Chile",
                  "country_subdivision_code" : "VS",
                  "country_subdivision_name" : "Valparaíso",
                  "lat" : -33.036,
                  "lng" : -71.62963,
                  "name" : "Valparaíso"
               },
               "geonames_id" : 3868626
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Playa Ancha University of Educational Sciences"
            },
            {
               "lang" : "es",
               "types" : [
                  "acronym"
               ],
               "value" : "UPLA"
            },
            {
               "lang" : "es",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Universidad de Playa Ancha de Ciencias de la Educación"
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
         "established" : 1982,
         "external_ids" : [
            {
               "all" : [
                  "grid.442021.1"
               ],
               "preferred" : "grid.442021.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0467 1725"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5917112"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00n4xrr40",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.unicesmag.edu.co"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "CO",
                  "country_name" : "Colombia",
                  "country_subdivision_code" : "NAR",
                  "country_subdivision_name" : "Nariño",
                  "lat" : 1.21361,
                  "lng" : -77.28111,
                  "name" : "Pasto"
               },
               "geonames_id" : 3672778
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IU CESMAG"
            },
            {
               "lang" : "es",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institución Universitaria Centro de Estudios Superiores María Goretti"
            },
            {
               "lang" : "es",
               "types" : [
                  "acronym"
               ],
               "value" : "UNICESMAG"
            },
            {
               "lang" : "es",
               "types" : [
                  "alias"
               ],
               "value" : "Universidad CESMAG"
            },
            {
               "lang" : "es",
               "types" : [
                  "alias"
               ],
               "value" : "Universidad Centro de Estudios Superiores María Goretti"
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
               "date" : "2024-07-08",
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
         "id" : "https://ror.org/00za68765",
         "links" : [
            {
               "type" : "website",
               "value" : "https://portal.estacio.br/unidades/centro-universit%C3%A1rio-est%C3%A1cio-de-santa-catarina/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SC",
                  "country_subdivision_name" : "Santa Catarina",
                  "lat" : -28.21344,
                  "lng" : -49.16383,
                  "name" : "São José"
               },
               "geonames_id" : 3448742
            }
         ],
         "names" : [
            {
               "lang" : "pt",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Centro Universitário Estácio de Santa Catarina"
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
         "domains" : [
            "ifpb.edu.br"
         ],
         "established" : 2008,
         "external_ids" : [
            {
               "all" : [
                  "100019080"
               ],
               "preferred" : "100019080",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.454344.6"
               ],
               "preferred" : "grid.454344.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9895 745X"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/01xc5jm57",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ifpb.edu.br"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "PB",
                  "country_subdivision_name" : "Paraíba",
                  "lat" : -7.115,
                  "lng" : -34.86306,
                  "name" : "João Pessoa"
               },
               "geonames_id" : 3397277
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CEFET-PB"
            },
            {
               "lang" : "pt",
               "types" : [
                  "acronym"
               ],
               "value" : "IFPB"
            },
            {
               "lang" : "pt",
               "types" : [
                  "alias"
               ],
               "value" : "Instituto Federal da Paraíba"
            },
            {
               "lang" : "pt",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Instituto Federal de Educação Ciência e Tecnologia da Paraíba"
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
         "domains" : [
            "isalud.edu.ar"
         ],
         "established" : 1991,
         "external_ids" : [
            {
               "all" : [
                  "grid.441634.0"
               ],
               "preferred" : "grid.441634.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 4690 323X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q29045289"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01sewfq88",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.isalud.edu.ar"
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
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "ISALUD University"
            },
            {
               "lang" : "es",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Universidad Isalud"
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
               "date" : "2024-07-08",
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
         "id" : "https://ror.org/01j6ykv32",
         "links" : [
            {
               "type" : "website",
               "value" : "https://investigacion.ucsm.edu.pe/instituto-innovergy/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "PE",
                  "country_name" : "Peru",
                  "country_subdivision_code" : "ARE",
                  "country_subdivision_name" : "Arequipa",
                  "lat" : -16.39889,
                  "lng" : -71.535,
                  "name" : "Arequipa"
               },
               "geonames_id" : 3947322
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Innovergy Institute"
            },
            {
               "lang" : "es",
               "types" : [
                  "alias"
               ],
               "value" : "Instituto Innovergy"
            },
            {
               "lang" : "es",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Instituto de Investigación e Innovación en Energías Renovables y Medio Ambiente"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Research Institute for Innovation in Renewable Energy and Environment"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/027ryxs60",
               "label" : "Catholic University of Santa María",
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
               "date" : "2024-07-08",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "anhanguera.com"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/04k6k8h87",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.anhanguera.com"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SP",
                  "country_subdivision_name" : "São Paulo",
                  "lat" : -23.02639,
                  "lng" : -45.55528,
                  "name" : "Taubaté"
               },
               "geonames_id" : 3446682
            }
         ],
         "names" : [
            {
               "lang" : "pt",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Faculdade Anhanguera"
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
               "date" : "2024-07-08",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "solca.med.ec"
         ],
         "established" : 1951,
         "external_ids" : [
            {
               "all" : [
                  "0000 0000 9905 0083"
               ],
               "preferred" : "0000 0000 9905 0083",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7551895"
               ],
               "preferred" : "Q7551895",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02wm3rq58",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.solca.med.ec"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Sociedad_de_Lucha_Contra_el_Cancer"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "EC",
                  "country_name" : "Ecuador",
                  "country_subdivision_code" : "G",
                  "country_subdivision_name" : "Guayas",
                  "lat" : -2.19616,
                  "lng" : -79.88621,
                  "name" : "Guayaquil"
               },
               "geonames_id" : 3657509
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "acronym"
               ],
               "value" : "SOLCA"
            },
            {
               "lang" : "es",
               "types" : [
                  "alias"
               ],
               "value" : "SOLCA - Guayaquil"
            },
            {
               "lang" : "es",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Sociedad de Lucha Contra el Cáncer del Ecuador"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Society to Fight Cancer"
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
         "established" : 1940,
         "external_ids" : [
            {
               "all" : [
                  "grid.418525.f"
               ],
               "preferred" : "grid.418525.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2206 8813"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30281759"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01fp8z436",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.pasteur-cayenne.fr/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "GF",
                  "country_name" : "French Guiana",
                  "country_subdivision_code" : null,
                  "country_subdivision_name" : "Guyane",
                  "lat" : 4.93333,
                  "lng" : -52.33333,
                  "name" : "Cayenne"
               },
               "geonames_id" : 3382160
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut Pasteur de la Guyane"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
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
               "date" : "2024-04-30",
               "schema_version" : "2.0"
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
                  "0000 0005 0955 754X"
               ],
               "preferred" : "0000 0005 0955 754X",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q4201403"
               ],
               "preferred" : "Q4201403",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/046fcjp93",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.saude.sp.gov.br/instituto-pasteur/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://pt.wikipedia.org/wiki/Instituto_Pasteur_(S%C3%A3o_Paulo)"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SP",
                  "country_subdivision_name" : "São Paulo",
                  "lat" : -23.5475,
                  "lng" : -46.63611,
                  "name" : "São Paulo"
               },
               "geonames_id" : 3448439
            }
         ],
         "names" : [
            {
               "lang" : "pt",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Instituto Pasteur"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Instituto Pasteur São Paulo"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Pasteur Institute"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Pasteur Institute São Paulo"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
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
         "domains" : [
            "uamerica.edu.co"
         ],
         "established" : 1956,
         "external_ids" : [
            {
               "all" : [
                  "grid.442159.f"
               ],
               "preferred" : "grid.442159.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0486 5407"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q11050122"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00kcjks51",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.uamerica.edu.co"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/University_of_America"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "CO",
                  "country_name" : "Colombia",
                  "country_subdivision_code" : "DC",
                  "country_subdivision_name" : "Bogota D.C.",
                  "lat" : 4.60971,
                  "lng" : -74.08175,
                  "name" : "Bogotá"
               },
               "geonames_id" : 3688689
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Fundación Universidad de América"
            },
            {
               "lang" : "es",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Universidad de América"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "alias"
               ],
               "value" : "University of America"
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
               "date" : "2024-07-08",
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
                  "501100013409"
               ],
               "preferred" : "501100013409",
               "type" : "fundref"
            },
            {
               "all" : [
                  "0000 0004 7777 4928"
               ],
               "preferred" : "0000 0004 7777 4928",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/05x7bg352",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.sgr.gov.co"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "CO",
                  "country_name" : "Colombia",
                  "country_subdivision_code" : "DC",
                  "country_subdivision_name" : "Bogota D.C.",
                  "lat" : 4.60971,
                  "lng" : -74.08175,
                  "name" : "Bogotá"
               },
               "geonames_id" : 3688689
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "General Royalties System"
            },
            {
               "lang" : "es",
               "types" : [
                  "acronym"
               ],
               "value" : "SGR"
            },
            {
               "lang" : "es",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Sistema General de Regalías de Colombia"
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
         "domains" : [
            "ces.edu.co"
         ],
         "established" : 1977,
         "external_ids" : [
            {
               "all" : [
                  "100018005"
               ],
               "preferred" : "100018005",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.411140.1"
               ],
               "preferred" : "grid.411140.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0812 5789"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q6156377"
               ],
               "preferred" : "Q6156377",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/037p13h95",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ces.edu.co"
            },
            {
               "type" : "wikipedia",
               "value" : "https://es.wikipedia.org/wiki/Universidad_CES"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "CO",
                  "country_name" : "Colombia",
                  "country_subdivision_code" : "ANT",
                  "country_subdivision_name" : "Antioquia",
                  "lat" : 6.25184,
                  "lng" : -75.56359,
                  "name" : "Medellín"
               },
               "geonames_id" : 3674962
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "CES University"
            },
            {
               "lang" : "es",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Universidad CES"
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
               "date" : "2024-07-08",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "eaesp.fgv.br"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "0000 0001 2301 4016"
               ],
               "preferred" : "0000 0001 2301 4016",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q10274560"
               ],
               "preferred" : "Q10274560",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02c0rrp38",
         "links" : [
            {
               "type" : "website",
               "value" : "https://eaesp.fgv.br"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Escola_de_Administra%C3%A7%C3%A3o_de_Empresas_de_S%C3%A3o_Paulo"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SP",
                  "country_subdivision_name" : "São Paulo",
                  "lat" : -23.5475,
                  "lng" : -46.63611,
                  "name" : "São Paulo"
               },
               "geonames_id" : 3448439
            }
         ],
         "names" : [
            {
               "lang" : "pt",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Escola de Administração de Empresas de São Paulo"
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
            "count" : 3446,
            "id" : "sa",
            "title" : "South America"
         }
      ],
      "countries" : [
         {
            "count" : 1860,
            "id" : "br",
            "title" : "Brazil"
         },
         {
            "count" : 390,
            "id" : "ar",
            "title" : "Argentina"
         },
         {
            "count" : 363,
            "id" : "co",
            "title" : "Colombia"
         },
         {
            "count" : 239,
            "id" : "cl",
            "title" : "Chile"
         },
         {
            "count" : 195,
            "id" : "pe",
            "title" : "Peru"
         },
         {
            "count" : 120,
            "id" : "ec",
            "title" : "Ecuador"
         },
         {
            "count" : 84,
            "id" : "ve",
            "title" : "Venezuela"
         },
         {
            "count" : 75,
            "id" : "bo",
            "title" : "Bolivia"
         },
         {
            "count" : 57,
            "id" : "uy",
            "title" : "Uruguay"
         },
         {
            "count" : 40,
            "id" : "py",
            "title" : "Paraguay"
         }
      ],
      "statuses" : [
         {
            "count" : 3446,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 1270,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 544,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 447,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 392,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 350,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 328,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 302,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 229,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 34,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 3446,
   "time_taken" : 4
}
```

# Combine filters

You can combine multiple filters in a single request.

> 📘 Format: Combined filters
> 
> `https://api.ror.org/v2/organizations?filter=<filter>:<value>,<filter>:<value>`

## Example

```curl
curl 'https://api.ror.org/v2/organizations?filter=status:inactive,status:withdrawn' | json_pp
```

Returns a list of both inactive and withdrawn records in ROR. 

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
         "established" : 1997,
         "external_ids" : [
            {
               "all" : [
                  "grid.434941.f"
               ],
               "preferred" : "grid.434941.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0507 1975"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/059zf1d45",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.iuct.com/en/"
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
                  "lat" : 41.55222,
                  "lng" : 2.20901,
                  "name" : "Mollet del Vallès"
               },
               "geonames_id" : 6356153
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IUCT"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut Universitari de Ciencia i Tecnologia (Spain)"
            }
         ],
         "relationships" : [],
         "status" : "inactive",
         "types" : [
            "company"
         ]
      },
      {
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
         "id" : "https://ror.org/03jsyxh43",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.sysard.com"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "CN",
                  "country_name" : "China",
                  "country_subdivision_code" : "LN",
                  "country_subdivision_name" : "Liaoning",
                  "lat" : 41.79222,
                  "lng" : 123.43278,
                  "name" : "Shenyang"
               },
               "geonames_id" : 2034937
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "Shenyang Sinochem Agrochemicals R&D Co."
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "Shenyang Sinochem Agrochemicals R&D Co., Ltd."
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Shenyang Sinochem Agrochemicals R&D Co., Ltd. (China)"
            },
            {
               "lang" : "zh",
               "types" : [
                  "label"
               ],
               "value" : "沈阳中化农药化工研发有限公司"
            }
         ],
         "relationships" : [],
         "status" : "withdrawn",
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
         "established" : 1974,
         "external_ids" : [
            {
               "all" : [
                  "grid.420100.7"
               ],
               "preferred" : "grid.420100.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2189 6043"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3578099"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04rfts775",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.enitiaa-nantes.fr/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/%C3%89cole_nationale_d%27ing%C3%A9nieurs_des_techniques_des_industries_agro-alimentaires"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "PDL",
                  "country_subdivision_name" : "Pays de la Loire",
                  "lat" : 47.21725,
                  "lng" : -1.55336,
                  "name" : "Nantes"
               },
               "geonames_id" : 2990969
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ENITIAA"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "École Nationale d'Ingénieurs des Techniques des Industries Agro-Alimentaires"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/029m96t80",
               "label" : "Ministère de l'Agriculture et de la Souveraineté alimentaire",
               "type" : "related"
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
               "date" : "2024-03-28",
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
                  "501100000330"
               ],
               "preferred" : "501100000330",
               "type" : "fundref"
            }
         ],
         "id" : "https://ror.org/01emdsc76",
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
                  "lat" : 51.50853,
                  "lng" : -0.12574,
                  "name" : "London"
               },
               "geonames_id" : 2643743
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Prostate Action"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04dkv6329",
               "label" : "Prostate Cancer UK",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.462526.1"
               ],
               "preferred" : "grid.462526.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0613 4851"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30261483"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05q94pf14",
         "links" : [
            {
               "type" : "website",
               "value" : "http://umr-lstm.cirad.fr/"
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
                  "lat" : 43.61093,
                  "lng" : 3.87635,
                  "name" : "Montpellier"
               },
               "geonames_id" : 2992166
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LSTM"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratoire des Symbioses Tropicales et Méditerranéennes"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05kpkpg04",
               "label" : "Centre de Coopération Internationale en Recherche Agronomique pour le Développement",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05q3vnk25",
               "label" : "Institut de Recherche pour le Développement",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03rnk6m14",
               "label" : "Institut Agro Montpellier",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/003vg9w96",
               "label" : "National Research Institute for Agriculture, Food and Environment",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/051escj72",
               "label" : "University of Montpellier",
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
               "date" : "2021-09-23",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2016,
         "external_ids" : [
            {
               "all" : [
                  "grid.512264.6"
               ],
               "preferred" : "grid.512264.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q21994834"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04x1a4s97",
         "links" : [
            {
               "type" : "website",
               "value" : "https://dircom.univ-rennes1.fr/UBL/"
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
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Université Bretagne Loire"
            }
         ],
         "relationships" : [],
         "status" : "inactive",
         "types" : [
            "education"
         ]
      },
      {
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
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/028ydjm20",
         "links" : [],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CO",
                  "country_subdivision_name" : "Colorado",
                  "lat" : 39.73915,
                  "lng" : -104.9847,
                  "name" : "Denver"
               },
               "geonames_id" : 5419384
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Argosy University"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Argosy University Denver"
            }
         ],
         "relationships" : [],
         "status" : "inactive",
         "types" : [
            "education"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2023-08-17",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2011,
         "external_ids" : [],
         "id" : "https://ror.org/01zc4nk92",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.diade-research.fr"
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
                  "lat" : 43.61093,
                  "lng" : 3.87635,
                  "name" : "Montpellier"
               },
               "geonames_id" : 2992166
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Diversité, adaptation, développement des plantes"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UMR DIADE"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05q3vnk25",
               "label" : "Institut de Recherche pour le Développement",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/051escj72",
               "label" : "University of Montpellier",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02banhz78",
               "label" : "Diversité, adaptation, développement des plantes",
               "type" : "successor"
            }
         ],
         "status" : "withdrawn",
         "types" : [
            "facility"
         ]
      },
      {
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
         "established" : 2004,
         "external_ids" : [],
         "id" : "https://ror.org/05rdqpb56",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.aimatmelanoma.org"
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
                  "lat" : 37.93576,
                  "lng" : -122.34775,
                  "name" : "Richmond"
               },
               "geonames_id" : 5387428
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "AIM at Melanoma"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02t5mzr26",
               "label" : "AIM at Melanoma Foundation",
               "type" : "successor"
            }
         ],
         "status" : "withdrawn",
         "types" : [
            "nonprofit"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2023-08-17",
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
                  "Q51780884"
               ],
               "preferred" : "Q51780884",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00j35ca78",
         "links" : [
            {
               "type" : "website",
               "value" : "https://nutripass.ird.fr"
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
                  "lat" : 43.61093,
                  "lng" : 3.87635,
                  "name" : "Montpellier"
               },
               "geonames_id" : 2992166
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "NutriPass"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Nutrition et Alimentation des Populations aux Suds"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05q3vnk25",
               "label" : "Institut de Recherche pour le Développement",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.464183.c"
               ],
               "preferred" : "grid.464183.c",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/03jhvx342",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ujf-grenoble.fr/laboratoire/biomedicale-et-neurosciences"
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
                  "ror_display",
                  "label"
               ],
               "value" : "RMN Biomédicale et Neurosciences"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04px4e658",
               "label" : "Délégation Alpes",
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
         "established" : 2001,
         "external_ids" : [
            {
               "all" : [
                  "grid.441437.1"
               ],
               "preferred" : "grid.441437.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0463 2136"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30293877"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/001p2e958",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.argosy.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Argosy_University"
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
                  "lat" : 33.78779,
                  "lng" : -117.85311,
                  "name" : "Orange"
               },
               "geonames_id" : 5379513
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Argosy University"
            }
         ],
         "relationships" : [],
         "status" : "inactive",
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
         "established" : 2012,
         "external_ids" : [
            {
               "all" : [
                  "grid.462870.f"
               ],
               "preferred" : "grid.462870.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1808 0475"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/02dg3n954",
         "links" : [
            {
               "type" : "website",
               "value" : "http://sites.univ-provence.fr/lnc/?lang=en"
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
               "value" : "LNC"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratoire de Neurosciences Cognitives"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/035xkbk20",
               "label" : "Aix-Marseille University",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/00rydyx93",
               "label" : "Institut des Sciences Biologiques",
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
               "date" : "2024-03-13",
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
                  "0000 0001 2326 7101"
               ],
               "preferred" : "0000 0001 2326 7101",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1779468"
               ],
               "preferred" : "Q1779468",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/034x1af55",
         "links" : [
            {
               "type" : "website",
               "value" : "http://uin.no"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/University_of_Nordland"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "NO",
                  "country_name" : "Norway",
                  "country_subdivision_code" : "18",
                  "country_subdivision_name" : "Nordland",
                  "lat" : 67.28325,
                  "lng" : 14.38319,
                  "name" : "Bodø"
               },
               "geonames_id" : 6453323
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Nordland"
            }
         ],
         "relationships" : [],
         "status" : "inactive",
         "types" : [
            "education"
         ]
      },
      {
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
         "established" : 1972,
         "external_ids" : [
            {
               "all" : [
                  "100012355"
               ],
               "preferred" : "100012355",
               "type" : "fundref"
            },
            {
               "all" : [
                  "0000 0001 1781 6696"
               ],
               "preferred" : "0000 0001 1781 6696",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/03cggs816",
         "links" : [
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/HECSU"
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
                  "lat" : 53.48095,
                  "lng" : -2.23743,
                  "name" : "Manchester"
               },
               "geonames_id" : 2643123
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HECSU"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Higher Education Careers Services Unit"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01rv9gx86",
               "label" : "Jisc",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.462337.7"
               ],
               "preferred" : "grid.462337.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0382 1904"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/01cmdpn82",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.institut-curie.org/recherche/signalisation-neurobiologie-cancer-institut-curie-cnrs-umr-3306-inserm-u1005"
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
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Signalisation, Neurobiologie et Cancer"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01y8j9r24",
               "label" : "Délégation Ile-de-France Sud",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02vjkv261",
               "label" : "Inserm",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/04t0gwh46",
               "label" : "Institute Curie",
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
               "date" : "2024-01-10",
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
         "id" : "https://ror.org/05dnzrj50",
         "links" : [
            {
               "type" : "website",
               "value" : "https://science.osti.gov/bes/mse"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "DC",
                  "country_subdivision_name" : "District of Columbia",
                  "lat" : 38.89511,
                  "lng" : -77.03637,
                  "name" : "Washington"
               },
               "geonames_id" : 4140963
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "DMSE"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Division of Materials Sciences and Engineering"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "MSE"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Materials Sciences and Engineering (MSE) Division"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Materials Sciences and Engineering Division"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/041m9xr71",
               "label" : "Ames National Laboratory",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05mg91w61",
               "label" : "Office of Basic Energy Sciences",
               "type" : "successor"
            }
         ],
         "status" : "withdrawn",
         "types" : [
            "government"
         ]
      },
      {
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
         "established" : 1989,
         "external_ids" : [],
         "id" : "https://ror.org/016gy9w96",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.education.govt.nz"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Ministry_of_Education_%28New_Zealand%29"
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
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Education"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Ministry of Education of New Zealand"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "New Zealand Ministry of Education"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05p70jq59",
               "label" : "Ministry of Education",
               "type" : "successor"
            }
         ],
         "status" : "withdrawn",
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
         "established" : 1924,
         "external_ids" : [
            {
               "all" : [
                  "grid.432648.f"
               ],
               "preferred" : "grid.432648.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30255354"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0294j1w75",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.bfec.us/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MD",
                  "country_subdivision_name" : "Maryland",
                  "lat" : 39.24038,
                  "lng" : -76.83942,
                  "name" : "Columbia"
               },
               "geonames_id" : 4352053
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bendix Field Engineering Corporation (United States)"
            }
         ],
         "relationships" : [],
         "status" : "inactive",
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
         "established" : 2005,
         "external_ids" : [
            {
               "all" : [
                  "grid.453215.4"
               ],
               "preferred" : "grid.453215.4",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/01pvzn627",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ontario.ca/page/ministry-economic-development-and-growth"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Ministry_of_Economic_Development,_Employment_and_Infrastructure"
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
                  "lat" : 43.70643,
                  "lng" : -79.39864,
                  "name" : "Toronto"
               },
               "geonames_id" : 6167865
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Economic Development, Employment and Infrastructure"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Ministère du Développement économique, de l'Emploi et de l'Infrastructure"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/039570836",
               "label" : "Ministry of Economic Development, Job Creation and Trade",
               "type" : "successor"
            }
         ],
         "status" : "withdrawn",
         "types" : [
            "government"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 994,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 681,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 227,
            "id" : "as",
            "title" : "Asia"
         },
         {
            "count" : 47,
            "id" : "oc",
            "title" : "Oceania"
         },
         {
            "count" : 39,
            "id" : "af",
            "title" : "Africa"
         },
         {
            "count" : 14,
            "id" : "sa",
            "title" : "South America"
         }
      ],
      "countries" : [
         {
            "count" : 620,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 232,
            "id" : "fr",
            "title" : "France"
         },
         {
            "count" : 180,
            "id" : "gb",
            "title" : "United Kingdom"
         },
         {
            "count" : 118,
            "id" : "de",
            "title" : "Germany"
         },
         {
            "count" : 76,
            "id" : "jp",
            "title" : "Japan"
         },
         {
            "count" : 67,
            "id" : "es",
            "title" : "Spain"
         },
         {
            "count" : 59,
            "id" : "ru",
            "title" : "Russia"
         },
         {
            "count" : 54,
            "id" : "ca",
            "title" : "Canada"
         },
         {
            "count" : 46,
            "id" : "cn",
            "title" : "China"
         },
         {
            "count" : 42,
            "id" : "au",
            "title" : "Australia"
         }
      ],
      "statuses" : [
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
            "count" : 577,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 522,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 313,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 186,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 167,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 152,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 151,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 99,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 16,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 2002,
   "time_taken" : 6
}
```

## Example

```curl
curl 'https://api.ror.org/v2/organizations?filter=types:facility,country.country_code:GB' | json_pp
```

Returns a list of research facilities in Great Britain. 

```json
{
   "items" : [
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
            "adams-institute.ac.uk"
         ],
         "established" : 2004,
         "external_ids" : [
            {
               "all" : [
                  "Q106485520"
               ],
               "preferred" : "Q106485520",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00wgpgb78",
         "links" : [
            {
               "type" : "website",
               "value" : "https://adams-institute.ac.uk"
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
            },
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "ENG",
                  "country_subdivision_name" : "England",
                  "lat" : 51.43158,
                  "lng" : -0.55239,
                  "name" : "Egham"
               },
               "geonames_id" : 2650188
            },
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "OH",
                  "country_subdivision_name" : "Ohio",
                  "lat" : 39.507,
                  "lng" : -84.74523,
                  "name" : "Oxford"
               },
               "geonames_id" : 4520760
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "acronym"
               ],
               "value" : "JAI"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "John Adams Institute for Accelerator Science"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/052gg0110",
               "label" : "University of Oxford",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/04g2vpn86",
               "label" : "Royal Holloway University of London",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/041kmwe10",
               "label" : "Imperial College London",
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
         "established" : 2009,
         "external_ids" : [
            {
               "all" : [
                  "grid.434160.4"
               ],
               "preferred" : "grid.434160.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 6043 947X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q19829505"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00w3swb66",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.esa.int/ESA_in_your_country/United_Kingdom"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/European_Centre_for_Space_Applications_and_Telecommunications"
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
                  "lat" : 51.60928,
                  "lng" : -1.24214,
                  "name" : "Didcot"
               },
               "geonames_id" : 2651269
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ECSAT"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "European Centre for Space Applications and Telecommunications"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03wd9za21",
               "label" : "European Space Agency",
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
         "established" : 2008,
         "external_ids" : [
            {
               "all" : [
                  "501100000349",
                  "100012373"
               ],
               "preferred" : "100012373",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.453470.1"
               ],
               "preferred" : "grid.453470.1",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/00bxmqe50",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.niaa.org.uk/"
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
                  "acronym"
               ],
               "value" : "NIAA"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "National Institute of Academic Anaesthesia"
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
         "established" : 2000,
         "external_ids" : [
            {
               "all" : [
                  "501100022585"
               ],
               "preferred" : "501100022585",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.481924.2"
               ],
               "preferred" : "grid.481924.2",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/002q11y83",
         "links" : [
            {
               "type" : "website",
               "value" : "http://icvi.org.uk/"
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
                  "acronym"
               ],
               "value" : "CVI"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ICVI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institute For Cancer Vaccine & Immunotherapy"
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
               "date" : "2019-02-17",
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
                  "501100017263"
               ],
               "preferred" : "501100017263",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.502890.6"
               ],
               "preferred" : "grid.502890.6",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/03vd8y618",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.riscs.org.uk/"
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
                  "acronym"
               ],
               "value" : "RISCS"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Research Institute in Science of Cyber Security"
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
         "established" : 1990,
         "external_ids" : [
            {
               "all" : [
                  "100014249"
               ],
               "preferred" : "100014249",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.500367.7"
               ],
               "preferred" : "grid.500367.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2288 0211"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q6049105"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03ab8zv57",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.icms.org.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/International_Centre_for_Mathematical_Sciences"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "SCT",
                  "country_subdivision_name" : "Scotland",
                  "lat" : 55.95206,
                  "lng" : -3.19648,
                  "name" : "Edinburgh"
               },
               "geonames_id" : 2650225
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ICMS"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "International Centre for Mathematical Sciences"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04mghma93",
               "label" : "Heriot-Watt University",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/01nrxwf90",
               "label" : "University of Edinburgh",
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
         "established" : 2011,
         "external_ids" : [
            {
               "all" : [
                  "501100014236"
               ],
               "preferred" : "501100014236",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.499434.7"
               ],
               "preferred" : "grid.499434.7",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/042sjcz88",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.srmrc.nihr.ac.uk/"
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
                  "ror_display",
                  "label"
               ],
               "value" : "NIHR Surgical Reconstruction and Microbiology Research Centre"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "SRMRC"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0187kwz08",
               "label" : "National Institute for Health Research",
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
         "established" : 1968,
         "external_ids" : [
            {
               "all" : [
                  "501100023376"
               ],
               "preferred" : "501100023376",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.4843.b"
               ],
               "preferred" : "grid.4843.b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 1703 001X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1408031"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03kchyj69",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.twi-global.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/The_Welding_Institute"
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
                  "lat" : 52.2,
                  "lng" : 0.11667,
                  "name" : "Cambridge"
               },
               "geonames_id" : 2653941
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "TWI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "The Welding Institute"
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
         "established" : 2008,
         "external_ids" : [
            {
               "all" : [
                  "501100019552"
               ],
               "preferred" : "501100019552",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.493543.c"
               ],
               "preferred" : "grid.493543.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q7950915"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/048eqtw54",
         "links" : [
            {
               "type" : "website",
               "value" : "https://wiserd.ac.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/WISERD"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "WLS",
                  "country_subdivision_name" : "Wales",
                  "lat" : 51.48,
                  "lng" : -3.18,
                  "name" : "Cardiff"
               },
               "geonames_id" : 2653822
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "WISERD"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Wales Institute of Social and Economic Research, Data and Methods"
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
               "date" : "2021-09-23",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2017,
         "external_ids" : [
            {
               "all" : [
                  "501100017510"
               ],
               "preferred" : "501100017510",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.511435.7"
               ],
               "preferred" : "grid.511435.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0005 0281 4208"
               ],
               "preferred" : "0000 0005 0281 4208",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/02wedp412",
         "links" : [
            {
               "type" : "website",
               "value" : "https://ukdri.ac.uk/"
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
                  "acronym"
               ],
               "value" : "UK DRI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "UK Dementia Research Institute"
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
                  "grid.421962.a"
               ],
               "preferred" : "grid.421962.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0641 4431"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/01q496a73",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.imm.ox.ac.uk/home"
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
                  "lat" : 51.75222,
                  "lng" : -1.25596,
                  "name" : "Oxford"
               },
               "geonames_id" : 2640729
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "MRC WIMM"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "MRC Weatherall Institute of Molecular Medicine"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02kcpr174",
               "label" : "MRC Human Immunology Unit",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02khxwt12",
               "label" : "MRC Molecular Haematology Unit",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03x94j517",
               "label" : "Medical Research Council",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/052gg0110",
               "label" : "University of Oxford",
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
               "date" : "2022-11-28",
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
                  "0000 0004 0606 3838"
               ],
               "preferred" : "0000 0004 0606 3838",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q102872408"
               ],
               "preferred" : "Q102872408",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02khxwt12",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.imm.ox.ac.uk/research/units-and-centres/mrc-molecular-haematology-unit"
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
                  "lat" : 51.75222,
                  "lng" : -1.25596,
                  "name" : "Oxford"
               },
               "geonames_id" : 2640729
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "MRC MHU"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "MRC Molecular Haematology Unit"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01q496a73",
               "label" : "MRC Weatherall Institute of Molecular Medicine",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03x94j517",
               "label" : "Medical Research Council",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/052gg0110",
               "label" : "University of Oxford",
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
         "established" : 1977,
         "external_ids" : [
            {
               "all" : [
                  "grid.268802.6"
               ],
               "preferred" : "grid.268802.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9140 1053"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30279654"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04gycm791",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.nottingham.ac.uk/mrcihr/index.aspx"
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
                  "lat" : 52.9536,
                  "lng" : -1.15047,
                  "name" : "Nottingham"
               },
               "geonames_id" : 2641170
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IHR"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "MRC Institute of Hearing Research"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03x94j517",
               "label" : "Medical Research Council",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/01ee9ar58",
               "label" : "University of Nottingham",
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
         "established" : 1968,
         "external_ids" : [
            {
               "all" : [
                  "501100001284",
                  "501100009314"
               ],
               "preferred" : "501100009314",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.40368.39"
               ],
               "preferred" : "grid.40368.39",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9347 0159"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q6040316"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04td3ys19",
         "links" : [
            {
               "type" : "website",
               "value" : "https://quadram.ac.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Quadram_Institute"
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
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IFR"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Institute of Food Research"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Quadram Institute"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Quadram Institute Bioscience"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00cwqg982",
               "label" : "Biotechnology and Biological Sciences Research Council",
               "type" : "parent"
            }
         ],
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
         "established" : 1992,
         "external_ids" : [
            {
               "all" : [
                  "501100005347",
                  "100012112"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.429097.1"
               ],
               "preferred" : "grid.429097.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0648 3322"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q126075"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/007vkej58",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.newton.ac.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Isaac_Newton_Institute"
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
                  "lat" : 52.2,
                  "lng" : 0.11667,
                  "name" : "Cambridge"
               },
               "geonames_id" : 2653941
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "INI"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Isaac Newton Institute"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Isaac Newton Institute for Mathematical Sciences"
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
         "established" : 1789,
         "external_ids" : [
            {
               "all" : [
                  "501100000527",
                  "501100014050"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.422885.1"
               ],
               "preferred" : "grid.422885.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0724 3660"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q676709"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04vhk9f59",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.armagh.space"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Armagh_Observatory"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GB",
                  "country_name" : "United Kingdom",
                  "country_subdivision_code" : "NIR",
                  "country_subdivision_name" : "Northern Ireland",
                  "lat" : 54.35,
                  "lng" : -6.66667,
                  "name" : "Armagh"
               },
               "geonames_id" : 2657060
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "AOP"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Armagh Observatory"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Armagh Observatory & Planetarium"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Armagh Observatory and Planetarium"
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
               "date" : "2021-09-23",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2013,
         "external_ids" : [
            {
               "all" : [
                  "grid.512598.2"
               ],
               "preferred" : "grid.512598.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q56758364"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05jx29z89",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.londonntd.org/"
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
                  "acronym"
               ],
               "value" : "LCNTDR"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "London Centre for Neglected Tropical Disease Research"
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
               "date" : "2022-06-16",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2017,
         "external_ids" : [],
         "id" : "https://ror.org/022p86748",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.bristol.ac.uk/wolfson-bioimaging/equipment/cryo-em"
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
               "value" : "GW4 Facility for High-Resolution Electron Cryo-Microscopy"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "GW4 cryoEM"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/000vekr11",
               "label" : "GW4",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/0524sp257",
               "label" : "University of Bristol",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03yghzc09",
               "label" : "University of Exeter",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/002h8g185",
               "label" : "University of Bath",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03kk7td41",
               "label" : "Cardiff University",
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
         "established" : 1950,
         "external_ids" : [
            {
               "all" : [
                  "501100000766"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.63833.3d"
               ],
               "preferred" : "grid.63833.3d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0643 7510"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q757609"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02gv4h649",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.awe.co.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Atomic_Weapons_Establishment"
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
                  "lat" : 51.45625,
                  "lng" : -0.97113,
                  "name" : "Reading"
               },
               "geonames_id" : 2639577
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "AWE"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "AWRE"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Atomic Weapons Establishment"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Atomic Weapons Research Establishment"
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
         "established" : 1988,
         "external_ids" : [
            {
               "all" : [
                  "grid.22319.3b"
               ],
               "preferred" : "grid.22319.3b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2106 2153"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7205824"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05av9mn02",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.pml.ac.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Plymouth_Marine_Laboratory"
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
                  "lat" : 50.37153,
                  "lng" : -4.14305,
                  "name" : "Plymouth"
               },
               "geonames_id" : 2640194
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "PML"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Plymouth Marine Laboratory"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02b5d8509",
               "label" : "Natural Environment Research Council",
               "type" : "related"
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
            "count" : 305,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 1,
            "id" : "na",
            "title" : "North America"
         }
      ],
      "countries" : [
         {
            "count" : 305,
            "id" : "gb",
            "title" : "United Kingdom"
         },
         {
            "count" : 1,
            "id" : "us",
            "title" : "United States"
         }
      ],
      "statuses" : [
         {
            "count" : 305,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 305,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 97,
            "id" : "funder",
            "title" : "funder"
         }
      ]
   },
   "number_of_results" : 305,
   "time_taken" : 65
}

```

## Example

```curl
curl 'https://api.ror.org/v2/organizations?filter=status:inactive,types:company,locations.geonames_details.country_code:AU' | json_pp
```

Returns a list of inactive companies in Australia. 

```json
{
   "items" : [
      {
         "admin" : {
            "created" : {
               "date" : "2020-07-06",
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
                  "100013336"
               ],
               "preferred" : "100013336",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.507920.9"
               ],
               "preferred" : "grid.507920.9",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 7860 3725"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/027xab135",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.lowcarbonlivingcrc.com.au/"
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
                  "lat" : -33.86785,
                  "lng" : 151.20732,
                  "name" : "Sydney"
               },
               "geonames_id" : 2147714
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CRCLCL"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Low Carbon Living CRC (Australia)"
            }
         ],
         "relationships" : [],
         "status" : "inactive",
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
         "established" : 2001,
         "external_ids" : [
            {
               "all" : [
                  "grid.472777.6"
               ],
               "preferred" : "grid.472777.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0640 4399"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/05r35r527",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.mylan.com/"
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
                  "ror_display",
                  "label"
               ],
               "value" : "Mylan (Australia)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04nsw4151",
               "label" : "Mylan (United States)",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/01g1gvr46",
               "label" : "Viatris",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "100011093"
               ],
               "preferred" : "100011093",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.473087.f"
               ],
               "preferred" : "grid.473087.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0466 7849"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/02z8ech05",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.shireaustralia.com.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Shire_(pharmaceutical_company)"
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
                  "lat" : -33.79677,
                  "lng" : 151.12436,
                  "name" : "North Ryde"
               },
               "geonames_id" : 7281782
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Shire (Australia)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04jfar564",
               "label" : "Takeda (Australia)",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
         "types" : [
            "company",
            "funder"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 3,
            "id" : "oc",
            "title" : "Oceania"
         }
      ],
      "countries" : [
         {
            "count" : 3,
            "id" : "au",
            "title" : "Australia"
         }
      ],
      "statuses" : [
         {
            "count" : 3,
            "id" : "inactive",
            "title" : "inactive"
         }
      ],
      "types" : [
         {
            "count" : 3,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 1,
            "id" : "funder",
            "title" : "funder"
         }
      ]
   },
   "number_of_results" : 3,
   "time_taken" : 1
}
```