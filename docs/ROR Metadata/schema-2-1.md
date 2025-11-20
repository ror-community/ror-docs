---
title: Schema 2.1
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    Schema v2.1 of the ROR data structure, finalized in November 2024, adds
    fields for continent and country subdivisions to address user feedback, with
    these changes incorporated into the v2 API without altering the API URL.
    Additionally, the `Funder` type has been backported to schema 1.0 to ensure
    compatibility with v1 records.
  robots: index
next:
  description: ''
---
> ❗️ Version 1 of the ROR schema and API will be sunset in December 2025
>
> In December 2025, version 1 of the ROR schema and API will be sunset, meaning that ROR API requests with v1 in the path will no longer return a response, v1 files will no longer be included in the ROR data dump, and v1 documentation will no longer be available. Read more in our [changelog](https://ror.readme.io/changelog/2025-07-01-sunset-of-version-1#/).

> 📘 JSON Schema
>
> JSON schema documents used to generate and validate ROR records are available at [https://github.com/ror-community/ror-schema](https://github.com/ror-community/ror-schema).

# Community feedback

The current ROR data structure was revised in April of 2024 and formalized into [Schema 2.0](doc:schema-v2) as a JSON schema document. In schema v2.0, significant changes were made to fields that contained geographic information, including removing fields related to administrative subdivisions corresponding to units such as Canadian provinces, Japanese prefectures, and US states.

While no issues were raised with these changes during the v2.0 feedback process, since the launch of this schema version, the need for additional location details to be included in our records was identified by users. As a result, a [proposal for schema v2.1](https://docs.google.com/document/d/11-bDfQWK038uoUBkL_CBZxP__xB6nBy0dVPH7tWxNnY), which adds country subdivision and continent fields, was circulated for public comment and finalized in Nov 2024.

This is a non-breaking change, and schema v2.1 changes have been incorporated directly into the v2 API with **no version change needed in the API URL**, per ROR's [schema and API versioning policy](https://ror.readme.io/v2/docs/api-versions).

# Schema changes

In schema v2.1, the following fields have been added to the `geonames_details` sub-field within the `locations` field:

* `continent_code` (required) 2-character code for the continent that the organization is located in, from the GeoNames record for the GeoNames ID in locations.geonames_id. All records have a value in the field.

* `continent_name` (required) Name of the continent that the organization is located in, from the GeoNames record for the GeoNames ID in locations.geonames_id. All records have a value in the field.

* `country_subdivision_code` 2 or 3-character code for the highest-level country subdivision that the organization is located in, from the admin1Codes field of the GeoNames record for the GeoNames ID in locations.geonames_id. These are derived from ISO-3166-2.

* `country_subdivision_name` Name of the highest-level country subdivision that the organization is located in, from the admin1Codes field of the GeoNames record for the GeoNames ID in locations.geonames_id. These are derived from ISO-3166-2.

**Continents:** GeoNames uses a 7-continent name and code convention, so continent name and code values are as follows: Africa (AF), Antarctica (AN), Asia (AS), Europe (EU), North America (NA), Oceania (OC), and South America (SA).

**Country subdivisions:** The type of subdivision represented by the fields `country_subdivision_code` and `country_subdivision_name` varies depending on the location identified by the GeoNames ID in the field `locations.geonames_id`. For the United States, for example, it represents states. Some organizations are not located within a country subdivision (ex, research stations in Antarctica), so not all records have values in these fields. See [ISO 3166-2](https://en.wikipedia.org/wiki/ISO_3166-2)  for a list of country subdivisions by country.

Also, in schema 1.0, `Funder` has been added as an allowed value in the `types` field. This is because `funder` was initially added as a type in v2 only, and when new records are created in v2 that only have a type of `funder,` the corresponding v1 record has no values in `types`. NOTE: Most v2 records with `funder` in `types` have multiple values in `types`, so only a very small number of records had no `types` in v1.

New fields listed above are included in the JSON and CSV v2 [Data dump](doc:data-dump) files. No changes have been made to data dump filenames. No changes have been made to the ROR [Web search](doc:web-search).

# API changes

## v2 API

Requests to the v2 API at `https://api.ror.org/v2/organizations` now return new fields, values, and facets.

The above new `locations` fields now appear in each record in when using the v2 API. A `continents` facet now appears in the meta section of list results in the v2 API.

New [filters](doc:api-filtering) for continent name and code are now available in the v2 API:

`https://api.ror.org/v2/organizations?filter=locations.geonames_details.continent_name:Asia`

`https://api.ror.org/v2/organizations?filter=locations.geonames_details.continent_code:AS`

Data in `locations` fields in all records has been updated using the latest values from GeoNames.

## v1 API

Requests to the v1 API at `https://api.ror.org/v1/organizations` now return records with the `Funder` value in the `types` field when the corresponding v2 record also includes `funder` in the `types` field.

`Funder` is now an allowed value for the `types` [filter](doc:api-filtering) on v1 API requests:

`https://api.ror.org/v1/organizations?filter=types:Funder`

Data in `addresses` fields in all records was updated using the latest values from GeoNames.

# Null values in v1 records

Beginning with the April 2024 launch of v2, ROR metadata is curated only in v2 and then crosswalked / copied to v1 records. As a result, when records are added or updated, v1 fields that do not have equivalents in v2 are not populated. This includes many `addresses` sub-fields in v1 records. All records have been updated in v2.1 ([release 1.58](https://github.com/ror-community/ror-updates/releases/tag/v1.58)), and therefore after December 2024 all deprecated sub-fields in `addresses` are null in all v1 records in the API and data dump.
