---
title: Query parameter
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR API query parameter
  description: Instructions for using the query parameter of the ROR API.
  robots: index
---
> 👍 ROR REST API v2
>
> This page documents v2 of the ROR REST API. For v1 documentation of the ROR REST API, see [https://ror.readme.io/v1/docs/api-query](https://ror.readme.io/v1/docs/api-query). You can also read more about ROR [API versions](doc:api-versions) and a summary of what's new in [Schema 2.0](doc:schema-v2) and [Schema 2.1](doc:schema-2-1).

> ❗️ Version 1 of the ROR schema and API will be sunset in December 2025
>
> In December 2025, version 1 of the ROR schema and API will be sunset, meaning that ROR API requests with v1 in the path will no longer return a response, v1 files will no longer be included in the ROR data dump, and v1 documentation will no longer be available. Read more in our [changelog](https://ror.readme.io/changelog/2025-07-01-sunset-of-version-1#/).

# About the query parameter

The query parameter is a "quick search" of only the `names` and `external_ids` fields in ROR. The query parameter works best for the following purposes:

* Keyword-based searching for organization names
* Form field auto-suggests / typeaheads
* Searching for exact matches of an organization name
* Searching for external identifiers

We recommend using the query parameter to build [ROR-powered typeaheads in forms](doc:forms) that suggest organization names to users. The ROR [Web search interface](doc:web-search) at [https://ror.org/search](https://ror.org/search) also uses the query parameter.

Search results from the query parameter can be [filtered](doc:api-filtering) and [paged](doc:api-paging). By default, only records with an _active_ status are returned, but records with _inactive_ or _withdrawn_ status can be retrieved with the [all_status](doc:api-list#retrieve-a-list-of-records-with-all-statuses) parameter.

> 📘 Query parameter format
>
> `https://api.ror.org/v2/organizations?query=[value]`

# Formatting searches

All request strings must be [URL-encoded](https://www.w3schools.com/tags/ref_urlencode.asp).

## Special characters

Some organization names contain characters like &, (), : and /, which have special meaning in URI syntax, Elasticsearch syntax or both. To avoid error responses or bad results:

* Be sure to [URL-encode](https://www.w3schools.com/tags/ref_urlencode.asp) all query parameter values.
* Escape any [Elasticsearch reserved characters](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_reserved_characters) in the organization name with a URL-encoded backslash \ character. Reserved characters include `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

## Spaces and quotation marks

[Elasticsearch query string syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax) will treat words separated by a space as separate parts of a query. It is therefore advisable to surround multi-word search terms of the ROR API with URL-encoded quotation marks.

# Keyword searching

The query parameter can be used to search for relatively unique keywords in organization names.

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query=Bath' | json_pp
```

Returns a list of active organizations with the keyword "Bath" in a `names` field. As with [retrieving a list of ROR records](doc:api-list), the response is a JSON object containing full records for the first 20 search results, but this search produces only 17 results, so only 17 items are returned.

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
         "established" : 2003,
         "external_ids" : [
            {
               "all" : [
                  "grid.432411.1"
               ],
               "preferred" : "grid.432411.1",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04hf68923",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.bathlabs.com/"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath Labs"
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
         "established" : 1892,
         "external_ids" : [
            {
               "all" : [
                  "grid.468747.e"
               ],
               "preferred" : "grid.468747.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 6437 268X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5123579"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00pbyj886",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.bathcollege.ac.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bath_College"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath College"
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
               "date" : "2024-02-21",
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
                  "0000 0004 0578 2443"
               ],
               "preferred" : "0000 0004 0578 2443",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/012mkjy35",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.circlehealthgroup.co.uk/hospitals/bath-clinic"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "BMI Bath Clinic"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath Clinic"
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
               "date" : "2024-08-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "lb.com"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "Q810773"
               ],
               "preferred" : "Q810773",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/012ddk914",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.lb.com"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bath_%26_Body_Works"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "OH",
                  "country_subdivision_name" : "Ohio",
                  "lat" : 39.96118,
                  "lng" : -82.99879,
                  "name" : "Columbus"
               },
               "geonames_id" : 4509177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Bath & Body Works"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Bath & Body Works (United States)"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath and Body Works"
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
         "established" : 1966,
         "external_ids" : [
            {
               "all" : [
                  "501100000835"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.7340.0"
               ],
               "preferred" : "grid.7340.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2162 1699"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1422458"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/002h8g185",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.bath.ac.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/University_of_Bath"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Bath"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00a858n67",
               "label" : "Royal United Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/022p86748",
               "label" : "GW4 Facility for High-Resolution Electron Cryo-Microscopy",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/000vekr11",
               "label" : "GW4",
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
         "established" : 1962,
         "external_ids" : [
            {
               "all" : [
                  "grid.447036.2"
               ],
               "preferred" : "grid.447036.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2226 6326"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/0277k6k46",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.mainemaritimemuseum.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Maine_Maritime_Museum"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "ME",
                  "country_subdivision_name" : "Maine",
                  "lat" : 43.91064,
                  "lng" : -69.8206,
                  "name" : "Bath"
               },
               "geonames_id" : 4957570
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath Marine Museum"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Maine Maritime Museum"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "archive"
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
                  "grid.474071.1"
               ],
               "preferred" : "grid.474071.1",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/03zhn2w96",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.maax.com/en"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "CA",
                  "country_name" : "Canada",
                  "country_subdivision_code" : "QC",
                  "country_subdivision_name" : "Quebec",
                  "lat" : 45.50884,
                  "lng" : -73.58781,
                  "name" : "Montreal"
               },
               "geonames_id" : 6077243
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "MAAX Bath (Canada)"
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
         "domains" : [
            "bathspa.ac.uk"
         ],
         "established" : 2005,
         "external_ids" : [
            {
               "all" : [
                  "100010331"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.252874.e"
               ],
               "preferred" : "grid.252874.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2034 9451"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3091754"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0038jbq24",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.bathspa.ac.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Bath_Spa_University"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath College of Higher Education"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath Spa University"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath Spa University College"
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
               "date" : "2020-12-21",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1878,
         "external_ids" : [
            {
               "all" : [
                  "grid.509313.d"
               ],
               "preferred" : "grid.509313.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0420 1096"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q4868960"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01d4n7s11",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.fingerlakes.va.gov/locations/Bath_VA_Medical_Center.asp"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bath_VA_Medical_Center"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 40.60455,
                  "lng" : -74.00431,
                  "name" : "Bath Beach"
               },
               "geonames_id" : 5108111
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath VA Medical Center"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01rkxdk30",
               "label" : "VA Finger Lakes Healthcare System",
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
         "established" : 1824,
         "external_ids" : [
            {
               "all" : [
                  "grid.493239.6"
               ],
               "preferred" : "grid.493239.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 5895 4392"
               ],
               "preferred" : "0000 0004 5895 4392",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/046jgev73",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.brlsi.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bath_Royal_Literary_and_Scientific_Institution"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BRLSI"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath Literary and Scientific Institution"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath Royal Literary and Scientific Institution"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
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
         "established" : 1975,
         "external_ids" : [
            {
               "all" : [
                  "grid.418231.d"
               ],
               "preferred" : "grid.418231.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0641 831X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30281630"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05tacab76",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.birdbath.org.uk/"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BIRD"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath Institute for Rheumatic Diseases"
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
         "established" : 2011,
         "external_ids" : [
            {
               "all" : [
                  "grid.498158.9"
               ],
               "preferred" : "grid.498158.9",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/0105p2r38",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.bbsp.co.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bristol_and_Bath_Science_Park"
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
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BBSP"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bristol and Bath Science Park"
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
         "established" : 2016,
         "external_ids" : [
            {
               "all" : [
                  "grid.499705.6"
               ],
               "preferred" : "grid.499705.6",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/02fzzq880",
         "links" : [
            {
               "type" : "website",
               "value" : "http://bathsdr.org/"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BSDR"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath Social and Development Research"
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
         "established" : 1968,
         "external_ids" : [
            {
               "all" : [
                  "grid.486748.1"
               ],
               "preferred" : "grid.486748.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0449 0078"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/01datx010",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.designability.org.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bath_Institute_of_Medical_Engineering"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath Institute of Medical Engineering"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Designability"
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
         "established" : 1996,
         "external_ids" : [
            {
               "all" : [
                  "grid.450510.5"
               ],
               "preferred" : "grid.450510.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0374 2966"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q16966588"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03rwxcx12",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.bathnes.gov.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bath_and_North_East_Somerset_Council"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath and North East Somerset Council"
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
         "established" : 1864,
         "external_ids" : [
            {
               "all" : [
                  "grid.413029.d"
               ],
               "preferred" : "grid.413029.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0374 2907"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q18162105"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/058x7dy48",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.ruh.nhs.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Royal_United_Hospital_Bath_NHS_Foundation_Trust"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Royal United Hospital Bath NHS Foundation Trust"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Royal United Hospital Bath NHS Trust"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05f9e0404",
               "label" : "Paulton Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05va5gy74",
               "label" : "Royal National Hospital for Rheumatic Diseases",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00a858n67",
               "label" : "Royal United Hospital",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.476920.e"
               ],
               "preferred" : "grid.476920.e",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/00pkb6h65",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.bathandnortheastsomersetccg.nhs.uk/"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BaNES CCG"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath and North East Somerset Clinical Commissioning Group"
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
            "count" : 13,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 4,
            "id" : "na",
            "title" : "North America"
         }
      ],
      "countries" : [
         {
            "count" : 13,
            "id" : "gb",
            "title" : "United Kingdom"
         },
         {
            "count" : 3,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 1,
            "id" : "ca",
            "title" : "Canada"
         }
      ],
      "statuses" : [
         {
            "count" : 17,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 4,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 4,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 3,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 2,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 2,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 1,
            "id" : "archive",
            "title" : "archive"
         },
         {
            "count" : 1,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 1,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 1,
            "id" : "other",
            "title" : "other"
         }
      ]
   },
   "number_of_results" : 17,
   "time_taken" : 4
}
```

> 🚧 Remember that the query parameter does not search all fields
>
> The query parameter searches only the `names` field, which includes acronyms, aliases, and the organization names in various languages, and the `external_ids` field.  Results from keyword searches using the query parameter **do not include** values from fields such as `links` and `locations`, for instance. To find organizations by website, location, or other criteria, use [Filtering](doc:api-filtering) or the [Advanced query parameter](doc:api-advanced-query).

# Exact string searching

Keyword searches can produce hundreds or thousands of results, especially when the string contains common terms in research organization names such as "University," "Health," or "Ministry." Surrounding a string with quotation marks will reduce the number of results in the response, limiting the results to records with exact matches to that string. Quotation marks must be [URL-encoded](https://developer.mozilla.org/en-US/docs/Glossary/percent-encoding).

## Example

Compare the following searches to see the difference quotation marks make for multiple-term search strings with spaces. The first example shows a search for Bath College.

```curl
curl 'https://api.ror.org/v2/organizations?query=Bath%20College' | json_pp
```

This search looks for **both** the term "Bath" **and** the term "College" and returns thousands of results because the term "College" appears in many organization names. A search on the single term "Bath" produces only 17 results, because it is a much less common term in organization names. The organization record for Bath College is the first result, but many factors can affect the order of results, so we strongly recommend against automatically selecting the first item in any list of search results.

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
         "established" : 1892,
         "external_ids" : [
            {
               "all" : [
                  "grid.468747.e"
               ],
               "preferred" : "grid.468747.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 6437 268X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5123579"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00pbyj886",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.bathcollege.ac.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bath_College"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath College"
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
            "bathspa.ac.uk"
         ],
         "established" : 2005,
         "external_ids" : [
            {
               "all" : [
                  "100010331"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.252874.e"
               ],
               "preferred" : "grid.252874.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2034 9451"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3091754"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0038jbq24",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.bathspa.ac.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Bath_Spa_University"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath College of Higher Education"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath Spa University"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath Spa University College"
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
         "established" : 2003,
         "external_ids" : [
            {
               "all" : [
                  "grid.432411.1"
               ],
               "preferred" : "grid.432411.1",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04hf68923",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.bathlabs.com/"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath Labs"
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
               "date" : "2024-02-21",
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
                  "0000 0004 0578 2443"
               ],
               "preferred" : "0000 0004 0578 2443",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/012mkjy35",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.circlehealthgroup.co.uk/hospitals/bath-clinic"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "BMI Bath Clinic"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath Clinic"
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
               "date" : "2024-08-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "lb.com"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "Q810773"
               ],
               "preferred" : "Q810773",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/012ddk914",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.lb.com"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bath_%26_Body_Works"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "OH",
                  "country_subdivision_name" : "Ohio",
                  "lat" : 39.96118,
                  "lng" : -82.99879,
                  "name" : "Columbus"
               },
               "geonames_id" : 4509177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Bath & Body Works"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Bath & Body Works (United States)"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath and Body Works"
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
         "established" : 1966,
         "external_ids" : [
            {
               "all" : [
                  "501100000835"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.7340.0"
               ],
               "preferred" : "grid.7340.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2162 1699"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1422458"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/002h8g185",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.bath.ac.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/University_of_Bath"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Bath"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00a858n67",
               "label" : "Royal United Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/022p86748",
               "label" : "GW4 Facility for High-Resolution Electron Cryo-Microscopy",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/000vekr11",
               "label" : "GW4",
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
         "established" : 1962,
         "external_ids" : [
            {
               "all" : [
                  "grid.447036.2"
               ],
               "preferred" : "grid.447036.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2226 6326"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/0277k6k46",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.mainemaritimemuseum.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Maine_Maritime_Museum"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "ME",
                  "country_subdivision_name" : "Maine",
                  "lat" : 43.91064,
                  "lng" : -69.8206,
                  "name" : "Bath"
               },
               "geonames_id" : 4957570
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath Marine Museum"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Maine Maritime Museum"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "archive"
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
                  "grid.474071.1"
               ],
               "preferred" : "grid.474071.1",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/03zhn2w96",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.maax.com/en"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "CA",
                  "country_name" : "Canada",
                  "country_subdivision_code" : "QC",
                  "country_subdivision_name" : "Quebec",
                  "lat" : 45.50884,
                  "lng" : -73.58781,
                  "name" : "Montreal"
               },
               "geonames_id" : 6077243
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "MAAX Bath (Canada)"
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
               "date" : "2020-12-21",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1878,
         "external_ids" : [
            {
               "all" : [
                  "grid.509313.d"
               ],
               "preferred" : "grid.509313.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0420 1096"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q4868960"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01d4n7s11",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.fingerlakes.va.gov/locations/Bath_VA_Medical_Center.asp"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bath_VA_Medical_Center"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 40.60455,
                  "lng" : -74.00431,
                  "name" : "Bath Beach"
               },
               "geonames_id" : 5108111
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath VA Medical Center"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01rkxdk30",
               "label" : "VA Finger Lakes Healthcare System",
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
         "established" : 1824,
         "external_ids" : [
            {
               "all" : [
                  "grid.493239.6"
               ],
               "preferred" : "grid.493239.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 5895 4392"
               ],
               "preferred" : "0000 0004 5895 4392",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/046jgev73",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.brlsi.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bath_Royal_Literary_and_Scientific_Institution"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BRLSI"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath Literary and Scientific Institution"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath Royal Literary and Scientific Institution"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
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
         "established" : 1975,
         "external_ids" : [
            {
               "all" : [
                  "grid.418231.d"
               ],
               "preferred" : "grid.418231.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0641 831X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30281630"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05tacab76",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.birdbath.org.uk/"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BIRD"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath Institute for Rheumatic Diseases"
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
         "established" : 2011,
         "external_ids" : [
            {
               "all" : [
                  "grid.498158.9"
               ],
               "preferred" : "grid.498158.9",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/0105p2r38",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.bbsp.co.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bristol_and_Bath_Science_Park"
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
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BBSP"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bristol and Bath Science Park"
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
         "established" : 2016,
         "external_ids" : [
            {
               "all" : [
                  "grid.499705.6"
               ],
               "preferred" : "grid.499705.6",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/02fzzq880",
         "links" : [
            {
               "type" : "website",
               "value" : "http://bathsdr.org/"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BSDR"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath Social and Development Research"
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
         "established" : 1968,
         "external_ids" : [
            {
               "all" : [
                  "grid.486748.1"
               ],
               "preferred" : "grid.486748.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0449 0078"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/01datx010",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.designability.org.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bath_Institute_of_Medical_Engineering"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath Institute of Medical Engineering"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Designability"
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
         "established" : 1996,
         "external_ids" : [
            {
               "all" : [
                  "grid.450510.5"
               ],
               "preferred" : "grid.450510.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0374 2966"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q16966588"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03rwxcx12",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.bathnes.gov.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bath_and_North_East_Somerset_Council"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath and North East Somerset Council"
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
         "established" : 1864,
         "external_ids" : [
            {
               "all" : [
                  "grid.413029.d"
               ],
               "preferred" : "grid.413029.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0374 2907"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q18162105"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/058x7dy48",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.ruh.nhs.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Royal_United_Hospital_Bath_NHS_Foundation_Trust"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Royal United Hospital Bath NHS Foundation Trust"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Royal United Hospital Bath NHS Trust"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05f9e0404",
               "label" : "Paulton Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05va5gy74",
               "label" : "Royal National Hospital for Rheumatic Diseases",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00a858n67",
               "label" : "Royal United Hospital",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.476920.e"
               ],
               "preferred" : "grid.476920.e",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/00pkb6h65",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.bathandnortheastsomersetccg.nhs.uk/"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BaNES CCG"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath and North East Somerset Clinical Commissioning Group"
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
         "established" : 1965,
         "external_ids" : [
            {
               "all" : [
                  "grid.443664.7"
               ],
               "preferred" : "grid.443664.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0642 2167"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q11526509"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01n7wbg90",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.higashiosaka.ac.jp/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Higashiosaka_Junior_College"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "27",
                  "country_subdivision_name" : "Osaka",
                  "lat" : 34.66667,
                  "lng" : 135.58333,
                  "name" : "Higashiosaka"
               },
               "geonames_id" : 1862752
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Higashiosaka College & Higashiosaka Junior College"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "Higashiosaka Junior College"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "東大阪大学・東大阪大学短期大学部"
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
               "date" : "2024-10-13",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "fmp.edu.cn"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "0000 0005 1471 2065"
               ],
               "preferred" : "0000 0005 1471 2065",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/02p34hm58",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.fmp.edu.cn"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "CN",
                  "country_name" : "China",
                  "country_subdivision_code" : "FJ",
                  "country_subdivision_name" : "Fujian",
                  "lat" : 26.06139,
                  "lng" : 119.30611,
                  "name" : "Fuzhou"
               },
               "geonames_id" : 1810821
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "acronym"
               ],
               "value" : "FMP"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Fuzhou Melbourne Polytechnic"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Minjiang College IEN International College"
            },
            {
               "lang" : "zh",
               "types" : [
                  "label"
               ],
               "value" : "福州墨尔本理工职业学院"
            },
            {
               "lang" : "zh",
               "types" : [
                  "alias"
               ],
               "value" : "闽江学院爱恩国际学院"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00s7tkw17",
               "label" : "Minjiang University",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/04cq7wg91",
               "label" : "Melbourne Polytechnic",
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
               "date" : "2024-07-08",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "hustwenhua.net"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "Q10905382"
               ],
               "preferred" : "Q10905382",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05fagpw72",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.hustwenhua.net"
            },
            {
               "type" : "wikipedia",
               "value" : "https://zh.wikipedia.org/wiki/%E6%96%87%E5%8D%8E%E5%AD%A6%E9%99%A2"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "CN",
                  "country_name" : "China",
                  "country_subdivision_code" : "HB",
                  "country_subdivision_name" : "Hubei",
                  "lat" : 30.58333,
                  "lng" : 114.26667,
                  "name" : "Wuhan"
               },
               "geonames_id" : 1791247
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Wenhua College"
            },
            {
               "lang" : "zh",
               "types" : [
                  "label"
               ],
               "value" : "文华学院"
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
            "count" : 2300,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 1665,
            "id" : "as",
            "title" : "Asia"
         },
         {
            "count" : 627,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 212,
            "id" : "af",
            "title" : "Africa"
         },
         {
            "count" : 73,
            "id" : "oc",
            "title" : "Oceania"
         },
         {
            "count" : 8,
            "id" : "sa",
            "title" : "South America"
         }
      ],
      "countries" : [
         {
            "count" : 2100,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 417,
            "id" : "in",
            "title" : "India"
         },
         {
            "count" : 361,
            "id" : "jp",
            "title" : "Japan"
         },
         {
            "count" : 337,
            "id" : "gb",
            "title" : "United Kingdom"
         },
         {
            "count" : 307,
            "id" : "cn",
            "title" : "China"
         },
         {
            "count" : 165,
            "id" : "ca",
            "title" : "Canada"
         },
         {
            "count" : 117,
            "id" : "ph",
            "title" : "Philippines"
         },
         {
            "count" : 78,
            "id" : "gh",
            "title" : "Ghana"
         },
         {
            "count" : 64,
            "id" : "kr",
            "title" : "South Korea"
         },
         {
            "count" : 53,
            "id" : "au",
            "title" : "Australia"
         }
      ],
      "statuses" : [
         {
            "count" : 4885,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 4564,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 475,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 182,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 65,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 46,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 17,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 4,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 4,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 3,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 4885,
   "time_taken" : 8
}
```

> 🚧 Don't automatically choose the first result in a list
>
> The first item is often but not always the best match or desired result for a given search of the ROR API. We advise against building tools that automatically select the first result in a list of records.

Searching for the exact term "Bath College" retrieves only records that contain that precise phrase in the `names` field. Remember that the `names` field can include names in several languages, acronyms, and aliases.

```curl
curl 'https://api.ror.org/v2/organizations?query=%22Bath%20College%22' | json_pp
```

The response returns two active records: the record for Bath College and the record for Bath Spa University, which has the alias "Bath College of Higher Education".

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
         "established" : 1892,
         "external_ids" : [
            {
               "all" : [
                  "grid.468747.e"
               ],
               "preferred" : "grid.468747.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 6437 268X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5123579"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00pbyj886",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.bathcollege.ac.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bath_College"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath College"
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
            "bathspa.ac.uk"
         ],
         "established" : 2005,
         "external_ids" : [
            {
               "all" : [
                  "100010331"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.252874.e"
               ],
               "preferred" : "grid.252874.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2034 9451"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3091754"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0038jbq24",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.bathspa.ac.uk/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Bath_Spa_University"
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
                  "lat" : 51.3751,
                  "lng" : -2.36172,
                  "name" : "Bath"
               },
               "geonames_id" : 2656173
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath College of Higher Education"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bath Spa University"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bath Spa University College"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education",
            "funder"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 2,
            "id" : "eu",
            "title" : "Europe"
         }
      ],
      "countries" : [
         {
            "count" : 2,
            "id" : "gb",
            "title" : "United Kingdom"
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
            "count" : 2,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 1,
            "id" : "funder",
            "title" : "funder"
         }
      ]
   },
   "number_of_results" : 2,
   "time_taken" : 3
}
```

## Example

Remember to URL-encode and if necessary escape special characters in multi-term search strings, as when searching for "Franklin & all College". The single & character is not reserved in Elasticsearch and therefore is not escaped.

```curl
curl 'https://api.ror.org/v2/organizations?query=%22Franklin%20%26%20Marshall%20College%22' | json_pp
```

When the `&` is URL-encoded, the response returns the active record for Franklin & Marshall College. When the & is not URL-encoded, the response is an error.

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
         "domains" : [
            "fandm.edu"
         ],
         "established" : 1787,
         "external_ids" : [
            {
               "all" : [
                  "100012632"
               ],
               "preferred" : "100012632",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.256069.e"
               ],
               "preferred" : "grid.256069.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2162 8305"
               ],
               "preferred" : "0000 0001 2162 8305",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q664881"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04fp4ps48",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.fandm.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Franklin_%26_Marshall_College"
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
                  "lat" : 40.03788,
                  "lng" : -76.30551,
                  "name" : "Lancaster"
               },
               "geonames_id" : 5197079
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "F&M"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Franklin & Marshall College"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education",
            "funder"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 1,
            "id" : "na",
            "title" : "North America"
         }
      ],
      "countries" : [
         {
            "count" : 1,
            "id" : "us",
            "title" : "United States"
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
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 1,
            "id" : "funder",
            "title" : "funder"
         }
      ]
   },
   "number_of_results" : 1,
   "time_taken" : 2
}
```

# Searching for identifiers

The query parameter searches the `external_ids` field and so can be used to search for ROR records that match an external unique identifier. Use URL-encoded quotation marks before and after the identifier search string for best results. This search will work for all identifier schemes supported in the `external_ids` field, including GRID, ISNI, Wikidata, and the Crossref Open Funder Registry. Remember that only active ROR records will be returned by default: use the [all_status](doc:api-list#retrieve-a-list-of-records-with-all-statuses) parameter to retrieve results that include _inactive_ and _withdrawn_ ROR records.

## Example

Search for an active ROR record corresponding to the GRID ID for the U.S. Department of Energy.

```curl
curl 'https://api.ror.org/v2/organizations?query=%22grid.85084.31%22' | json_pp
```

The response is a single active record for the U.S. Department of Energy that contains the searched-for identifier in `external_ids`.

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
         "established" : 1977,
         "external_ids" : [
            {
               "all" : [
                  "100000015"
               ],
               "preferred" : "100000015",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.85084.31"
               ],
               "preferred" : "grid.85084.31",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2342 3717"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q217810",
                  "Q6422983"
               ],
               "preferred" : "Q217810",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01bj3aw27",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.energy.gov"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/United_States_Department_of_Energy"
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
               "value" : "DOE"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Departamento de Energía de los Estados Unidos"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Département de l'Énergie des États-unis"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "U.S. Department of Energy"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "United States Department of Energy"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03q1rgc19",
               "label" : "Advanced Research Projects Agency - Energy",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00qq8bd87",
               "label" : "Consortium for the Advanced Simulation of Light Water Reactors",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01ca2by25",
               "label" : "Great Lakes Bioenergy Research Center",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04d8cf622",
               "label" : "International Partnership for the Hydrogen and Fuel Cell in the Economy",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03ww55028",
               "label" : "Joint BioEnergy Institute",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03sk1we31",
               "label" : "National Nuclear Security Administration",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/037k8mg80",
               "label" : "Nevada National Security Site",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05hsv7e61",
               "label" : "Office of Economic Impact and Diversity",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02xznz413",
               "label" : "Office of Energy Efficiency and Renewable Energy",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00km40770",
               "label" : "Office of Environmental Management",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01c9ay627",
               "label" : "Office of Environmental Protection, Sustainability Support and Corporate Safety Analysis",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03ery9d53",
               "label" : "Office of Fossil Energy",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04nnxen11",
               "label" : "Office of Inspector General",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02z1qvq09",
               "label" : "Office of Intelligence and Counterintelligence",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05ek3m339",
               "label" : "Office of International Affairs",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03jf3w726",
               "label" : "Office of Legacy Management",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02ah1da87",
               "label" : "Office of Management",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05tj7dm33",
               "label" : "Office of Nuclear Energy",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00mmn6b08",
               "label" : "Office of Science",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04s778r16",
               "label" : "Office of Space and Defense Power Systems",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03eecgp81",
               "label" : "Office of Under Secretary of Energy for Science",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0054t4769",
               "label" : "Office of the General Counsel",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05hhm9a98",
               "label" : "Savannah River Operations Office",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01h04ms65",
               "label" : "U.S. Energy Information Administration",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/048g3cy84",
               "label" : "Vera C. Rubin Observatory",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03e7mhc87",
               "label" : "Kansas City National Security Campus",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01t14bp54",
               "label" : "Environmental System Science Data Infrastructure for a Virtual Ecosystem",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00hvbwp12",
               "label" : "Office of the Chief Information Officer",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00htph268",
               "label" : "Office of Under Secretary for Science and Innovation",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02vg64e60",
               "label" : "Office of Technology Transitions",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/028rfb880",
               "label" : "Office of Secretary of Energy",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00mw1dx44",
               "label" : "Office of Public Affairs",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05p9bjd64",
               "label" : "Office of Policy",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02bhbx726",
               "label" : "Office of Indian Energy Policy and Programs",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/037897814",
               "label" : "Office of Fossil Energy and Carbon Management",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04csf5r20",
               "label" : "Office of Environment, Health, Safety and Security",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/035fxjt55",
               "label" : "Office of Enterprise Assessments",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/035sc8895",
               "label" : "Office of State and Community Energy Programs",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01zprx076",
               "label" : "Office of Federal Energy Management Programs",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00n4kg172",
               "label" : "Grid Deployment Office",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02y6dp041",
               "label" : "Office of Electricity",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/041g4v832",
               "label" : "Office of Cybersecurity, Energy Security, and Emergency Response",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05cpnza27",
               "label" : "Office of Congressional and Intergovernmental Affairs",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/040vxhp34",
               "label" : "Oak Ridge Institute for Science and Education",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00536t873",
               "label" : "Artificial Intelligence & Technology Office",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04rp4zh16",
               "label" : "Office of Clean Energy Demonstrations",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02rcrvv70",
               "label" : "Government of the United States of America",
               "type" : "parent"
            }
         ],
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
            "id" : "na",
            "title" : "North America"
         }
      ],
      "countries" : [
         {
            "count" : 1,
            "id" : "us",
            "title" : "United States"
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
   "time_taken" : 6
}
```

# Retrieve records with any status

By default, queries of the ROR API return only records with _active_ status. The [all_status](doc:api-list#retrieve-a-list-of-records-with-all-statuses) parameter can be appended to searches in order to retrieve _inactive_ and _withdrawn_ records as well as _active_ records.

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query=Harvard&all_status' | json_pp
```

The response returns the first 20 results of a keyword search for "Harvard" that includes records of all status types.

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
               "date" : "2025-05-05",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "harvard.edu"
         ],
         "established" : 1636,
         "external_ids" : [
            {
               "all" : [
                  "100007229",
                  "100010520",
                  "100009802",
                  "100009868",
                  "100008548",
                  "100008549",
                  "100005724",
                  "100005578",
                  "100009116",
                  "100008036",
                  "100009345",
                  "100005668",
                  "100010952",
                  "100008024",
                  "100005487",
                  "100005473",
                  "100005293",
                  "100006075",
                  "100005915",
                  "100005469",
                  "100005650",
                  "100005678",
                  "100005692",
                  "100005802",
                  "100005856",
                  "100005893",
                  "100005941",
                  "100006007",
                  "100006011",
                  "100006274",
                  "100007230",
                  "100007300",
                  "100007887",
                  "100008263",
                  "100005574",
                  "100007299",
                  "100006691",
                  "100007301",
                  "100019552",
                  "100010951",
                  "100013146",
                  "100013753",
                  "100013843",
                  "100015369",
                  "100018077",
                  "100018418",
                  "100019067",
                  "100019111",
                  "100019984",
                  "100022285"
               ],
               "preferred" : "100007229",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.38142.3c"
               ],
               "preferred" : "grid.38142.3c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1936 754X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q13371",
                  "Q5676556"
               ],
               "preferred" : "Q13371",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03vek6s52",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.harvard.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Harvard_University"
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
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard University"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Universidad de Harvard"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/032q5ym94",
               "label" : "Athinoula A. Martinos Center for Biomedical Imaging",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03hj6c016",
               "label" : "Berenson Allen Center for Noninvasive Brain Stimulation",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03c3r2d17",
               "label" : "Center for Astrophysics Harvard & Smithsonian",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05r3dyn47",
               "label" : "Center for Systems Biology",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04wsv7966",
               "label" : "Center for Vascular Biology Research",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/004y4rj95",
               "label" : "Gordon Center for Medical Imaging",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04kj1hn59",
               "label" : "Harvard Stem Cell Institute",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/006v7bf86",
               "label" : "Harvard University Press",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/053tmcn30",
               "label" : "MIT-Harvard Center for Ultracold Atoms",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/053r20n13",
               "label" : "Ragon Institute of MGH, MIT and Harvard",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04r5ess67",
               "label" : "Sleep and Human Health Institute",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04pvzz946",
               "label" : "The NSF AI Institute for Artificial Intelligence and Fundamental Interactions",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05xsxgs79",
               "label" : "Arnold Arboretum",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/059cpzx98",
               "label" : "Harvard Forest Long Term Ecological Research",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/008cfmj78",
               "label" : "Wyss Institute for Biologically Inspired Engineering",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03pvyf116",
               "label" : "Dana-Farber/Harvard Cancer Center",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05b0g2v72",
               "label" : "Real Colegio Complutense",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04drvxt59",
               "label" : "Beth Israel Deaconess Medical Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00dvg7y05",
               "label" : "Boston Children's Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05xckek43",
               "label" : "Boston IVF",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/04rkbns44",
               "label" : "Botswana Harvard AIDS Institute Partnership",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03w44ff23",
               "label" : "Brigham and Women's Faulkner Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/04b6nzv94",
               "label" : "Brigham and Women's Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05a0ya142",
               "label" : "Broad Institute",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/059c3mv67",
               "label" : "Cambridge Health Alliance",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/02jzgtq86",
               "label" : "Dana-Farber Cancer Institute",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01zxdeg39",
               "label" : "Harvard Pilgrim Health Care",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00jjeh629",
               "label" : "Harvard–MIT Division of Health Sciences and Technology",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/02vptss42",
               "label" : "Hebrew SeniorLife",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/044hpwe09",
               "label" : "IIT@Harvard",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/0280a3n32",
               "label" : "Joslin Diabetes Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05tby3y60",
               "label" : "Judge Baker Children's Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03mbq3y29",
               "label" : "Lahey Hospital and Medical Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/04g3dn724",
               "label" : "Massachusetts Eye and Ear Infirmary",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/002pd6e78",
               "label" : "Massachusetts General Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05par7p11",
               "label" : "Massachusetts Green High Performance Computing Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01kta7d96",
               "label" : "McLean Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03hrxmf69",
               "label" : "Newton Wellesley Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/023pf5e38",
               "label" : "Somerville Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/011dvr318",
               "label" : "Spaulding Rehabilitation Hospital",
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
               "date" : "2021-04-06",
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
                  "grid.509953.3"
               ],
               "preferred" : "grid.509953.3",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/044hpwe09",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.iit.it/research/lines/iit-harvard"
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
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "IIT@Harvard"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/042t93s57",
               "label" : "Italian Institute of Technology",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
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
         "established" : 1913,
         "external_ids" : [
            {
               "all" : [
                  "grid.446714.4"
               ],
               "preferred" : "grid.446714.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0694 1061"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1587900"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/006v7bf86",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.hup.harvard.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Harvard_University_Press"
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
                  "acronym"
               ],
               "value" : "HUP"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard University Press"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
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
         "established" : 1901,
         "external_ids" : [
            {
               "all" : [
                  "grid.425914.f"
               ],
               "preferred" : "grid.425914.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30285210"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05x88b790",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.harvardbioscience.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Harvard_Bioscience"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BW",
                  "country_subdivision_name" : "Baden-Wurttemberg",
                  "lat" : 48.49144,
                  "lng" : 9.20427,
                  "name" : "Reutlingen"
               },
               "geonames_id" : 2847736
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard Bioscience (Germany)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02b1nt179",
               "label" : "Harvard Bioscience (United States)",
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
         "established" : 2001,
         "external_ids" : [
            {
               "all" : [
                  "100001157"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.452581.f"
               ],
               "preferred" : "grid.452581.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0457 7595"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/000cs1t14",
         "links" : [
            {
               "type" : "website",
               "value" : "http://neurodiscovery.harvard.edu/"
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
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard NeuroDiscovery Center"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
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
         "established" : 1839,
         "external_ids" : [
            {
               "all" : [
                  "grid.455753.5"
               ],
               "preferred" : "grid.455753.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0634 3397"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q637936"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01mcvy510",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.cfa.harvard.edu/hco"
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
                  "acronym"
               ],
               "value" : "HCO"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard College Observatory"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03c3r2d17",
               "label" : "Center for Astrophysics Harvard & Smithsonian",
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
         "established" : 1913,
         "external_ids" : [
            {
               "all" : [
                  "100004043"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.479827.7"
               ],
               "preferred" : "grid.479827.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 5902 5982"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/05xph5761",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.harvardtravellersclub.org/"
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
                  "lat" : 42.15843,
                  "lng" : -71.14477,
                  "name" : "Canton"
               },
               "geonames_id" : 4932214
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard Travellers Club"
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
         "established" : 1928,
         "external_ids" : [
            {
               "all" : [
                  "100001158"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.479832.3"
               ],
               "preferred" : "grid.479832.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 8982 0275"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q175863"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01kv2k333",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.harvard-yenching.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Harvard%E2%80%93Yenching_Institute"
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
                  "acronym"
               ],
               "value" : "HYI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard-Yenching Institute"
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
         "established" : 1973,
         "external_ids" : [
            {
               "all" : [
                  "grid.477299.6"
               ],
               "preferred" : "grid.477299.6",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/03d9hbe08",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.harvardeye.com/"
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
                  "lat" : 33.61252,
                  "lng" : -117.71283,
                  "name" : "Laguna Hills"
               },
               "geonames_id" : 5364306
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard Eye Associates"
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
               "date" : "2022-06-16",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1988,
         "external_ids" : [],
         "id" : "https://ror.org/059cpzx98",
         "links" : [
            {
               "type" : "website",
               "value" : "https://harvardforest.fas.harvard.edu/other-tags/lter"
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
                  "lat" : 42.48759,
                  "lng" : -72.18703,
                  "name" : "Petersham"
               },
               "geonames_id" : 4947085
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HFR"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HFR LTER"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Harvard Forest LTER"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard Forest Long Term Ecological Research"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/039kwqk96",
               "label" : "Long Term Ecological Research Network",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/0330j0z60",
               "label" : "Environmental Data Initiative",
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
         "established" : 1782,
         "external_ids" : [
            {
               "all" : [
                  "grid.471403.5"
               ],
               "preferred" : "grid.471403.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0378 0882"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q49121"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03wevmz92",
         "links" : [
            {
               "type" : "website",
               "value" : "http://hms.harvard.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Harvard_Medical_School"
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
                  "acronym"
               ],
               "value" : "HMS"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard Medical School"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05xckek43",
               "label" : "Boston IVF",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/02vptss42",
               "label" : "Hebrew SeniorLife",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/0280a3n32",
               "label" : "Joslin Diabetes Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05tby3y60",
               "label" : "Judge Baker Children's Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "successor"
            }
         ],
         "status" : "withdrawn",
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
         "established" : 1960,
         "external_ids" : [
            {
               "all" : [
                  "grid.413723.0"
               ],
               "preferred" : "grid.413723.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0546 0655"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5676629"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03hwmxn85",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.atriushealth.org/?practice=harvard"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Harvard_Vanguard_Medical_Associates"
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
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard Vanguard Medical Associates"
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
         "established" : 1901,
         "external_ids" : [
            {
               "all" : [
                  "grid.438765.b"
               ],
               "preferred" : "grid.438765.b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0545 2436"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q16994890"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02b1nt179",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.harvardbioscience.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Harvard_Bioscience"
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
                  "lat" : 42.2001,
                  "lng" : -71.4245,
                  "name" : "Holliston"
               },
               "geonames_id" : 4939693
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard Bioscience (United States)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05x88b790",
               "label" : "Harvard Bioscience (Germany)",
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
         "established" : 1980,
         "external_ids" : [
            {
               "all" : [
                  "grid.67104.34"
               ],
               "preferred" : "grid.67104.34",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0415 0102"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5676515"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01zxdeg39",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.harvardpilgrim.org/portal/page?_pageid=1391,1&_dad=portal&_schema=PORTAL"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Harvard_Pilgrim_Health_Care"
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
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard Pilgrim Health Care"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
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
         "established" : 1993,
         "external_ids" : [
            {
               "all" : [
                  "grid.488688.2"
               ],
               "preferred" : "grid.488688.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0422 1863"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/01va8fr66",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.baiminstitute.org/"
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
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Baim Institute for Clinical Research"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Harvard Clinical Research Institute"
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
         "established" : 1996,
         "external_ids" : [
            {
               "all" : [
                  "100006781"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.453479.8"
               ],
               "preferred" : "grid.453479.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0627 108X"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/04q644746",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.armeniseharvard.org/"
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
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Giovanni Armenise-Harvard Foundation"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
            "nonprofit"
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
         "established" : 1990,
         "external_ids" : [
            {
               "all" : [
                  "100013721"
               ],
               "preferred" : "100013721",
               "type" : "fundref"
            },
            {
               "all" : [
                  "Q7300869"
               ],
               "preferred" : "Q7300869",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05b0g2v72",
         "links" : [
            {
               "type" : "website",
               "value" : "https://rcc.harvard.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Real_Colegio_Complutense"
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
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "RCC at Harvard University"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "RCCHU"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Real Colegio Complutense"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Real Colegio Complutense at Harvard University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "parent"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "501100001206"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.467421.2"
               ],
               "preferred" : "grid.467421.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 5900 2457"
               ],
               "preferred" : "0000 0004 5900 2457",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/04ms16277",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.harvardclub.org.au/"
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
               "value" : "HCA"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard Club of Australia"
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
               "date" : "2021-09-23",
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
                  "100018332"
               ],
               "preferred" : "100018332",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.512289.5"
               ],
               "preferred" : "grid.512289.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9488 0205"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/055avk103",
         "links" : [
            {
               "type" : "website",
               "value" : "https://globalhealth.harvard.edu/"
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
                  "acronym"
               ],
               "value" : "HGHI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard Global Health Institute"
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
                  "100008035"
               ],
               "preferred" : "100008035",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.511171.2"
               ],
               "preferred" : "grid.511171.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q40771227"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04kj1hn59",
         "links" : [
            {
               "type" : "website",
               "value" : "https://hsci.harvard.edu/"
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
                  "acronym"
               ],
               "value" : "HSCI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard Stem Cell Institute"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "facility",
            "funder"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 27,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 2,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 1,
            "id" : "af",
            "title" : "Africa"
         },
         {
            "count" : 1,
            "id" : "oc",
            "title" : "Oceania"
         }
      ],
      "countries" : [
         {
            "count" : 27,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 2,
            "id" : "de",
            "title" : "Germany"
         },
         {
            "count" : 1,
            "id" : "au",
            "title" : "Australia"
         },
         {
            "count" : 1,
            "id" : "bw",
            "title" : "Botswana"
         }
      ],
      "statuses" : [
         {
            "count" : 29,
            "id" : "active",
            "title" : "active"
         },
         {
            "count" : 1,
            "id" : "inactive",
            "title" : "inactive"
         },
         {
            "count" : 1,
            "id" : "withdrawn",
            "title" : "withdrawn"
         }
      ],
      "types" : [
         {
            "count" : 15,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 6,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 6,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 6,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 5,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 5,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 2,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 1,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 31,
   "time_taken" : 4
}
```

## Example

Search for an _active_, _inactive_, or _withdrawn_ ROR record corresponding to a particular GRID ID.

```curl
curl 'https://api.ror.org/v2/organizations?query=%22grid.419291.6%22&all_status' | json_pp
```

The response returns the inactive record for the Rowland Institute at Harvard that contains the searched-for identifier in `external_ids`.

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
         "established" : 1980,
         "external_ids" : [
            {
               "all" : [
                  "100009835"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.419291.6"
               ],
               "preferred" : "grid.419291.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0384 6984"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7372153"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03eta9142",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www2.rowland.harvard.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Rowland_Institute_for_Science"
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
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Rowland Institute at Harvard"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
         "types" : [
            "funder",
            "nonprofit"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 1,
            "id" : "na",
            "title" : "North America"
         }
      ],
      "countries" : [
         {
            "count" : 1,
            "id" : "us",
            "title" : "United States"
         }
      ],
      "statuses" : [
         {
            "count" : 1,
            "id" : "inactive",
            "title" : "inactive"
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
            "id" : "nonprofit",
            "title" : "nonprofit"
         }
      ]
   },
   "number_of_results" : 1,
   "time_taken" : 3
}
```

# Technical details

The query parameter searches abbreviated Elasticsearch documents (called `names_ids`) that combine all the values from each ROR record's `names`  and `external_ids` fields. Only the values, not the language code or type, are included. Field names are removed, and each value is simply categorized as a "name" or an "id".

## Example

Here is a truncated example of a `names_ids` document searched by the query parameter.

```json
"names_ids" : [
            {
              "name" : "University of Wisconsin–Madison"
            },
            {
              "name" : "Université du Wisconsin à Madison"
            },
            {
              "name" : "Universidad de Wisconsin-Madison"
            },
            {
              "name" : "UW–Madison"
            },
            {
              "name" : "UW"
            },
            {
              "id" : "https://ror.org/01y2jtd41"
            },
            {
              "id" : "ror.org/01y2jtd41"
            },
            {
              "id" : "01y2jtd41"
            },
...
```
