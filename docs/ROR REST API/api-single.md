---
title: Retrieve a single record
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    This document explains how to format single ROR ID requests and provides
    examples of retrieving a single record for an organization using the ROR
    API.
  robots: index
---
# Formatting single ROR ID requests

When retrieving a single record, you can use any of these formats in the ID portion of your request:

* **Full ROR ID URL:**  [https://ror.org/015w2mp89](https://ror.org/015w2mp89)
* **Domain and ID:** ror.org/015w2mp89
* **ID only:** 015w2mp89

All formats will return the same record. See [ROR identifier pattern](doc:identifier) for more on ROR ID formats.

> 📘 Single ROR ID request format
>
> `https://api.ror.org/v2/organizations/[ror id]`

## Example

Request the ROR record for Keimyung University.

```curl
curl 'https://api.ror.org/v2/organizations/00tjv0s33' | json_pp
```

The response is a JSON object containing a full ROR record. See [ROR data structure](doc:ror-data-structure) for details about the fields and values in a ROR record.

```json
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
   "established" : 1899,
   "external_ids" : [
      {
         "all" : [
            "501100002509",
            "501100006017"
         ],
         "preferred" : "501100002509",
         "type" : "fundref"
      },
      {
         "all" : [
            "grid.412091.f"
         ],
         "preferred" : "grid.412091.f",
         "type" : "grid"
      },
      {
         "all" : [
            "0000 0001 0669 3109"
         ],
         "preferred" : null,
         "type" : "isni"
      },
      {
         "all" : [
            "Q483402"
         ],
         "preferred" : null,
         "type" : "wikidata"
      }
   ],
   "id" : "https://ror.org/00tjv0s33",
   "links" : [
      {
         "type" : "website",
         "value" : "http://www.kmu.ac.kr/main.jsp"
      },
      {
         "type" : "wikipedia",
         "value" : "http://en.wikipedia.org/wiki/Keimyung_University"
      }
   ],
   "locations" : [
      {
         "geonames_details" : {
            "continent_code" : "AS",
            "continent_name" : "Asia",
            "country_code" : "KR",
            "country_name" : "South Korea",
            "country_subdivision_code" : "27",
            "country_subdivision_name" : "Daegu",
            "lat" : 35.87028,
            "lng" : 128.59111,
            "name" : "Daegu"
         },
         "geonames_id" : 1835329
      }
   ],
   "names" : [
      {
         "lang" : null,
         "types" : [
            "acronym"
         ],
         "value" : "KMU"
      },
      {
         "lang" : null,
         "types" : [
            "alias"
         ],
         "value" : "Kei-dae"
      },
      {
         "lang" : "en",
         "types" : [
            "ror_display",
            "label"
         ],
         "value" : "Keimyung University"
      },
      {
         "lang" : "ko",
         "types" : [
            "label"
         ],
         "value" : "계명대학교"
      }
   ],
   "relationships" : [
      {
         "id" : "https://ror.org/035r7hb75",
         "label" : "Keimyung University Dongsan Medical Center",
         "type" : "related"
      }
   ],
   "status" : "active",
   "types" : [
      "education",
      "funder"
   ]
}
```

# Status and single ROR ID requests

Although [as of December 2022](https://ror.readme.io/changelog/2022-12-01-organization-status-changes) the ROR API defaults to returning only records whose status is _active_ in lists of results, requests for a single specific record by its ROR ID will always return the record even when its status is _inactive_ or _withdrawn_. Read more about [status in ROR data](doc:data-structure#status).

## Example

```curl
curl 'https://api.ror.org/v2/organizations/03fyd9f03' | json_pp
```

Returns the inactive record for Alcatel-Lucent (United Kingdom).

```json
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
            "grid.425346.2"
         ],
         "preferred" : "grid.425346.2",
         "type" : "grid"
      },
      {
         "all" : [
            "0000 0004 0626 3995"
         ],
         "preferred" : null,
         "type" : "isni"
      },
      {
         "all" : [
            "Q28975935"
         ],
         "preferred" : null,
         "type" : "wikidata"
      }
   ],
   "id" : "https://ror.org/03fyd9f03",
   "links" : [
      {
         "type" : "website",
         "value" : "https://www5.alcatel-lucent.com/"
      },
      {
         "type" : "wikipedia",
         "value" : "https://en.wikipedia.org/wiki/Alcatel-Lucent"
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
         "value" : "Alcatel-Lucent (United Kingdom)"
      }
   ],
   "relationships" : [
      {
         "id" : "https://ror.org/00zpf0626",
         "label" : "Nokia (United Kingdom)",
         "type" : "successor"
      }
   ],
   "status" : "inactive",
   "types" : [
      "company"
   ]
}
```
