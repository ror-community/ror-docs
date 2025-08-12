---
title: ROR schema & API v2 (beta)
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: >-
    ROR is introducing a beta version of their v2 schema and API update,
    available for testing until October 31, 2023. Users are encouraged to
    provide feedback on the functionality and any bugs encountered during
    testing.
  robots: index
next:
  description: ''
---
# Introducing v2

After nearly a year of planning and community input, we are thrilled to release a beta version of ROR's first major schema and API update (version 2.0). The v2 beta is available in the **dev API environment** with base URL `https://api.dev.ror.org/v2/organizations` and is open to the public for testing through ~~16 Oct 2023~~ **31 October 2023**. Before trying it out, please review the information about the v2 schema and API beta below. For instructions on providing feedback, see [How can I provide feedback on the v2 beta?](#how-can-i-provide-feedback-on-the-v2-beta) below. 

> ❗️ v2 beta available for API and data dump only
> 
> **Important note!** the v2 beta is available for the API and data dump only. v2 is not yet available for testing in the ROR search interface or ROR reconciler.

## About ROR schema & API versioning

In 2022, ROR [gathered community feedback](/docs/feedback-docs#handling-schema-and-api-versioning-in-ror) and adopted a versioning policy that allows making changes to the schema and API in a way that provides API users with ample time and notice to update to the new schema/API version. Key highlights of this policy include:

- Breaking changes to either the schema or API (ex: change to the structure of a field) will be implemented using a new major API version. Minor (non-breaking) changes (ex: a new value allowed for a field whose values comes from a controlled list)  will be implemented within the current major API version.
- The API version will be supplied in the path portion of an API request, e.g. `https://api.dev.ror.org/v2/organizations`
- Requests that do not include a version in the path portion will be redirected to a default version
- 2 major API versions will be supported concurrently (the current major version and the most recent previous version).
- At least 1 year notice will be provided before sunsetting a major version, via typical ROR communication channels such as our email list, Tech Support Forum, support site, Slack and community meetings.

[Read more about the ROR schema versioning policy](doc:schema-versions).

> 📘 v2 production release & v1 sunset schedule
> 
> Per this policy, we expect to make API v2 available in production in early 2024. v1 (the current version) will remain the default version until at least early 2025.

## Schema v2

During early 2023, ROR gathered 3 rounds of community feedback on a new schema version (v2) to address ongoing issues and community use cases that couldn't be supported in the original ROR schema (which was inherited from GRID). Key documents from that process are:

- [ROR Schema v2.0 draft proposal](https://docs.google.com/document/d/1JNDMoKmjR2y0quWXwFfoJTsIttbltJVN0l5Wddw1cIk/edit?usp=sharing) - proposal open for comment through February 2023
- [ROR Schema v2.0 second draft proposal](https://docs.google.com/document/d/18Qg6-lv2Fxkc97SLpD8gdS0V8p0y9fdaZEywyeyKJWM/edit?usp=sharing) - proposal open for comment through April 2023
- [ROR Schema v2.0 final proposal](https://docs.google.com/document/d/1lYybpmtFW3cSitNAUzuVgFieco17PfuIbeVlcqcwync/edit?usp=sharing) - proposal adopted April 2023

### Key changes in schema v2

- **Name information** previously in `name`, `acronyms`, `aliases`, and `labels` fields is now contained in 1 parent field, `names` with subfields `lang`, `value` and `types`. **Please note that the `lang` subfield has only been populated for names with `labels` in their `types`. ** The curation team will be working on adding language codes to other names types over the coming months.
- **Location information** previously in `addresses` field is now in `locations` field with subfields `geonames_id` and `geoneames_details`. Many fields containing very granular information derived from Geonames have been removed, as this information is avilable directly from [Geonames](https://www.geonames.org/). Additionally, country code and name information previously in the `country` field has been moved to `locations.geonames_details.country_code` and `locations.geonames_details.country_name`
- **Website/domain information** previously in `links` and `wikipedia_url` have been combined into a 1 parent field `links` with subfields `type` and `value`. The `ip_addresses` field has been removed (it was not populated by GRID for any records). The `domains` field has been added, however, **please note that this field has not yet been populated**. The curation team will be working on this over the coming months. 
- **External identifiers information** has been restructured within the existing `external_ids` field. Each item in external_ids now has subfields `type`, `all` and `preferred`. The data type for `all` is a list for each `external_ids` item, whereas it was previously a string for GRID IDs and a list for other ID types.
- **Administrative information** was not included previously. A new parent field `admin` has been added, which contains subfields `created` and `last_modified`. Each of those subfields contains additional subfields `date` and `schema_version`. Created date for each record was extracted from previous GRID _and_ ROR releases. Last modified dates were extracted from ROR releases only, as, at a minimum, each record in ROR has been modified by the ROR curation team to add a ROR ID in the `id` field.
- **Controlled lists** previously had variations in casing. For example, values in the `types` and `relationships.type` fields began with an uppercase character, while values in `status` were lowercase and external ID types contained a variety of casings. In v2, allowed values in controlled lists are consistently lowercase, with the exception of country codes derived from [ISO-3166](https://www.iso.org/iso-3166-country-codes.html), which are uppercase per the standard.

For details and examples of each field, see the [Schema 2.0 doc](doc:schema-v2). You can also view the [v2 JSON schema doc in GitHub](https://github.com/ror-community/ror-schema/blob/schema-v2/ror_schema_v2_0.json). For comparison, the [current (v1) JSON schema doc](https://github.com/ror-community/ror-schema/blob/master/ror_schema.json) is also available in GitHub. Note that there is additional validation applied to each record beyond the rules specified in the JSON Schema doc, because some rules cannot be expressed in JSON schema (ex: types of relationships allowed between active and inactive records).

### Example v2 record

```Text json
{
  "admin": {
    "created": {
      "date": "2018-11-14",
      "schema_version": "1.0"
    },
    "last_modified": {
      "date": "2023-08-17",
      "schema_version": "2.0"
    }
  },
  "domains": [],
  "established": 1868,
  "external_ids": [
    {
      "all": [
        "0000 0001 2348 0690"
      ],
      "preferred": null,
      "type": "isni"
    },
    {
      "all": [
        "100005595",
        "100009350",
        "100004802",
        "100010574",
        "100005188",
        "100005192"
      ],
      "preferred": "100005595",
      "type": "fundref"
    },
    {
      "all": [
        "Q184478"
      ],
      "preferred": null,
      "type": "wikidata"
    },
    {
      "all": [
        "grid.30389.31"
      ],
      "preferred": "grid.30389.31",
      "type": "grid"
    }
  ],
  "id": "https://ror.org/00pjdza24",
  "links": [
    {
      "type": "website",
      "value": "http://www.universityofcalifornia.edu/"
    }
  ],
  "locations": [
    {
      "geonames_details": {
        "country_code": "US",
        "country_name": "United States",
        "lat": 37.80437,
        "lng": -122.2708,
        "name": "Oakland"
      },
      "geonames_id": 5378538
    }
  ],
  "names": [
    {
      "lang": null,
      "types": [
        "ror_display",
        "label"
      ],
      "value": "University of California System"
    },
    {
      "lang": "es",
      "types": [
        "label"
      ],
      "value": "Universidad de California"
    },
    {
      "lang": "fr",
      "types": [
        "label"
      ],
      "value": "Université de Californie"
    },
    {
      "lang": null,
      "types": [
        "acronym"
      ],
      "value": "UC"
    }
  ],
  "relationships": [
    {
      "label": "Lawrence Berkeley National Laboratory",
      "type": "related",
      "id": "https://ror.org/02jbv0t02"
    },
    {
      "label": "California Digital Library",
      "type": "child",
      "id": "https://ror.org/03yrm5c26"
    },
    {
      "label": "Center for Information Technology Research in the Interest of Society",
      "type": "child",
      "id": "https://ror.org/00zv0wd17"
    },
    {
      "label": "University of California Division of Agriculture and Natural Resources",
      "type": "child",
      "id": "https://ror.org/03t0t6y08"
    },
    {
      "label": "University of California, Berkeley",
      "type": "child",
      "id": "https://ror.org/01an7q238"
    },
    {
      "label": "University of California, Davis",
      "type": "child",
      "id": "https://ror.org/05rrcem69"
    },
    {
      "label": "University of California, Irvine",
      "type": "child",
      "id": "https://ror.org/04gyf1771"
    },
    {
      "label": "University of California, Los Angeles",
      "type": "child",
      "id": "https://ror.org/046rm7j60"
    },
    {
      "label": "University of California, Merced",
      "type": "child",
      "id": "https://ror.org/00d9ah105"
    },
    {
      "label": "University of California, Riverside",
      "type": "child",
      "id": "https://ror.org/03nawhv43"
    },
    {
      "label": "University of California, San Diego",
      "type": "child",
      "id": "https://ror.org/0168r3w48"
    },
    {
      "label": "University of California, San Francisco",
      "type": "child",
      "id": "https://ror.org/043mz5j54"
    },
    {
      "label": "University of California, Santa Barbara",
      "type": "child",
      "id": "https://ror.org/02t274463"
    },
    {
      "label": "University of California, Santa Cruz",
      "type": "child",
      "id": "https://ror.org/03s65by71"
    },
    {
      "label": "University of California Natural Reserve System",
      "type": "child",
      "id": "https://ror.org/04nmjep87"
    },
    {
      "label": "University of California Office of the President",
      "type": "child",
      "id": "https://ror.org/00dmfq477"
    }
  ],
  "status": "active",
  "types": [
    "education"
  ]
}
```

### Full list of schema v2 fields and subfields

| Field name                              | Type               | Description                                                                                                                                                                                                                                                                                                                                                                                                                      | Allowed values                                                                                           |
| :-------------------------------------- | :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------- |
| admin                                   | Object             | Container for administrative information about the record                                                                                                                                                                                                                                                                                                                                                                        |                                                                                                          |
| admin.created                           | Object             | Container for administrative information about the creation of the record                                                                                                                                                                                                                                                                                                                                                        |                                                                                                          |
| admin.created.date                      | String             | Date the record was added to ROR                                                                                                                                                                                                                                                                                                                                                                                                 | Date formatted as YYYY-MM-DD                                                                             |
| admin.created.schema_version            | String             | ROR schema version that the record was initially created in                                                                                                                                                                                                                                                                                                                                                                      | 1.0, 2.0                                                                                                 |
| admin.last_modified                     | Object             | Container for administrative information about the last modification to the record                                                                                                                                                                                                                                                                                                                                               |                                                                                                          |
| admin.last_modified.date                | String             | Date the record was last modified in ROR                                                                                                                                                                                                                                                                                                                                                                                         | Date formatted as YYYY-MM-DD                                                                             |
| admin.last_modified.schema_version      | String             | ROR schema version that the record was last modified in                                                                                                                                                                                                                                                                                                                                                                          | 1.0, 2.0                                                                                                 |
| domains                                 | Array (of strings) | Fully-qualified domains that belong to the organization, using the smallest number of labels needed encompass the organization (excluding www). Each domain must be unique within ROR; a given domain cannot be listed in multiple ROR records. Multiple values are allowed, however, values cannot be subdomains of other domains  listed in the same ROR record.                                                               |                                                                                                          |
| established                             | Number             | Year the organization was established (CE)                                                                                                                                                                                                                                                                                                                                                                                       | Date as YYYY                                                                                             |
| external_ids                            | Object             | Container for information about identifiers in other systems ("external identifiers") that are associated with a given organization in ROR                                                                                                                                                                                                                                                                                       |                                                                                                          |
| external_ids.all                        | Array (of strings) | All external identifiers of the type specified in external_ids.type                                                                                                                                                                                                                                                                                                                                                              |                                                                                                          |
| external_ids.preferred                  | String             | Preferred external identifier for the organization of the type specified in external_ids.type                                                                                                                                                                                                                                                                                                                                    |                                                                                                          |
| external_ids.type                       | String             | Identifier system that the identifiers in external_ids.all and external_ids.preferred belong to. Supported systems are [Crossref Open Funder Registry](https://www.crossref.org/services/funder-registry) (formerly FundRef), [GRID](https://grid.ac/) (deprecated, but currently supported in ROR for records included in ROR seed data supplied by GRID), [ISNI](https://isni.org/) and [Wikidata](https://www.wikidata.org/). | fundref, grid, isni, wikidata                                                                            |
| id                                      | String             | Unique ROR ID for the organization                                                                                                                                                                                                                                                                                                                                                                                               |                                                                                                          |
| links                                   | Object             | Container for information about URLs related to the organization                                                                                                                                                                                                                                                                                                                                                                 |                                                                                                          |
| links.type                              | String             | Type of link listed in links.value                                                                                                                                                                                                                                                                                                                                                                                               | website, wikipedia                                                                                       |
| links.value                             | String             | URL of a link related to the organization                                                                                                                                                                                                                                                                                                                                                                                        | Valid URI, according to [IETF RFC 3986](https://datatracker.ietf.org/doc/html/rfc3986)                   |
| locations                               | Object             | Container for location information                                                                                                                                                                                                                                                                                                                                                                                               |                                                                                                          |
| locations.geonames_details              | Object             | Container for details derived from the [Geonames](https://www.geonames.org/) record for the Geonames ID in locations.geonames_id                                                                                                                                                                                                                                                                                                 |                                                                                                          |
| locations.geonames_details.country_code | String             | ISO 3166-2 code for the country that the organization is located in, from the [Geonames](https://www.geonames.org/) record for the Geonames ID in locations.geonames_id                                                                                                                                                                                                                                                          | Valid 2-character [ISO 3166](https://www.iso.org/iso-3166-country-codes.html)-2 country code (uppercase) |
| locations.geonames_details.lat          | Number             | Latitude of the location identified in locations.geonames_id, from the [Geonames](https://www.geonames.org/) record for that Geonames ID                                                                                                                                                                                                                                                                                         |                                                                                                          |
| locations.geonames_details.lng          | Number             | Longitude of the location identified in locations.geonames_id, from the [Geonames](https://www.geonames.org/) record for that Geonames ID                                                                                                                                                                                                                                                                                        |                                                                                                          |
| locations.geonames.name                 | String             | Name of the location identified in locations.geonames_id, from the [Geonames](https://www.geonames.org/) record for that Geonames ID.                                                                                                                                                                                                                                                                                            |                                                                                                          |
| locations.geonames_id                   | Integer            | [Geonames](https://www.geonames.org/) ID for the city or most granular administrative region that the organization is located in. For most records, this ID represents a city, but for organizations not located in a city, the value in this field is ID of the most granular administrative region for the location available in Geonames.                                                                                     | Valid [Geonames](https://www.geonames.org/) ID                                                           |
| names                                   | Object             | Container for name information                                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                          |
| names.lang                              | String             | ISO 639-1 language code that identifies the language of a value in names.value. May be used with any name type(s).                                                                                                                                                                                                                                                                                                               | Valid 2-character [ISO 639](https://www.iso.org/iso-639-language-codes.html)-1 language code (lowercase) |
| names.types                             | Array (of strings) | The type(s) associated with the name contained in names.value. Each name must have at least 1 type, and exactly 1 name must have ror_display in its types. Each name can have multiple types, for example ror_display and label.                                                                                                                                                                                                 | acronym, alias, label, ror_display                                                                       |
| names.value                             | String             | Name that the organization is (or was) known by, which may be a current official name, former name, alias, acronym, etc.                                                                                                                                                                                                                                                                                                         |                                                                                                          |
| relationships                           | Object             | Container for relationship information                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                          |
| relationships.id                        | String             | Unique ROR ID of another organization which is related to the organization                                                                                                                                                                                                                                                                                                                                                       |                                                                                                          |
| relationships.label                     | String             | Name of another organization identified in relationships.id, which is related to the organization                                                                                                                                                                                                                                                                                                                                |                                                                                                          |
| relationships.type                      | String             | Type of relationship between the organization and another organization identified in relationships.id                                                                                                                                                                                                                                                                                                                            | child, parent, related, successor, predecessor                                                           |
| status                                  | String             | Whether the organization is active or not                                                                                                                                                                                                                                                                                                                                                                                        | active, inactive, withdrawn                                                                              |
| types                                   | Array (of strings) | Organization type(s). Allowed types: Education, Healthcare, Company, Archive, Nonprofit, Government, Facility, Funder, Other                                                                                                                                                                                                                                                                                                     | archive, company, education, facility, funder, government, healthcare, other                             |

## API v2

The v2 REST API includes all the same search and retrieval functionality as the current (v1) REST API, but with ROR records in responses formatted according to the v2 schema.

> ❗️ v2 API currently available in dev environment only
> 
> During this beta period, the v2 API is only available in the ROR dev environment at <https://api.dev.ror.org/v2/organizations>.

### Specifying the API version

For all ROR API requests, the API version is specified in the path portion of the request, ex 

```curl
curl https://api.dev.ror.org/v2/organizations
```

Version options available during this beta test are:

- **v1** `https://api.dev.ror.org/v1/organizations` Returns a response formatted using the original ROR data model (which was not versioned when introduced, but it being retroactively versioned as v1.0)
- **v2** `https://api.dev.ror.org/v2/organizations` Returns a response formatted using the v2.0 ROR data model
- **No version**`https://api.dev.ror.org/organizations`Returns a response formatted using the default version, which is currently v1. Same response as v1 `https://api.dev.ror.org/v1/organizations`. After the production launch of v2 (planned for Jan 2024), the default version will remain v1 until approximately Jan 2025, when the default version will be changed to v2.

### Example API requests

#### Retrieve a single ROR record

As in the current production API, the ROR ID included in this request can be formatted like `00tjv0s33`, `ror.org/00tjv0s33` or `https://ror.org/00tjv0s33`.

```curl
curl 'https://api.dev.ror.org/v2/organizations/00tjv0s33'
```

#### Retrieve list of ROR records

```curl
curl 'https://api.dev.ror.org/v2/organizations'
```

#### Query parameter search

```curl
curl 'https://api.dev.ror.org/v2/organizations?query=Bath'
```

#### Advanced query parameter search

The biggest difference in API v1 vs v2 is the fields available to search using `?query.advanced`, because some field names and structures have been added, changed or removed. See the [full list of schema v2 fields and subfields](#full-list-of-schema-v2-fields-and-subfields) below that you can use in advanced queries.

Search a single field

```curl
curl 'https://api.dev.ror.org/v2/organizations?query.advanced=names.value:%22Harvard%20University%22'
```

Search multiple fields

```curl
curl 'https://api.dev.ror.org/v2/organizations?query.advanced=names.value:Cornell+AND+locations.geonames_details.name:Ithaca'
```

Search by last modified date for records modified between 2 dates (using a [range query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_ranges)). Note that unescaped reserved characters such as `[, ], {,` and `}` must be used in range queries. This is the use case they are reserved for! Escaped brackets and braces will be processed as string literals and will not produce the expected results.

```curl
curl 'https://api.dev.ror.org/v2/organizations?query.advanced=admin.last_modified.date:[2023-07-01%20TO%202023-09-05]'
```

Search by created date for active records created before a specific date (using a [range query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_ranges)).

```curl cur
curl 'https://api.dev.ror.org/v2/organizations?query.advanced=admin.created.date:{*%20TO%202019-12-31}'
```

#### Affiliation parameter search

```curl
curl 'https://api.ror.org/v2/organizations?affiliation=UCL%20School%20of%20Slavonic%20and%20East%20European%20Studies'
```

### Paging & filtering

As in the current API, results for `/organizations`, `?query` and `query.advanced` requests with more than 20 results are paginated, with the first 20 results returned by default. A maximum of 10,000 records can be returned with any given request. `?affiliation` requests are not paginated. For more information, see [Paging](doc:api-paging).

As in the current API, results for `/organizations`, `?query` and `query.advanced` can be filtered by status, type, country code and country name. `?affiliation` requests cannot be filtered. For more information, see [Filtering](doc:api-filtering). When filtering by country name/code in v2, the following filter names are equivalent and can be used interchangeably:

- `country.country_code` and `locations.geonames_details.country_code`
- `country.country_name` and `locations.geonames_details.country_name`

### Formatting search strings (special characters, wildcards, etc)

As in the current API, all search strings used `?query`, `?query.advanced`and`?affiliation` requests must be [URL-encoded](https://www.w3schools.com/tags/ref_urlencode.asp) and [Elasticsearch reserved characters](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_reserved_characters) must be escaped. [Elasticsearch query string syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax) is supported for `?query` and `?query.advanced` requests, but not for `?affiliation` requests. For more information, see [Formatting searches](https://ror.readme.io/docs/query-parameter#formatting-searches).

### Record status

As in the current API, all requests except requests for a specific ROR record (by its ID) return only records with a status of active by default. Records with a status of inactive or withdrawn can be included in results by adding an additional parameter `all_status` to your request. This parameter can be used in combination with the `?query`, `?query.advanced` or `?affiliation` parameter. For more information, see [Retrieve a list of records with all statuses](https://ror.readme.io/docs/retrieve-a-list-of-ror-records#retrieve-a-list-of-records-with-all-statuses).

## Data dump v2

Sample data dumps containing both v1 and v2 files are available on GitHub at [\<https://github.com/ror-community/ror-data-test>](https://github.com/ror-community/ror-data-test). Updated v2 samples are added with every new [ROR release](https://github.com/ror-community/ror-updates/releases).

> 🚧 The files in <https://github.com/ror-community/ror-data-test> are for testing only and are not official release files.

The sample zip files provided each contain 4 files: 2 files with data using schema v2 (JSON and CSV) and 2 files using schema v1, as in the examples below: 

- v1.33-2023-09-21-ror-data_schema_v2.json (new file not in production data dumps - JSON data dump with records in v2 schema)
- v1.33-2023-09-21-ror-data_schema_v2.csv (new file not in production data dumps - CSV data dump containing a subset of v2 fields)
- v1.33-2023-09-21-ror-data.json (JSON data dump with records in v1 schema)
- v1.33-2023-09-21-ror-data.csv (CSV data dump containing a subset of v1 fields)

> 📘 Data dump filenames
> 
> For individual files inside the vX.XX-YYY-MM-DD-ror-data.zip data dump file:
> 
> - We will leave the v1 file names as is during the period when v1 is the default version (approx Jan 2024-Dec 2025), in order to allow data dump users time to update their code
> - Once v2 becomes the default version, v1 data dumps will continue to be produced, however, `_schema_v1` will be added to the end of v1 JSON and CSV data dump filenames.

# Testing guidelines

As a community-driven initiative, ROR relies on input from its stakeholders to ensure that new and updated features serve the needs of its wide array of users and use cases. We invite the ROR community to test and provide feedback on schema and API v2 through ~~16 Oct 2023~~ **31 October 2023**. During this beta period, the v2 API is only available in the ROR dev environment at <https://api.dev.ror.org/v2/organizations>. 

## Who should participate in the ROR schema & API v2 beta?

This beta is open to the public, and all feedback is welcome. **We're particularly seeking feedback from current and prospective ROR API and data dump users, especially those with a technical focus who work directly with ROR data through software development, product design/specification, support, etc.**

## What kind of feedback is ROR looking for in this beta?

This beta is focused on functionality of the v2 schema and API (rather than the data contained in the records - see note on v2 data below). Examples of feedback we're most interested in are:

- Bugs specifically related to v2, ex: a request that works as expected in the current API, but does not work as expected when repeating the same request in the v2 API.
- Use case issues related specifically to schema/API v2, ex: a ROR API or data dump feature/functionality that you use right now is not available in v2, or an aspect of the v2 API or data dump will present major implementation challenges for your use case.
- Other comments/feedback specifically related to the v2 schema, API and/or data dump

## What kind of feedback is not in scope for this beta?

- Major changes to [schema v2](https://ror.readme.io/docs/schema-v20). During early 2023, we conducted [3 rounds of public comment on v2 of the schema](https://ror.readme.io/docs/community-feedback-documents#ror-schema-v20), and through that process, many current/prospective ROR users helped to shape the resulting v2 schema. To request a change for consideration in a future schema version, please [submit a schema change request](https://github.com/ror-community/ror-roadmap/issues/new?assignees=&labels=data+model%2Fschema&projects=&template=schema-change-request.md&title=%5BSCHEMA%5D).
- Feedback related to the ROR search interface, which is not included in this beta.
- General bug reports or feature requests that are not related specifically to v2. Please submit general [bug reports](https://github.com/ror-community/ror-roadmap/issues/new?assignees=&labels=bug&projects=&template=bug_report.md&title=%5BBUG%5D+) and [feature requests](https://github.com/ror-community/ror-roadmap/issues/new?assignees=&labels=feature&projects=&template=feature_request.md&title=%5BFEATURE%5D) separately. 
- Requests for updates/additions to records in the ROR registry. To request an update or addition to ROR,  please use the [curation request form](https://curation-request.ror.org/).

## Important notes about v2 record data

There are several new fields/subfields in v2, and the dataset used in the beta has not been fully updated with values in all new fields/subfields. In particular:

- **Created/last modified dates** _HAVE_ been added to all records, using actual dates from GRID and ROR data releases.
- **Domains** _HAVE NOT_ been added. This field is currently an empty list for all records. This field requires careful curation to ensure accuracy. We plan to add data to this field over the coming months.
- **Language codes** for items in the names fields are only included for names inherited from the labels field in the current schema. Language codes _HAVE NOT_ been added for names inherited from the name and aliases fields in the current schema. We plan to add language codes over the coming months, with the goal of ensuring that (minimally) each name with “ror_display” in its types has a language code.

## How can I provide feedback on the v2 beta?

1. Check the [ROR schema & API v2 beta project board](https://github.com/orgs/ror-community/projects/12/views/1) to see if the same or a closely related issue has already been reported. If so, please add comments to that issue.
2. If you have new feedback that’s not related to another issue on the ROR schema & API v2 beta project board, submit a Github issue using the [ROR schema & API v2 beta feedback template](https://github.com/ror-community/ror-roadmap/issues/new?assignees=lizkrznarich&labels=api+v2+beta&projects=&template=ror-schema---api-v2-beta-feedback.md&title=%5BV2+BETA%5D).

All issues will be reviewed by ROR staff, and they may contact you using your Github handle to request additional information. If you are not a Github user or prefer to submit your feedback privately, please email [support@ror.org](mailto:support@ror.org) . **Please provide your feedback by ~~16 Oct 2023~~ **31 October 2023.\*\*

# Testing tips

Not sure where to start? We recommend testing v2 based on the ROR feature(s)/functionality you're using now. If you're not sure which features are available in the the current production environment, see the [ROR REST API](doc:rest-api) and [data dump](https://ror.readme.io/docs/data-dump) documentation.

## API users

- Determine which request(s) your applications/services are currently making to the ROR API. Test those requests using the v2 dev API, with base URL [\<https://api.dev.ror.org/v2/organizations>](https://api.dev.ror.org/v2/organizations) .
- Review the v2 result(s) and the response format to v1. _Note that since v2 uses a separate Elasticsearch index from v1, and since v2 records contain different data from v1, there may be small variations in the number and order of results returned._
- Submit feedback about any bugs encountered and/or any aspects of the v2 API/schema that will present significant implementation challenges for you/your use cases, per [How can I provide feedback on the v2 beta?](https://ror.readme.io/docs/ror-schema-api-v2-beta#how-can-i-provide-feedback-on-the-v2-beta)

## Data dump users

- Download and review a sample v2 data dump from <https://github.com/ror-community/ror-data-test>. 
- Check whether you are able to extract and work with records in the v2 files as needed for your ROR use case.
- If you use both the JSON data and the CSV data in the data dump, be sure to test whether you are able to work with both files.
- Submit feedback about any bugs encountered and/or any aspects of the v2 data dump that will present significant implementation challenges for you/your use case, per [How can I provide feedback on the v2 beta?](https://ror.readme.io/docs/ror-schema-api-v2-beta#how-can-i-provide-feedback-on-the-v2-beta)

We are grateful for your help! Following the close of the beta test on ~~October 16, 2023~~ **October 31, 2023**, we will review all reports and work to incorporate the improvements before we release v2 into production.