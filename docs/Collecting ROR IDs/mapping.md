---
title: Map other organization IDs to ROR IDs
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    This document explains how to map other organization IDs to ROR IDs using
    the ROR API or data dump, including tips for implementation and specific use
    cases like migrating from GRID to ROR.
  robots: index
---
Many ROR records contain equivalent organization IDs of other types in the `external_ids` section. This allows you to find the equivalent ROR ID for several other organization ID types. 

Organization ID types you'll find in ROR records include:

* Crossref Funder ID (formerly FundRef)
* GRID
* ISNI
* Wikidata

Not all records contain all of the ID types above in `external_ids`. In some cases, an equivalent ID may not exist in that identifier type. 

> 📘 Migrating from GRID to ROR
>
> **GRID published its final public release on 16 Sep 2021** (see [GRID/ROR Transition FAQ](doc:grid)) 
>
> If you're migrating to ROR from GRID, the good news is that [ROR's JSON data structure](doc:ror-data-structure) is identical to GRID's *and* every GRID ID has a one-to-one match to a ROR ID!
>
> The latest ROR data is available from Zenodo, and you can [download the ROR data dump using the Zenodo API](doc:data-dump#download-ror-data-dumps-programmatically-with-the-zenodo-api).
>
> You can also find a list of ROR IDs and their equivalent GRID IDs from the Sep 2021 ROR data dump as a CSV file at [https://doi.org/10.5281/zenodo.5534785](https://doi.org/10.5281/zenodo.5534785).

## Map other IDs to ROR using the API

This guide provides tips for implementing the ROR API for this specific use case. See the [REST API guide](doc:rest-api) for full information about the ROR API.

### Map a single ID to ROR

Search for a ROR record containing an equivalent ID of another type in the `external_ids` field by using quotes around the ID in a query. 

The [Query parameter](doc:api-query) search approach includes the `external_ids` field and so can be used to search for ROR records that match an external identifier. Use URL-encoded quotation marks before and after the identifier search string for best results.

Because the query parameter search does not search relationships fields, only the record with the searched-for identifier in the `external_ids` field will be returned.

```curl
curl 'https://api.ror.org/v2/organizations?query=%22grid.14003.36%22' | json_pp
```

If a match is found, the response will contain 1 or more full ROR records. You'll find the corresponding ROR ID(s) in `items[n].id.`

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
   "time_taken" : 7
}
```

### Map a list of IDs to ROR

If you have a list of other IDs you'd like to map to ROR, automate this search with a script. See our [ror-utilities GitHub repository](https://github.com/ror-community/ror-utilities) for an example Python script built for version 1 of the ROR API that accepts a list of other IDs as a CSV file and returns a CSV file with corresponding ROR IDs.

### Mapping tips

* In `external_ids`, each ID type has 2 fields: "all" and "preferred". For many external IDs, "preferred" is null even when "all" has only 1 value, so we recommend that you *don't* limit your mapping to only external IDs designated as "preferred".
* Mapping many thousands of organizations using a script that calls the ROR API can take many hours. If you require a faster approach, you may want to use the [data dump,](doc:data-dump) or an incremental approach, such as "just in time" mapping to ROR when organization data is read from or written to your system.

### Find all ROR records mapped to a certain ID type

You can retrieve a list of all ROR records with a certain external ID type. 

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=external_ids.type:wikidata' | json_pp
```

Returns a list of all records in ROR mapped to a Wikidata ID. 

## Map other IDs to ROR using the data dump

You can also use the [ROR data dump](doc:data-dump) to match other IDs to their equivalent ROR IDs using your own scripts or processing tools. Advantages of this approach include:

* Fine-grained control over matching criteria
* Faster processing in cases where you have many IDs to map
* No chance of error responses due to network interruptions or API outages
* Data available in both JSON and CSV format

## Mapping Ringgold IDs to ROR

Since Ringgold is a proprietary identifier system, we're currently not able to include Ringgold IDs in ROR records. In the meantime, these approaches are available:

* **If you have access to the Ringgold Identify database/API**, you can map Ringgold IDs to ROR IDs using ISNI as an intermediary, as ISNI IDs are included in both ROR and Ringgold records.
* **If you do not have access to the Ringgold Identify database/API**, you may be able to map some Ringgold IDs to ROR IDs using Wikidata as an intermediary, as many ROR records contain Wikidata IDs and some Wikidata records contain Ringgold IDs that were derived from the 2019 ORCID public data file and published in [http://doi.org/10.5281/zenodo.3241717](http://doi.org/10.5281/zenodo.3241717). See the script at [https://github.com/ror-community/ror-utilities/blob/main/general-scripts/map-ringgold-via-wikidata.py](https://github.com/ror-community/ror-utilities/blob/main/general-scripts/map-ringgold-via-wikidata.py) for a Python script that performs this matching.

## Other external identifiers

ROR's current policy is to include only globally unique persistent identifiers as corresponding external IDs in ROR records, which means that national and regional identifiers such as UEI and PIC are not included. If you would benefit from having these or other identifiers in the ROR record, however, you may submit a schema change request on our [roadmap](https://github.com/ror-community/ror-roadmap) or add a comment to an existing request. 

* [Add European Union Participant Identifier Code (PIC)](https://github.com/ror-community/ror-roadmap/issues/97) - GitHub roadmap request for EU PIC mapping. Includes a link in the comments to a third-party-generated PIC-to-ROR mapping project that uses CORDIS data. 
* [Add Unique Entity ID (UEI) tags as external IDs](https://github.com/ror-community/ror-roadmap/issues/203) - GitHub roadmap request for UEI mapping. Includes a third-party-generated spreadsheet in the comments that crosswalks over 950 SAM UEI, CAGE codes, DUNS numbers, and GRID IDs.
* [US IPEDs mapping file](https://github.com/opensyllabus/institution-identifiers) - GitHub repository from the [OpenSyllabus](https://www.opensyllabus.org/) project with a CSV file that maps ROR IDs for records of type "Education" to GRID, Wikidata, and IPEDs identifiers.
* [RRID to ROR mapping file](https://docs.google.com/spreadsheets/d/e/2PACX-1vSWoD7q1vo4j8UC15ok2uC25DfUd87em1fihCMTXeSedK1U_wqn79VO1JqpiLrtI64kGKa3nyVlHxtz/pubhtml) - Spreadsheet of mappings between RRIDs and ROR IDs as of July 2024. See also ["Understanding RRID and ROR for Facilities."](https://ror.org/blog/2024-11-26-rrid-ror-facilities/)