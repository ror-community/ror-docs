---
title: Fields and sub-fields
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR fields and subfields
  description: >-
    Reference page for all the fields and subfields in version 1 of the ROR
    metadata schema.
  keywords:
    - fields
    - ' subfields'
    - ' schema'
  robots: index
next:
  description: ''
---
<Callout icon="👍" theme="okay">
  ## Recommended schema

  This page documents the current recommended, stable schema, [Schema 2.1](doc:schema-v2-1), which is used by the ROR [REST API](doc:rest-api), the ROR [Web search](doc:web-search), and in the ROR [Data dump](doc:data-dump).

  JSON schema documents used to generate and validate ROR records are available at [https://github.com/ror-community/ror-schema](https://github.com/ror-community/ror-schema).
</Callout>

The following list includes all available fields and sub-fields in version 2.1 of the ROR metadata schema. For a list of top-level fields only, see [ROR data structure](doc:ror-data-structure). If you'd like to request a change to the ROR metadata schema, please [submit a schema change request on our roadmap](https://github.com/ror-community/ror-roadmap/issues/new/choose).

# All v2.1 fields and sub-fields in alphabetical order

| Field name                                          | Type               | Description                                                                                                                                                                                                                                                                                                                                                                                                                      | Allowed values                                                                                           |
| :-------------------------------------------------- | :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------- |
| admin                                               | Object             | Container for administrative information about the record                                                                                                                                                                                                                                                                                                                                                                        |                                                                                                          |
| admin.created                                       | Object             | Container for administrative information about the creation of the record                                                                                                                                                                                                                                                                                                                                                        |                                                                                                          |
| admin.created.date                                  | String             | Date the record was added to ROR                                                                                                                                                                                                                                                                                                                                                                                                 | Date formatted as YYYY-MM-DD                                                                             |
| admin.created.schema_version                        | String             | ROR schema version in effect when the record was created                                                                                                                                                                                                                                                                                                                                                                         | 1.0, 2.0, 2.1                                                                                            |
| admin.last_modified                                 | Object             | Container for administrative information about the last modification to the record                                                                                                                                                                                                                                                                                                                                               |                                                                                                          |
| admin.last_modified.date                            | String             | Date the information in the record was last modified                                                                                                                                                                                                                                                                                                                                                                             | Date formatted as YYYY-MM-DD                                                                             |
| admin.last_modified.schema_version                  | String             | ROR schema version in effect when the information in the record was last modified                                                                                                                                                                                                                                                                                                                                                | 1.0, 2.0, 2.1                                                                                            |
| domains                                             | Array (of strings) | Fully-qualified domains that belong to the organization, using the smallest number of labels needed encompass the organization (excluding www). Each domain must be unique within ROR; a given domain cannot be listed in multiple ROR records. Multiple values are allowed, however, values cannot be subdomains of other domains  listed in the same ROR record.                                                               |                                                                                                          |
| established                                         | Number             | Year the organization was established (CE)                                                                                                                                                                                                                                                                                                                                                                                       | Date as YYYY                                                                                             |
| external_ids                                        | Object             | Container for information about identifiers in other systems ("external identifiers") that are associated with a given organization in ROR                                                                                                                                                                                                                                                                                       |                                                                                                          |
| external_ids.all                                    | Array (of strings) | All external identifiers of the type specified in external_ids.type                                                                                                                                                                                                                                                                                                                                                              |                                                                                                          |
| external_ids.preferred                              | String             | Preferred external identifier for the organization of the type specified in external_ids.type                                                                                                                                                                                                                                                                                                                                    |                                                                                                          |
| external_ids.type                                   | String             | Identifier system that the identifiers in external_ids.all and external_ids.preferred belong to. Supported systems are [Crossref Open Funder Registry](https://www.crossref.org/services/funder-registry) (formerly FundRef), [GRID](https://grid.ac/) (deprecated, but currently supported in ROR for records included in ROR seed data supplied by GRID), [ISNI](https://isni.org/) and [Wikidata](https://www.wikidata.org/). | fundref, grid, isni, wikidata                                                                            |
| id                                                  | String             | Unique ROR ID for the organization                                                                                                                                                                                                                                                                                                                                                                                               |                                                                                                          |
| links                                               | Object             | Container for information about URLs related to the organization                                                                                                                                                                                                                                                                                                                                                                 |                                                                                                          |
| links.type                                          | String             | Type of link listed in links.value                                                                                                                                                                                                                                                                                                                                                                                               | website, wikipedia                                                                                       |
| links.value                                         | String             | URL of a link related to the organization                                                                                                                                                                                                                                                                                                                                                                                        | Valid URI, according to [IETF RFC 3986](https://datatracker.ietf.org/doc/html/rfc3986)                   |
| locations                                           | Object             | Container for location information                                                                                                                                                                                                                                                                                                                                                                                               |                                                                                                          |
| locations.geonames_details                          | Object             | Container for details derived from the [GeoNames](https://www.geonames.org/) record for the GeoNames ID in locations.geonames_id                                                                                                                                                                                                                                                                                                 |                                                                                                          |
| locations.geonames_details.continent_code           | String             | 2-character code for the continent that the organization is located in, from the [GeoNames](https://www.geonames.org/) record for the GeoNames ID in locations.geonames_id.                                                                                                                                                                                                                                                      | Required. All records have a value in the field.                                                         |
| locations.geonames_details.continent_name           | String             | Name of the continent that the organization is located in, from the [GeoNames](https://www.geonames.org/)  record for the GeoNames ID in locations.geonames_id.                                                                                                                                                                                                                                                                  | Required. All records have a value in the field.                                                         |
| locations.geonames_details.country_code             | String             | ISO 3166-2 code for the country that the organization is located in, from the [GeoNames](https://www.geonames.org/) record for the GeoNames ID in locations.geonames_id                                                                                                                                                                                                                                                          | Valid 2-character [ISO 3166](https://www.iso.org/iso-3166-country-codes.html)-2 country code (uppercase) |
| locations.geonames_details.country_name             | String             | Name of the country that the organization is located in, from the [GeoNames](https://www.geonames.org/)  record for the GeoNames ID in locations.geonames_id                                                                                                                                                                                                                                                                     |                                                                                                          |
| locations.geonames_details.country_subdivision_code | String             | 2 or 3-character code for the highest-level country subdivision that the organization is located in, from the admin1Codes field of the [GeoNames](https://www.geonames.org/)   record for the GeoNames ID in locations.geonames_id.                                                                                                                                                                                              | Derived from [ISO-3166-2](https://en.wikipedia.org/wiki/ISO_3166-2) .                                    |
| locations.geonames_details.country_subdivision_name | String             | Name of the highest-level country subdivision that the organization is located in, from the admin1Codes field of the [GeoNames](https://www.geonames.org/)    record for the GeoNames ID in locations.geonames_id.                                                                                                                                                                                                               | Derived from [ISO-3166-2](https://en.wikipedia.org/wiki/ISO_3166-2)  .                                   |
| locations.geonames_details.lat                      | Number             | Latitude of the location identified in locations.geonames_id, from the [GeoNames](https://www.geonames.org/) record for that Geonames ID                                                                                                                                                                                                                                                                                         |                                                                                                          |
| locations.geonames_details.lng                      | Number             | Longitude of the location identified in locations.geonames_id, from the [GeoNames](https://www.geonames.org/) record for that GeoNames ID                                                                                                                                                                                                                                                                                        |                                                                                                          |
| locations.geonames_details.name                     | String             | Name of the location (e.g., city or town) identified in locations.geonames_id, from the [GeoNames](https://www.geonames.org/) record for that GeoNames ID.                                                                                                                                                                                                                                                                       |                                                                                                          |
| locations.geonames_id                               | Integer            | [GeoNames](https://www.geonames.org/) ID for the city or most granular administrative region that the organization is located in. For most records, this ID represents a city, but for organizations not located in a city, the value in this field is ID of the most granular administrative region for the location available in GeoNames.                                                                                     | Valid [GeoNames](https://www.geonames.org/) ID                                                           |
| names                                               | Object             | Container for name information                                                                                                                                                                                                                                                                                                                                                                                                   |                                                                                                          |
| names.lang                                          | String             | ISO 639-1 language code that identifies the language of a value in names.value. May be used with any name type(s).                                                                                                                                                                                                                                                                                                               | Valid 2-character [ISO 639](https://www.iso.org/iso-639-language-codes.html)-1 language code (lowercase) |
| names.types                                         | Array (of strings) | The type(s) associated with the name contained in names.value. Each name must have at least 1 type, and exactly 1 name must have ror_display in its types. Each name can have multiple types, for example ror_display and label.                                                                                                                                                                                                 | acronym, alias, label, ror_display                                                                       |
| names.value                                         | String             | Name that the organization is (or was) known by, which may be a current official name, former name, alias, acronym, etc.                                                                                                                                                                                                                                                                                                         |                                                                                                          |
| relationships                                       | Object             | Container for relationship information                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                          |
| relationships.id                                    | String             | Unique ROR ID of another organization which is related to the organization                                                                                                                                                                                                                                                                                                                                                       |                                                                                                          |
| relationships.label                                 | String             | Name of another organization identified in relationships.id, which is related to the organization                                                                                                                                                                                                                                                                                                                                |                                                                                                          |
| relationships.type                                  | String             | Type of relationship between the organization and another organization identified in relationships.id                                                                                                                                                                                                                                                                                                                            | child, parent, related, successor, predecessor                                                           |
| status                                              | String             | Whether the organization is active or not                                                                                                                                                                                                                                                                                                                                                                                        | active, inactive, withdrawn                                                                              |
| types                                               | Array (of strings) | Organization type(s). Allowed types: Education, Healthcare, Company, Archive, Nonprofit, Government, Facility, Funder, Other                                                                                                                                                                                                                                                                                                     | archive, company, education, facility, funder, government, healthcare, other                             |

# Example record

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
   "established" : 1992,
   "external_ids" : [
      {
         "all" : [
            "501100003441"
         ],
         "preferred" : null,
         "type" : "fundref"
      },
      {
         "all" : [
            "grid.410566.0"
         ],
         "preferred" : "grid.410566.0",
         "type" : "grid"
      },
      {
         "all" : [
            "0000 0004 0626 3303"
         ],
         "preferred" : null,
         "type" : "isni"
      },
      {
         "all" : [
            "Q1920549"
         ],
         "preferred" : null,
         "type" : "wikidata"
      }
   ],
   "id" : "https://ror.org/00xmkp704",
   "links" : [
      {
         "type" : "website",
         "value" : "http://www.uzgent.be/"
      },
      {
         "type" : "wikipedia",
         "value" : "https://en.wikipedia.org/wiki/Ghent_University_Hospital"
      }
   ],
   "locations" : [
      {
         "geonames_details" : {
            "continent_code" : "EU",
            "continent_name" : "Europe",
            "country_code" : "BE",
            "country_name" : "Belgium",
            "country_subdivision_code" : "VLG",
            "country_subdivision_name" : "Flanders",
            "lat" : 51.05,
            "lng" : 3.71667,
            "name" : "Ghent"
         },
         "geonames_id" : 2797656
      }
   ],
   "names" : [
      {
         "lang" : "en",
         "types" : [
            "ror_display",
            "label"
         ],
         "value" : "Ghent University Hospital"
      },
      {
         "lang" : "nl",
         "types" : [
            "alias"
         ],
         "value" : "UZ Gent"
      },
      {
         "lang" : "nl",
         "types" : [
            "label"
         ],
         "value" : "Universitair Ziekenhuis Gent"
      }
   ],
   "relationships" : [
      {
         "id" : "https://ror.org/00cv9y106",
         "label" : "Ghent University",
         "type" : "related"
      }
   ],
   "status" : "active",
   "types" : [
      "funder",
      "healthcare"
   ]
}
```

<br />

<br />
