---
title: Schema 2.0
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR schema v2.0
  description: >-
    The document outlines major changes to the ROR metadata schema based on
    community feedback, including consolidating name-related fields, simplifying
    location information, updating web domain details, revising external
    identifiers, and adding created/last modified dates to records. These
    changes are expected to be implemented in version 2.0 of the ROR metadata
    schema by the end of 2023.
  robots: index
next:
  description: ''
---
> 👍 Recommended schema
>
> ROR [Schema 2.1](doc:schema-v2-1) is the current recommended, stable schema. It is the version used by the ROR [REST API](doc:rest-api), the ROR [Web search](doc:web-search), and in the ROR [Data dump](doc:data-dump). See [Data structure](doc:ror-data-structure) for documentation of the current recommended schema.
>
> JSON schema documents used to generate and validate ROR records are available at [https://github.com/ror-community/ror-schema](https://github.com/ror-community/ror-schema).

# Community feedback

In the spring of 2023, ROR asked for community feedback on the specifics of major, breaking changes to the ROR metadata schema.

* [ROR Schema v2.0 draft proposal](https://docs.google.com/document/d/1JNDMoKmjR2y0quWXwFfoJTsIttbltJVN0l5Wddw1cIk/edit?usp=sharing) - proposal open for comment through February 2023
* [ROR Schema v2.0 second draft proposal](https://docs.google.com/document/d/18Qg6-lv2Fxkc97SLpD8gdS0V8p0y9fdaZEywyeyKJWM/edit?usp=sharing) - proposal open for comment through April 2023
* [ROR Schema v2.0 final proposal](https://docs.google.com/document/d/1lYybpmtFW3cSitNAUzuVgFieco17PfuIbeVlcqcwync/edit?usp=sharing) - proposal adopted April 2023

# Planned changes

Work to implement the below agreed-upon changes to the current ROR metadata schema will continue throughout 2023, and version 2.0 of the ROR metadata schema is expected to be released in the last quarter of 2023 or early in 2024. The current version (unofficially v1.0) will be maintained for at least a year after the release of v2.0.

Read more about how ROR will handle [Schema versions](doc:schema-versions).

## Name information (name, acronyms, aliases, labels)

ROR records in version 1 currently include 4 separate fields that represent variations of an organization’s name: `name`, `aliases`, `labels` and `acronyms`.  User-reported problems with these fields include

* the lack of language code in the `name` field, which makes it difficult to utilize ROR data in internationalized applications or in cases where names need to be displayed in a single preferred language, and
* issues with the concept of a "primary" name that can have only one value, since in multilingual countries  such as Canada and Switzerland it is important that names in different languages be given equal weight.

We are therefore "flattening" all name-related fields into one `names` field that can hold an array of values, types, and languages. For use cases where a single "default" name for an organization is desired, we have added the `ror_display` type to indicate that this is the name that ROR has chosen to display in its [web-based search](https://ror.org/search).

### Current v1.0 example

```json
"acronyms" : [  
      "UC"  
   ],  
"aliases" : ["UC System"],  
"labels" : [  
      {  
         "iso639" : "es",  
         "label" : "Universidad de California"  
      },  
      {  
         "iso639" : "fr",  
         "label" : "Université de Californie"  
      }  
   ],  
"name" : "University of California System",
```

### Forthcoming v2.0 example

```json
"names" : [  
  {  
    "value" : "UC",  
    "types": ["acronym"],  
    "lang" : "en"  
  },  
  {  
    "value" : "UC System",  
    "types": ["alias"],  
    "lang" : "en"  
  },  
  {  
    "value" : "University of California System",  
    "types": ["ror_display", "label"],  
    "lang" : "en"  
  },  
  {  
    "value" : "Université de Californie",  
    "types": ["label"],  
    "lang" : "fr"  
  }  
]
```

## ​​​​Location information (addresses, country)

The current `addresses` field contains data from [GeoNames](https://www.geonames.org/). It contains several sub-fields that contain no values and are therefore not usable, and the ROR team also spends a disproportionate amount of time handling issues with validating and retrieving very granular GeoNames data within `addresses` (e.g., `geonames_admin2`, `nuts_level1`, `nuts_level2`, etc.) that users could easily retrieve themselves directly from GeoNames using the GeoNames ID provided in each ROR record. There is also a `country` field in each ROR record that duplicates the country information in the `addresses` field.

We are therefore removing empty or overly detailed GeoNames sub-fields within the ROR record, removing the `country` field, and adding a `locations` field that will contain the most important and universally applicable location information.

> 📘 Additional location information added in v2.1
>
> See [Schema 2.1](doc:schema-2-1) for additional location information added in December 2024 corresponding to state-level and continental locations.

### Current v1.0 example

```json
"addresses" : [
      {
         "city" : "Oakland",
         "country_geonames_id" : 6252001,
         "geonames_city" : {
            "city" : "Oakland",
            "geonames_admin1" : {
               "ascii_name" : "California",
               "code" : "US.CA",
               "id" : 5332921,
               "name" : "California"
            },
            "geonames_admin2" : {
               "ascii_name" : "Alameda County",
               "code" : "US.CA.001",
               "id" : 5322745,
               "name" : "Alameda County"
            },
            "id" : 5378538,
            "license" : {
               "attribution" : "Data from geonames.org under a CC-BY 3.0 license",
               "license" : "http://creativecommons.org/licenses/by/3.0/"
            },
            "nuts_level1" : {
               "code" : null,
               "name" : null
            },
            "nuts_level2" : {
               "code" : null,
               "name" : null
            },
            "nuts_level3" : {
               "code" : null,
               "name" : null
            }
         },
         "lat" : 37.802168,
         "line" : null,
         "lng" : -122.271281,
         "postcode" : null,
         "primary" : false,
         "state" : "California",
         "state_code" : "US-CA"
      }
   ]
```

### Forthcoming v2.0 example

```json
"locations" : [
      {
         "geonames_id" : 5378538,
         "geonames_details" : {
             "country_code" : "US",
             "country_name" : "United States",
             "lat" : 37.802168,
             "lng" : -122.271281,
             "name": "Oakland" 
          }
       }
   ]
```

## Web domain information (links, ip_addresses, email_address, wikipedia_url)

ROR contains four fields related to an organization’s web domain/presence (`links`, `ip_address`, `email_address` and `wikipedia_url`), two of which have never had any values (`ip_address` and `email_address`). URLs in the field `links` are inconsistently formatted, and there is no consensus across the data about what value this field should represent (a home page? an About page? something else?).

Further, use cases from the community point to the domain registered to a particular institution (not including protocol, path portions, query parameters, etc.) as being much more useful than a full URL, as it can be unambiguously mapped to records in other services, such as identity management systems. Also, while full URLs may fail to resolve over time, domains are much more persistent, and certainly more persistent than IP addresses, given the rise of public cloud computing.

We are therefore removing the `ip_addresses` and `email_addresses` fields, adding a `domains` field, and creating a `links` field to store organization websites and Wikipedia pages.

### Current v1.0 example

```json
"email_address" : "",
"ip_addresses" : [],
"links" : [
      "http://www.universityofcalifornia.edu/"
   ],
"wikipedia_url":"http://en.wikipedia.org/wiki/University_of_California"
```

### Forthcoming v2.0 example

```json
"domains" : [
      "universityofcalifornia.edu"
   ],
"links" : [
    {
      "type": "website",
      "value": "http://www.universityofcalifornia.edu/"
    },
    {
      "type": "wikipedia",
      "value": "http://en.wikipedia.org/wiki/University_of_California"
    }
  ],
```

## External identifiers (external_ids)

The current structure of the `external_ids` field is problematic because it uses the identifier type as a field name. This means that a significant schema change is required to add a new identifier type or remove a deprecated type. ROR currently includes a significant number of records with deprecated external IDs (e.g., CNRS, HESA, and OrgRef) and has received several requests to add new external ID types.

We are therefore revising the `external_ids` field to be more flexible, with external identifier schemes expressed as type values rather than field names. We are also removing fields for deprecated identifier schemes.

### Current v1.0 example

```json
"external_ids" : {
      "FundRef" : {
         "all" : [
            "100005595",
            "100009350",
            "100004802",
            "100010574",
            "100005188",
            "100005192"
         ],
         "preferred" : "100005595"
      },
      "GRID" : {
         "all" : "grid.30389.31",
         "preferred" : "grid.30389.31"
      },
      "ISNI" : {
         "all" : [
            "0000 0001 2348 0690"
         ],
         "preferred" : null
      },
      "OrgRef" : {
         "all" : [
            "31921"
         ],
         "preferred" : null
      },
      "Wikidata" : {
         "all" : [
            "Q184478"
         ],
         "preferred" : null
      }
   }
```

### Forthcoming v2.0 example

```json
 "external_ids" : [
      {
         "type" : "fundref",
         "all" : [
            "100005595",
            "100009350",
            "100004802",
            "100010574",
            "100005188",
            "100005192"
         ],
         "preferred" : "100005595"
      },
      {
         "type" : "grid",
         "all" : [
            "grid.30389.31"
         ],
         "preferred" : "grid.30389.31"
      },
      {
         "type" : "isni",
         "all" : [
            "0000 0001 2348 0690"
         ],
         "preferred" : null
      },
```

## Created/last modified dates (new field)

ROR records do not currently contain created or last modified dates, which presents challenges for both data users and ROR staff. For example, data dump users cannot easily ingest only new/updated records: they must either compare a new data dump to a previous data dump in order to identify changes or ingest the entire data set each time a new version is released. Even ROR staff cannot easily identify when a change was introduced to a record based on the record data; staff must consult curation request records and GitHub release history.

We are therefore adding an `admin` field to support created and last modified dates as well as other administrative information that may be desirable in the future.

### Forthcoming v2.0 example

```json
"admin" : {
         "created" : {
            "date": "2020-04-25",
            "schema_version" : "1.0"
         },
         "last_modified” : {
           "date" : "2022-10-18",
           "schema_version" : "2.0"
         }
      }
```

<br />
