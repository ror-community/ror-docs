---
title: Data structure
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR Data Structure
  description: >-
    This document outlines the policies and definitions for top-level metadata
    elements in the ROR schema, including required fields such as organization
    ID, name, type, establishment year, relationships, addresses, status, and
    external identifiers.
  keywords:
    - schema
    - ' metadata'
  robots: index
---
> 👍 Recommended schema
>
> This page documents the current recommended, stable schema, [Schema 2.1](doc:schema-v2-1), which is used by the ROR [REST API](doc:rest-api), the ROR [Web search](doc:web-search), and in the ROR [Data dump](doc:data-dump). 
>
> JSON schema documents used to generate and validate ROR records are available at [https://github.com/ror-community/ror-schema](https://github.com/ror-community/ror-schema).


The original ROR metadata schema inherited from GRID in 2019 is now known as version 1.0. After two rounds of community feedback in 2022/2023, [metadata schema version 2.0](doc:schema-v2) was developed and beta-tested, then launched into production in April 2024. [Schema 2.1](doc:schema-v2-1), a minor update to v2 that adds additional location information, was launched in December 2024 and is the recommended and most recent version.

Version 1.0 of the ROR schema was permanently sunset in 2025. Read more about ROR [schema versioning](doc:schema-versions).

If you'd like to request a change to the ROR metadata schema, please [submit a schema change request on our roadmap](https://github.com/ror-community/ror-roadmap/issues/new/choose).

# Fields

> 📘 JSON Schema
>
> JSON schema documents used to generate and validate ROR records are available at [https://github.com/ror-community/ror-schema](https://github.com/ror-community/ror-schema).

Below are listed the top-level fields (or "elements") in the v2.1 ROR metadata schema along with their names, definitions, types, whether the field is required, and whether a value in the field is required.

Queries to the ROR API will return all fields regardless of whether they have a value. JSON will include null values and empty arrays and objects if there is no value available for the given organization. Values in fields that contain multiple values are sorted by Unicode value, which is alphabetical for characters in the Basic Latin set.

| Field name    | Definition                                                | Type   | Required | Value required | Remarks                                                                                                             |
| :------------ | :-------------------------------------------------------- | :----- | :------- | :------------- | :------------------------------------------------------------------------------------------------------------------ |
| id            | Unique ROR ID for the organization                        | String | TRUE     | TRUE           |                                                                                                                     |
| admin         | Container for administrative information about the record | Object | TRUE     | TRUE           |                                                                                                                     |
| domains       | The domains registered to a particular institution        | Array  | TRUE     | FALSE          |                                                                                                                     |
| established   | Year the organization was established (CE)                | Number | TRUE     | FALSE          |                                                                                                                     |
| external_ids  | Other identifiers for the organization                    | Array  | TRUE     | FALSE          | Allowed ID types: fundref, grid, isni, wikidata                                                                     |
| links         | The organization's website and Wikipedia page             | Array  | TRUE     | FALSE          |                                                                                                                     |
| locations     | The location of the organization                          | Array  | TRUE     | TRUE           |                                                                                                                     |
| names         | Names the organization goes by                            | Array  | TRUE     | TRUE           | Allowed name types: acronym, alias, label, ror_display                                                              |
| relationships | Related organizations in ROR                              | Array  | TRUE     | FALSE          | Allowed relationship types: related, parent, child, predecessor, successor                                          |
| status        | Whether the organization is active                        | String | TRUE     | TRUE           | Allowed values: active, inactive, withdrawn                                                                         |
| types         | Organization type                                         | Array  | TRUE     | TRUE           | Allowed organization types: education, funder, healthcare, company, archive, nonprofit, government, facility, other |

> 📘 All available fields and sub-fields
>
> See also the complete alphabetical list of [All ROR fields and sub-fields](doc:fields) in v2.1 of the ROR metadata schema.

# Definitions and policies

Policies and expanded definitions for top-level metadata elements. See also [ROR Metadata Policies on GitHub](https://github.com/ror-community/ror-updates/wiki/ROR-Metadata-Policies).

_* indicates a value is required_

## `id*`

The ROR ID for the organization, created and assigned by ROR. See [ROR identifier pattern](doc:identifier) for more information on how the ROR ID is generated and structured.

## `admin*`

A field with information about the ROR record, including the date when the record was created, the schema version in effect when the record was created, the date when the information in the record was last modified, and the schema version in effect when the information in the record was last modified.

## `domains`

The domains registered to a particular institution, not including the protocol, path portions, or query parameters that may exist in the URL for the organization's website. An organization's website is stored in the `links` field.

## `established`

The year the organization was established, written as four digits (YYYY).

## `external_ids`

Other identifiers for the organization (if available). ROR maps its IDs to four types of external identifiers: GRID, Wikidata, ISNI, and the Crossref Open Funder Registry (formerly “FundRef”).

## `links`

The primary website and Wikipedia page for the organization. Only one website URL and one Wikipedia URL should be associated with the record.

In the case of websites with translated versions that use a language suffix like “/en”, the generic URL (without the language suffix) is used as long as the website resolves without it. Otherwise, the English version will be used.

## `locations*`

The location(s) of the organization, including continent name, continent code, country name, two-letter [ISO-3166](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code, country subdivision name (e.g., Canadian province, Japanese prefecture or US state), country subdivision code, latitude, longitude, and specified location (usually city) name. Location data comes from [GeoNames](https://www.geonames.org/).

A city or specified location is required to populate corresponding data from GeoNames, such as latitude, longitude, country, country subdivision, and continent.

GeoNames data is licensed under a [Creative Commons Attribution 3.0 license](http://creativecommons.org/licenses/by/3.0/).

## `names*`

Names for the organization, including four types of names: acronyms, aliases, labels, and the name ROR displays on its [web search interface](https://ror.org/search). Names may have multiple types.

Each name variant has a corresponding language tag with the two-letter [ISO-639](https://www.loc.gov/standards/iso639-2/php/code_list.php) language code where available. The language is not known for all name variants, so corresponding language codes may be null for some names. ROR name metadata fields support multiple languages and character sets.

* ### `acronyms`

  One or more official acronyms or initialisms for the organization, typically consisting of the first letters of the words in the organization name (e.g., "UCLA" for “University of California, Los Angeles”).

* ### `aliases`

  Used for alternate forms of the organization name that may be now or previously in common use but are not official according to the organization's current website or policy (e.g., "London School of Economics" for "London School of Economics and Political Science"). This field may include both current and historical name variants. ROR does not currently identify which aliases are current versus historical, but future iterations of the ROR schema may differentiate between the two.

* ### `labels`

  Displays equivalent forms of the organization name in one or more languages.

* ### `ror_display`

  The name of the organization displayed most prominently on records in ROR's [web search](https://ror.org/search).

  With the release of v2 of ROR's schema, ROR no longer assigns a "primary" name for any organization, since these decisions are highly subjective and are often inappropriately Anglocentric. Additionally, a single “primary” name in only one language is inappropriate for organizations located in countries with multiple official languages.

  At the same time, many ROR users require a default name for use in their applications. Therefore, each record will have exactly one name of type _ror_display_. The _ror_display_ name will be used as the main heading on the organization record in the ROR web search and can be used by those who want to select exactly one name for use in their applications.

  The primary data source for the _ror_display_ name and the formatting thereof is the organization's website. This may be different than the organization's legal name. Many of the name values in _ror_display_ are in English due to legacy data, but _ror_display_ names may also be in non-English languages, so long as said names are also written in Latin characters. Display limitations in many users' systems require this constraint, but as indicated in the _labels_ section, all label forms, including those in non-Latin character scripts, are considered equivalent.

## `relationships`

One or more organizations in ROR that the organization is related to.

Five types of relationships are supported: `parent`, `child`, `related`, `predecessor`, and `successor`. An organization can have multiple relationships, but each relationship must be expressed as one of these relationship types. Note that inverse and corresponding relationships are significantly affected by the type of relationship and by record [status](doc:data-structure#status).

### `parent`, `child`, and `related`

`Parent` / `child` relationships indicate a relationship where the parent exercises control (supervisory, administrative, or financial) over the child, or the child is a component of the parent entity, like a research center within a university. The `related` relationship type denotes less defined connections, such as resource sharing or participation without direct control.

**Active records:** A `parent`, `child`, or `related` relationship in an active record will always have a corresponding relationship in the related active record. For example, if Organization A's record contains a relationship to Organization B with type `parent` (and both records have status `active`), Organization B's record must contain a corresponding relationship to Organization A with type `child`. If Organization C contains a relationship to Organization D with type `related` (and both records have status `active`), then Organization D must also contain a corresponding relationship with type `related`.

**Inactive and withdrawn records:** When the status of a record is set to `inactive`, any `parent`, `child`, and `related` relationships are **retained** in the inactive record as a tombstone and **removed** from related active records. An active record must not contain a  `parent`, `child`, or `related` relationship to a record with a status of `inactive` or `withdrawn` since the relationship is no longer considered current. However, a record with a status of `inactive` may contain a `parent`, `child`, or `related` relationship to a record with a status of `active` in order to preserve the history of the organizational relationship. Records with a status of `withdrawn` generally do not retain `parent`, `child`, and `related` relationships.

### `predecessor` and `successor`

`Successor` and `predecessor` relationships track organizational continuity and are used when an entity ceases operations or to redirect from erroneous records to correct ones.

A `successor` relationship indicates that an organization continues the work of a `predecessor` organization that has ceased operations. If an organization simply changes its name, it will not receive a `successor` relationship; instead, the `names` field of the record for the organization will be modified with the new name added and the previous name retained. The `successor` relationship also appears in records with a status of `withdrawn` that were added to ROR in error in order to point users to the correct record in ROR.

The `predecessor` or `successor` relationship in a record may have a corresponding relationship in the related record, but the corresponding relationship is not required. For example, if Organization A shuts down and a relationship to Organization B with type `successor` is added to the inactive record for Organization A, the record for Organization B may or may not contain a corresponding relationship to Organization A with type `predecessor`.

The `successor` and `predecessor` relationship types can appear on records with any status, although in most cases records with a `successor` will have a status of `inactive` or `withdrawn` and records with a `predecessor` will have a status of `active`.

Depending on your use case, you may wish to search for ROR records with a status of `inactive` or `withdrawn` in your database or application and replace them with the record(s) indicated by the `successor` relationship where available.

If you are depositing DOI records, the name & ROR ID of the organization should in general be kept historically correct. In other words, if an organization publishes content under a certain name and ROR ID, the name and ROR ID should remain the same in DOI metadata even if the organization later becomes inactive or is merged into a successor organization. Predecessor and successor relationships in ROR records ensure that research can still be tracked despite organizational changes.

## `status*`

Indication of whether the organization is active or not, based on a controlled list of status values. Records can be active, inactive, or withdrawn.

* ### `active`
  An organization that is actively producing research outputs.
* ### `inactive`
  An organization that has ceased operation or producing research outputs.
* ### `withdrawn`
  A record that was created in error, such as a duplicate record or a record that is not in scope for the registry.

A record with a status of `inactive` or `withdrawn` may have one or more Successor organizations listed in its relationships. Successor relationships indicate that another organization continues the work of an organization that has become inactive or has been withdrawn. See [relationships](https://ror.readme.io/docs/ror-data-structure#/relationships)  for more information.

## `types*`

The type of organization based on a controlled list of categories. An organization always has a type. ROR metadata can support multiple types for a given organization. Allowed types are `education`, `funder`, `healthcare`, `company`, `archive`, `nonprofit`, `government`, `facility`, and `other`.

* ### `education`
  A university or similar institution involved in providing education and educating/employing researchers.
* ### `funder`
  An organization that awards research funds or provides in-kind support. All records that are mapped to a Funder ID will be assigned this type, usually in conjunction with an additional organization type.
* ### `healthcare`
  A medical care facility such as hospital or medical clinic. Excludes medical schools, which should be categorized as “Education”.
* ### `company`
  A private for-profit corporate entity involved in conducting or sponsoring research.
* ### `archive`
  An organization involved in stewarding research and cultural heritage materials. Includes libraries, museums, and zoos.
* ### `nonprofit`
  A non-profit and non-governmental organization involved in conducting or funding research.
* ### `government`
  An organization that is part of or operated by a national or regional government and that conducts or supports research.
* ### `facility`
  A specialized facility where research takes place, such as a laboratory or telescope or dedicated research area.
* ### `other`
  A category for any organization that does not fit the categories above.

# Example record

See the JSON structure in an example organization record.

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
            "501100008427"
         ],
         "preferred" : null,
         "type" : "fundref"
      },
      {
         "all" : [
            "grid.479785.0"
         ],
         "preferred" : "grid.479785.0",
         "type" : "grid"
      }
   ],
   "id" : "https://ror.org/05wgann87",
   "links" : [
      {
         "type" : "website",
         "value" : "https://www.deutsches-stiftungszentrum.de/"
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
            "acronym"
         ],
         "value" : "DSZ"
      },
      {
         "lang" : "de",
         "types" : [
            "ror_display",
            "label"
         ],
         "value" : "Deutsches Stiftungszentrum"
      }
   ],
   "relationships" : [
      {
         "id" : "https://ror.org/01f7ent21",
         "label" : "Hermann und Lilly Schilling-Stiftung",
         "type" : "child"
      }
   ],
   "status" : "active",
   "types" : [
      "funder",
      "nonprofit"
   ]
}
```

<br />

<br />
