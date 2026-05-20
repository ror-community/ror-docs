---
title: Schema 1.0
deprecated: false
hidden: false
metadata:
  robots: index
---
<Callout icon="❗️" theme="error">
  ## Schema 1.0 has been sunset

  ROR schema and API v1 were permanently sunset in December 2025 and are no longer supported. [Read more on the ROR changelog.](changelog:2025-12-04-version-1-of-the-ror-api-and-schema-is-sunsetting)
</Callout>

<Callout icon="👍" theme="okay">
  ## Recommended schema

  ROR [Schema 2.1](doc:schema-v2-1) is the current recommended, stable schema. It is the version used by the ROR [REST API](doc:rest-api), the ROR [Web search](doc:web-search), and in the ROR [Data dump](doc:data-dump). See [Data structure](doc:ror-data-structure) for documentation of the current recommended schema.

  JSON schema documents used to generate and validate ROR records are available at [https://github.com/ror-community/ror-schema](https://github.com/ror-community/ror-schema).
</Callout>

Version 1 of ROR's data structure (aka its "metadata schema" or "JSON schema") is based on [Digital Science's GRID](https://digitalscience.figshare.com/search?q=GRID\&itemTypes=3), which provided the original seed data for the registry. GRID retired its public releases as of 16 Sep 2021, and ROR began managing its data independently from GRID in March 2022.

After two rounds of community feedback in 2022/2023, [metadata schema version 2.0](doc:schema-v2) was developed and launched in April 2024, after which the ROR metadata schema inherited from GRID in 2019 became known as version 1.0. [Schema 2.1](doc:schema-2-1) was deployed in December 2024 and is the recommended and most recent version of ROR's [Data structure](doc:ror-data-structure). Version 1.0 of the ROR schema was sunset in December 2025 in accordance with ROR's policies on metadata [schema versioning](doc:schema-versions).

# Fields

Below are listed the top-level fields (or "elements") in ROR metadata schema v1 along with their names, definitions, types, whether the field is required, and whether a value in the field is required. For information on all fields and subfields in v1 of the ROR metadata schema, see the below example record or contact [support@ror.org](mailto:support@ror.org).

| Field name    | Definition                                        | Type   | Required | Value required | Remarks                                                                                                                                                     |
| :------------ | :------------------------------------------------ | :----- | :------- | :------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id            | Unique ROR ID for the organization                | String | TRUE     | TRUE           |                                                                                                                                                             |
| name          | The primary name of the organization              | String | TRUE     | TRUE           |                                                                                                                                                             |
| email_address | A contact mail address for the organization       | String | FALSE    | FALSE          | Deprecated field - Not actively curating                                                                                                                    |
| ip_addresses  | IP address(es) associated with the organization   | Array  | FALSE    | FALSE          | Deprecated field - Not actively curating                                                                                                                    |
| established   | Year the organization was established (CE)        | Number | TRUE     | FALSE          |                                                                                                                                                             |
| types         | Organization type                                 | Array  | TRUE     | TRUE           | Allowed types: Education, Healthcare, Company, Archive, Nonprofit, Government, Facility, Funder, Other                                                      |
| relationships | Related organizations in ROR                      | Array  | TRUE     | FALSE          | Allowed relationship types: Parent, Child, Related, Predecessor, Successor                                                                                  |
| addresses     | The organization's location                       | Array  | TRUE     | TRUE           |                                                                                                                                                             |
| links         | Official website of the organization              | Array  | TRUE     | FALSE          |                                                                                                                                                             |
| aliases       | Other names the organization is known by          | Array  | TRUE     | FALSE          |                                                                                                                                                             |
| acronyms      | Acronyms or initialisms for the organization name | Array  | TRUE     | FALSE          |                                                                                                                                                             |
| status        | Whether the organization is active or not         | String | TRUE     | TRUE           | Allowed values: Active, Inactive, Withdrawn                                                                                                                 |
| wikipedia_url | Wikipedia link for the organization               | String | TRUE     | FALSE          |                                                                                                                                                             |
| labels        | Name(s) for the organization in other language(s) | Array  | TRUE     | FALSE          |                                                                                                                                                             |
| country       | Country where organization is located             | Object | TRUE     | TRUE           |                                                                                                                                                             |
| external_ids  | Other identifiers for the organization            | Array  | TRUE     | FALSE          | Allowed external IDs: Crossref Funder ID (FundRef), ISNI, Wikidata. Other external IDs not actively curated include  GRID, OrgRef, HESA, UCAS, UKPRN, CNRS. |

<br />

# Definitions and policies

Policies and expanded definitions for top-level metadata elements. See also [ROR Metadata Policies on GitHub](https://github.com/ror-community/ror-updates/wiki/ROR-Metadata-Policies).

_* indicates a value is required_

## `id*`

The ROR ID for the organization, created and assigned by ROR. See [ROR identifier pattern](doc:identifier) for more information on how the ROR ID is generated and structured.

## `name*`

The official name of the organization used for affiliation purposes (this may be different than the organization's legal name).

Only one name can be included in the primary name field. ROR metadata includes additional fields (`aliases`, `labels`, `acronyms`) for other versions of an organization’s name so that these versions of the name can also be represented in the metadata record and so that the organization can be discoverable in the registry no matter which version of a name is input by a user in searches and API queries.

The primary name field defaults to English when an English version of an organization’s name is available, but primary names may also be in non-English languages.

ROR metadata can support multiple languages and character sets. However, the primary name field uses Latin (Roman) characters only.

The primary data source for the organization name and formatting thereof is the organization's website.

## `email_address`

This field is deprecated and is empty or null in ROR records. It will be removed in version 2 of the ROR schema.

## `ip_addresses`

This field is deprecated and is empty or null in ROR records. It will be removed in version 2 of the ROR schema.

## `established`

The year the organization was established, written as four digits (YYYY).

## `types*`

The type of organization based on a controlled list of categories. An organization always has a type. ROR metadata can support multiple types for a given organization, but in most cases, there will just be one type associated with the organization.

Based on the available information about the organization, curators will use their judgement in determining the appropriate category to assign.

Allowed types:

**Education**: A university or similar institution involved in providing education and educating/employing researchers

**Healthcare**: A medical care facility such as hospital or medical clinic. Excludes medical schools, which should be categorized as “Education”.

**Company**: A private for-profit corporate entity involved in conducting or sponsoring research.

**Archive**: An organization involved in stewarding research and cultural heritage materials. Includes libraries, museums, and zoos.

**Nonprofit**: A non-profit and non-governmental organization involved in conducting or funding research.

**Government**: An organization that is part of or operated by a national or regional government and that conducts or supports research.

**Facility**: A specialized facility where research takes place, such as a laboratory or telescope or dedicated research area.

**Funder**: An organization that awards research funds or provides in-kind support. All records that are mapped to a Funder ID will be assigned this type, usually in conjunction with an additional organization type.

**Other**: Use this category for any organization that does not fit the categories above.

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

## `addresses*`

Location details for the organization. Location data comes from [GeoNames](https://www.geonames.org/).

A city or specified location is required to populate corresponding data from GeoNames, such as latitude/longitude coordinates, state/province, and country. Note too the existence of the separate top-level `country` element, which contains the name of the organization's country and its two-letter ISO country code.

A significant number of sub-fields in the `addresses` field are null in v1 metadata. See [Fields and sub-fields](doc:fields) for a full list of deprecated and null value address fields.

GeoNames data is licensed under a [Creative Commons Attribution 3.0 license](http://creativecommons.org/licenses/by/3.0/).

## `links`

The primary website of the organization. Only one URL should be associated with the record.

In the case of websites with translated versions that use a language suffix like “/en”, the generic URL (without the language suffix) is used as long as the website resolves without it. Otherwise, the English version will be used.

## `aliases`

Used for one or more alternate forms of the organization name that may be used for affiliation purposes but are not considered the primary name according to official organization policy and/or for the purposes of ROR metadata handling. This field may include both current and historical name variants. ROR does not currently identify which aliases are current versus historical, but future iterations of the ROR schema may differentiate between the two.

## `acronyms`

One or more official acronyms or initialisms for the organization, typically consisting of the first letters of the words in the organization name (e.g., UCLA for “University of California, Los Angeles”).

## `status*`

Indication of whether the organization is active or not, based on a controlled list of status values. Allowed status values:

**active:** An organization that is actively producing research outputs.

**inactive:** An organization that has ceased operation or producing research outputs.

**withdrawn:** A record that was created in error, such as a duplicate record, or a record that is not in scope for the registry.

A record with a status of `inactive` or `withdrawn` may have one or more Successor organizations listed in its relationships. Successor relationships indicate that another organization continues the work of an organization that has become inactive or has been withdrawn. See [relationships](#relationships) for more information.

_Note: Prior to 1 Dec 2022, all records had a status of active. Read more about this change in the [Dec 1 2022 release notes](https://ror.readme.io/changelog/2022-12-01-organization-status-changes)_

## `wikipedia_url`

A Wikipedia page for the organization. Only one URL should be associated with the record.

## `labels`

Displays versions of the organization name in one or more languages other than that used in the primary `name` field, with a corresponding language tag based on the two-letter [ISO-639](https://www.loc.gov/standards/iso639-2/php/code_list.php) code.

## `country*`

Includes the name and the two-letter [ISO-3166](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code for the organization's primary country.

## `external_ids`

Other identifiers for the organization (if available).

ROR maps its IDs to the following other identifiers: GRID, Wikidata, ISNI, and Crossref Funder Registry (formerly “Fundref”).

There are additional IDs in the existing ROR metadata that are primarily legacy identifiers. These will not be actively curated in new records for the time being: OrgRef, UCAS, CNRS, HESA, UKPRN.

# Example record

See the JSON structure in an example v1 organization record.

```json
{
   "acronyms" : [
      "DSZ"
   ],
   "addresses" : [
      {
         "city" : "Essen",
         "country_geonames_id" : null,
         "geonames_city" : {
            "city" : "Essen",
            "geonames_admin1" : {
               "ascii_name" : null,
               "code" : "DE.NW",
               "id" : null,
               "name" : "North Rhine-Westphalia"
            },
            "geonames_admin2" : {
               "ascii_name" : null,
               "code" : null,
               "id" : null,
               "name" : null
            },
            "id" : 2928810,
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
         "lat" : 51.45657,
         "line" : null,
         "lng" : 7.01228,
         "postcode" : null,
         "primary" : false,
         "state" : null,
         "state_code" : null
      }
   ],
   "aliases" : [],
   "country" : {
      "country_code" : "DE",
      "country_name" : "Germany"
   },
   "email_address" : null,
   "established" : null,
   "external_ids" : {
      "FundRef" : {
         "all" : [
            "501100008427"
         ],
         "preferred" : null
      },
      "GRID" : {
         "all" : "grid.479785.0",
         "preferred" : "grid.479785.0"
      }
   },
   "id" : "https://ror.org/05wgann87",
   "ip_addresses" : [],
   "labels" : [],
   "links" : [
      "https://www.deutsches-stiftungszentrum.de/"
   ],
   "name" : "Deutsches Stiftungszentrum",
   "relationships" : [
      {
         "id" : "https://ror.org/01f7ent21",
         "label" : "Hermann und Lilly Schilling-Stiftung",
         "type" : "Child"
      }
   ],
   "status" : "active",
   "types" : [
      "Nonprofit",
      "Funder"
   ],
   "wikipedia_url" : null
}
```
