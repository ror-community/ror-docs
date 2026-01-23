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

The original ROR metadata schema inherited from GRID in 2019 is now known as [Schema 1.0](doc:schema-v1). After two rounds of community feedback in 2022/2023, [Schema 2.0](doc:schema-v2) was developed and beta-tested, then launched into production in April 2024. [Schema 2.1](doc:schema-v2-1), a minor update to v2 that adds additional location information, was launched in December 2024 and is the recommended and most recent version.

Version 1.0 of the ROR schema was permanently sunset in 2025. Read more about ROR [schema versioning](doc:schema-versions).

If you'd like to request a change to the ROR metadata schema, please [submit a schema change request on our roadmap](https://github.com/ror-community/ror-roadmap/issues/new/choose).

# Fields

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
         "date" : "2026-01-15",
         "schema_version" : "2.1"
      }
   },
   "domains" : [],
   "established" : 2010,
   "external_ids" : [
      {
         "all" : [
            "501100005737",
            "501100019125",
            "100012946"
         ],
         "preferred" : "501100019125",
         "type" : "fundref"
      },
      {
         "all" : [
            "grid.462844.8"
         ],
         "preferred" : "grid.462844.8",
         "type" : "grid"
      },
      {
         "all" : [
            "0000 0001 2308 1657"
         ],
         "preferred" : null,
         "type" : "isni"
      },
      {
         "all" : [
            "Q41497113",
            "Q3491150",
            "Q546118",
            "Q1144549"
         ],
         "preferred" : "Q41497113",
         "type" : "wikidata"
      }
   ],
   "id" : "https://ror.org/02en5vm52",
   "links" : [
      {
         "type" : "website",
         "value" : "https://www.sorbonne-universite.fr"
      },
      {
         "type" : "wikipedia",
         "value" : "https://en.wikipedia.org/wiki/Sorbonne_University"
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
            "alias"
         ],
         "value" : "Pierre and Marie Curie University"
      },
      {
         "lang" : "en",
         "types" : [
            "label"
         ],
         "value" : "Sorbonne University"
      },
      {
         "lang" : "fr",
         "types" : [
            "ror_display",
            "label"
         ],
         "value" : "Sorbonne Université"
      },
      {
         "lang" : "en",
         "types" : [
            "alias"
         ],
         "value" : "University of Paris-Sorbonne"
      }
   ],
   "relationships" : [
      {
         "id" : "https://ror.org/0293jn610",
         "label" : "Adaptation et Diversité en Milieu Marin",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/049xh5y45",
         "label" : "Laboratoire d'Ecogéochimie des Environnements Benthiques",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03wg93s13",
         "label" : "Biologie Intégrative des Organismes Marins",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01tp1c480",
         "label" : "Biologie des Organismes et Écosystèmes Aquatiques",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02b9znm90",
         "label" : "Laboratoire d’Imagerie Biomédicale",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03jqm0b89",
         "label" : "Centre André-Chastel",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/0375b8f90",
         "label" : "Centre d'Immunologie et des Maladies Infectieuses",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/059v8sw69",
         "label" : "Centre d’étude de la langue et des littératures françaises",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03wxndv36",
         "label" : "Centre de Recherche Saint-Antoine",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00dmms154",
         "label" : "Centre de Recherche des Cordeliers",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02d19mr69",
         "label" : "Centre de recherches sur la pensée antique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00sad8321",
         "label" : "Centre d'Écologie et des Sciences de la Conservation",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/028ta1f94",
         "label" : "Chimie de la Matière Condensée de Paris",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02q6fa122",
         "label" : "Dynamique de l'information génétique : bases fondamentales et cancer",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05xtktk35",
         "label" : "Géoazur",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00kzsxx38",
         "label" : "Groupe d'Étude des Méthodes de l'Analyse Sociologique de la Sorbonne",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05dfxeg46",
         "label" : "Institut Henri Poincaré",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/043we9s22",
         "label" : "Institut Jean Le Rond d'Alembert",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04qwfwm19",
         "label" : "Institut Parisien de Chimie Moléculaire",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01c2cjg59",
         "label" : "Institut de Biologie Paris-Seine",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03fk87k11",
         "label" : "Institut de Mathématiques de Jussieu-Paris Rive Gauche",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01dadvw90",
         "label" : "Institut de Systématique, Évolution, Biodiversité",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00xagyq07",
         "label" : "Institut des Sciences de la Terre de Paris",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/050gn5214",
         "label" : "Institut du Cerveau",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05neq8668",
         "label" : "Institut Systèmes Intelligents et de Robotique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02s56xp85",
         "label" : "Institut d'écologie et des sciences de l'environnement de Paris",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05abgg682",
         "label" : "Institut de minéralogie, de physique des matériaux et de cosmochimie",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/0270xt841",
         "label" : "Institut de Myologie",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01h14ww21",
         "label" : "Laboratoire Kastler Brossel",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00jb20a14",
         "label" : "Laboratoire Interfaces et Systèmes Électrochimiques",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04xmteb38",
         "label" : "Laboratoire Jacques-Louis Lions",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01ghvgs84",
         "label" : "Laboratoire Jean Perrin",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05j3atf73",
         "label" : "Laboratoire d'Océanographie et du Climat : Expérimentations et Approches Numériques",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02m0hy518",
         "label" : "Laboratoire d'études sur les monothéismes",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/039p01270",
         "label" : "Laboratoire de Biodiversité et Biotechnologies Microbiennes",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04hke8425",
         "label" : "Laboratoire de Biologie du Développement de Villefranche-sur-Mer",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03z20vp15",
         "label" : "Laboratoire de Chimie Physique - Matière et Rayonnement",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00tmb7y09",
         "label" : "Laboratoire de Chimie Théorique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02fv42846",
         "label" : "Laboratoire de chimie des processus biologiques",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02xnnng09",
         "label" : "Laboratoire de Génie Électrique et Électronique de Paris",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/000ehr937",
         "label" : "Laboratoire de Météorologie Dynamique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01hg8p552",
         "label" : "Laboratoire de Physique Nucléaire et de Hautes Énergies",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05c95bg36",
         "label" : "Laboratoire de Physique des Plasmas",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05krcen59",
         "label" : "LIP6",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04vthwx70",
         "label" : "Laboratoire de Réactivité de Surface",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02ctqqq48",
         "label" : "Laboratoire d’Archéologie Moléculaire et Structurale",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/002ty1h48",
         "label" : "Laboratoire pour l'Utilisation des Lasers Intenses",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00pcqj134",
         "label" : "Biologie Computationnelle, Quantitative et Synthétique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/001c8pb03",
         "label" : "Laboratoire de Biologie Intégrative des Modèles Marins",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03cqwn895",
         "label" : "Laboratoire d’immunologie intégrative du cancer",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05nk54s89",
         "label" : "Laboratoire d'Océanographie Microbienne",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05mx55f96",
         "label" : "Laboratoire de Biologie Moléculaire et Cellulaire des Eucaryotes",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00a72jq18",
         "label" : "Laboratoire de Physique et d’Étude des Matériaux",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04zaaa143",
         "label" : "Laboratoire de Physique Théorique de la Matière Condensée",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02mph9k76",
         "label" : "Laboratoire de Physique Théorique et Hautes Energies",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00kr24y60",
         "label" : "Institut Langevin",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/0366b1491",
         "label" : "Metabolism and Renal Physiology",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/0229z0679",
         "label" : "Milieux environnementaux, transferts et interactions dans les hydrosystèmes et les sols",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02jkg8m11",
         "label" : "Molécule aux Nanos-objets : Réactivité, Interactions et Spectroscopies",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04team556",
         "label" : "Dynamique du noyau",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05gz4kr37",
         "label" : "Observatoire Océanologique de Banyuls-sur-Mer",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/051gvfp41",
         "label" : "Orient & Méditerranée",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03t2f0a12",
         "label" : "Institut des NanoSciences de Paris",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/055ss7a31",
         "label" : "Physique des Cellules et Cancers",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/046htjf88",
         "label" : "PHENIX laboratory",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/001r32c80",
         "label" : "Biologie du chloroplaste et perception de la lumière chez les micro-algues",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03kr50w79",
         "label" : "Physique et Mécanique des Milieux Hétérogènes",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02qqh1125",
         "label" : "Institut Pierre Louis d‘Épidémiologie et de Santé Publique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/043y2tx42",
         "label" : "Unité de recherche sur les maladies cardiovasculaires et métaboliques",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03s0pzj56",
         "label" : "Station Biologique de Roscoff",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03f0z1g15",
         "label" : "Sciences et Ingénierie de la Matière Molle",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03s877y45",
         "label" : "Chimie du Solide et Energie",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03ae8w006",
         "label" : "Sorbonne - Identités, Relations Internationales et Civilisations de l’Europe",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04w11tv37",
         "label" : "Biologie cellulaire et Cancer",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/053kxkj53",
         "label" : "Unité de Modélisation Mathématique et Informatique des Systèmes Complexes",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05r5y6641",
         "label" : "Laboratoire d’Océanographie de Villefranche",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/000nfmp17",
         "label" : "Centre de Recherche sur l'Amérique Pré-hispanique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01exas502",
         "label" : "Centre d'investigation clinique Quinze-Vingts",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05dbe0t82",
         "label" : "Immunologie - Immunopathologie - Immunothérapie",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05ax2x637",
         "label" : "Centre de recherche en paléontologie - Paris",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/007xn7v02",
         "label" : "Japanese-French Laboratory for Informatics",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03ygj8248",
         "label" : "Europe orientale, balkanique et médiane",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04gf9wd48",
         "label" : "Institut de Recherche en Musicologie",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/0259td381",
         "label" : "Neurophysiologie respiratoire expérimentale et clinique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02vnd0e65",
         "label" : "Laboratoire de Probabilités, Statistique et Modélisation",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01vrz7264",
         "label" : "Génétique et biologie du développement",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/000zhpw23",
         "label" : "Institut de la Vision",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05re0sm29",
         "label" : "École des Neurosciences de Paris",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05ppf7q77",
         "label" : "Laboratoire atmosphères, milieux, observations spatiales",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01d7n9638",
         "label" : "ITER",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/027jrtw17",
         "label" : "UMI MajuLab",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03a26mh11",
         "label" : "Laboratoire de Physique de l'Ecole Normale Supérieure",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02haar591",
         "label" : "Institut Pierre-Simon Laplace",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04ezpxa16",
         "label" : "Biologie du Chloroplaste et Perception de la Lumière chez les Microalgues",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01ms54x07",
         "label" : "UMS-Autonomie",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/015cf0x85",
         "label" : "Edition, Interprétation et traduction des textes anciens",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/006jv0w93",
         "label" : "Centre de Recherches Interdisciplinaires sur les Mondes Ibéro-américains Contemporains",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04vwsc311",
         "label" : "Centre de Recherche sur l’Extrême Orient de Paris – Sorbonne",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04hrxxd90",
         "label" : "Centre d'histoire du XIXe siècle",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04sv5r538",
         "label" : "Métaphysique, histoires, transformations, actualités",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/025xvn046",
         "label" : "Sciences et Technologies de la Musique et du Son",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05pm2xt51",
         "label" : "UMS Phénotypage du petit animal",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01w1erp60",
         "label" : "Couplage Multi-physiques et Multi-échelles en mécanique géo-environnemental",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/0557mft23",
         "label" : "Formation, Innovation, Recherche, Services et Transfert en Temps-Fréquence",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00s0m4j44",
         "label" : "Centre Expert en Endométriose",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04shr9s31",
         "label" : "Robotique et Innovation Chirurgicale",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/011gr5n11",
         "label" : "Handicap moteur et cognitif et réadaptation",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03f1mtj27",
         "label" : "Analyse, Recherche, Développement et Evaluation en Endourologie et Lithiase Urinaire",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01gapzp55",
         "label" : "Centre Roland Mousnier",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04p8das24",
         "label" : "Groupe de recherche interdisciplinaire sur les processus d'information et de communication",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01ph0t361",
         "label" : "Fondation Voir & Entendre",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02zzf0m94",
         "label" : "Centre de linguistique en Sorbonne",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04fgntk96",
         "label" : "Tara Oceans Systems Ecology & Evolution",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02qvhgb32",
         "label" : "Réanimation et soins intensifs du patient en Insuffisance respiratoire aigüe",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05kytrf83",
         "label" : "Méthodes et Outils pour les Sciences Participatives",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/0387w4y93",
         "label" : "Maladies génétiques d’expression pédiatrique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04tb23024",
         "label" : "Laboratoire Médiations",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01zprwd36",
         "label" : "Centre d'Acquisition et de Traitement des Images",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02g0s3c07",
         "label" : "Transplantation et Thérapies Innovantes de la Cornée",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01jnrzt83",
         "label" : "Biomarqueurs d’urgence et de réanimation",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/011hja523",
         "label" : "Rome et ses renaissances : arts, archéologie, littérature et philosophie",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00fyy4d45",
         "label" : "CERES - Centre d'expérimentation en méthodes numériques pour les recherches en sciences humaines et sociales",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/033eqsk67",
         "label" : "Sciences, Normes, Démocratie",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04m69ya83",
         "label" : "Théâtre antique : textes, histoire et réception",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00y16rm59",
         "label" : "Institut des Sciences du Calcul et des Données",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00njafa27",
         "label" : "Complications Cardiovasculaires et Métaboliques chez les patients vivant avec le VIH",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05gzcy376",
         "label" : "Groupe de Recherche Clinique en Neuro-urologie",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/058qr4y65",
         "label" : "Alzheimer Precision Medicine",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/0138eq662",
         "label" : "Onco-Urologie Prédictive",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/011mac819",
         "label" : "Histoire et Archéologie Maritimes",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03p2d3k93",
         "label" : "Production et analyse de données en sciences de la vie et en santé",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00xwwwr97",
         "label" : "Institut Parisien de Chimie Physique et Théorique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01jr1v359",
         "label" : "Laboratoire d'Informatique Médicale et d'Ingénierie des Connaissances en e-Santé",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05n2mnn68",
         "label" : "Enzymologie de l'ARN",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01r19bq53",
         "label" : "Replication des chromosomes eucaryotes et ses points de contrôle",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02r1kz177",
         "label" : "Bioinformatique Moléculaire",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01365h357",
         "label" : "Observatoire des sciences de l'Univers Paris-Centre Ecce Terra",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05jpad840",
         "label" : "Institut de la Mer de Villefranche",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03vkkad71",
         "label" : "Histoire et Dynamique des Espaces Anglophones: du Réel au Virtuel",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/037ptrs19",
         "label" : "Étude et Édition de Textes Médiévaux",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02zgjrf98",
         "label" : "Fondation Sciences mathématiques de Paris",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/053sj3y70",
         "label" : "Représentations et Identités. Espaces Germanique, Nordique et Néerlandophone",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01acw3598",
         "label" : "Centre d'Études Médiévales Anglaises",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04ef65y11",
         "label" : "Institut de Chimie Moléculaire de Paris : organique, inorganique et biologique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/015tg7676",
         "label" : "Drépanocytose : groupe de Recherche de Paris – Sorbonne Université",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00x677422",
         "label" : "IMAGES : La médecine de la femme et de l’enfant assistée par l’image",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02e3eqz10",
         "label" : "Centre de Recherche en Myologie",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04xy2by41",
         "label" : "Civilisations et littératures d'Espagne et d'Amérique du Moyen-Age aux Lumières",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02q5emw59",
         "label" : "Sens, Texte, Informatique, Histoire",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/015th7t48",
         "label" : "Centre de Recherche en Littérature Comparée",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05ntgv723",
         "label" : "Équipe Littérature et Culture italiennes",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/013s2ts57",
         "label" : "Groupe d’Étude sur l’HyperTension Intra Crânienne idiopathique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02zn90974",
         "label" : "Groupe de recherche clinique en anesthésie réanimation médecine périopératoire",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04n40rk24",
         "label" : "Groupe de recherche clinique – Tumeurs Thyroïdiennes",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02qe0rk31",
         "label" : "THERANOSCAN : Biomarqueurs Théranostiques des Cancers Bronchiques Non à Petites Cellules",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02ffzdx16",
         "label" : "Paris Network for Advanced Microscopy",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00a8q8z31",
         "label" : "NeurON : Interface Neuro-machine",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04ph5sm90",
         "label" : "PremUP",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00adasd53",
         "label" : "Paris Centre for Quantum Technologies",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/02m26fm05",
         "label" : "Voix Anglophones : Littérature et Esthétique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00qdphm77",
         "label" : "Nutrition et obésité : approches systémiques",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01sra1980",
         "label" : "Remodelage et Reparation du Tissu Renal",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01x3ydm75",
         "label" : "Maladies rénales fréquentes et rares : des mécanismes moléculaires à la médecine personnalisée",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/022bnxw24",
         "label" : "Institut d'Astrophysique de Paris",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01rrvyp50",
         "label" : "OURAGAN: Outils de Résolution Algébriques pour la Géométrie et ses Applications",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/002v3cy83",
         "label" : "Pôle de Recherche pour l'Organisation et la Diffusion de l'Information Géographique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/044feat76",
         "label" : "SUMMIT",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03hgsyv65",
         "label" : "Laboratoire Temps Espace",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/050c3pq49",
         "label" : "Fondation pour l’innovation en Cadiométabolisme et Nutrition",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04c2tm284",
         "label" : "AC Echo : Analyse Centralisée Échocardiographique en imagerie cardiovasculaire",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00nqhfc30",
         "label" : "GR-Trans : Groupe de Recherche en Transidentités",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04n97g567",
         "label" : "HTAM2 : Hypertension Maligne Multimodale",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/0100b4v88",
         "label" : "Neurodev : Troubles du neuro-développement : du fœtus à l'adulte",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04nz36f20",
         "label" : "Nova : inNOvation in NeuroVAscular diseases",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04bsm4075",
         "label" : "SoLID : SOrbonne study group for Lung Infectious Diseases",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01e26yv04",
         "label" : "SURDI.AD : Surdités neurosensorielles complexes et réhabilitation de l'audition chez l'adulte",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00rw1dq87",
         "label" : "Université des patients",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03p96ke84",
         "label" : "Développement Adaptation et Vieillissement",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/04kjehf08",
         "label" : "Laboratoire d'Activation Moléculaire",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00cpdar66",
         "label" : "Laboratoire d'Étude de l'Univers et des Phénomènes Extrêmes",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05ebxsr90",
         "label" : "Chimie Physique et Chimie du Vivant",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/017zz7s54",
         "label" : "LIRA - Laboratoire d'instrumentation et de recherche en astrophysique",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03r83qv57",
         "label" : "France Cohortes",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05pchb838",
         "label" : "Structure et Instabilité des Génomes",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/022bnxw24",
         "label" : "Institut d'Astrophysique de Paris",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/0038zss60",
         "label" : "European Marine Biological Resource Centre",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/00yfbr841",
         "label" : "Hôpital Armand-Trousseau",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/05h5v3c50",
         "label" : "Hôpital Tenon",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/04v3xcy66",
         "label" : "Hôpital Charles-Foix",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/009kb8w74",
         "label" : "Hôpital Rothschild",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/01875pg84",
         "label" : "Hôpital Saint-Antoine",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/02mh9a093",
         "label" : "Pitié-Salpêtrière Hospital",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/02t1zz428",
         "label" : "Hôpital de la Roche-Guyon",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/024v1ns19",
         "label" : "Centre hospitalier national d'ophtalmologie des Quinze-Vingts",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/04yem5s35",
         "label" : "INFRANALYTICS",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/0459fdx51",
         "label" : "Fédération de Recherche sur l'Energie Solaire",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/01nrtdp55",
         "label" : "GDR NBODY : Problème quantique à N corps en chimie et physique",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/05hb8m595",
         "label" : "PHOTOSYNTHESE",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/05be9p317",
         "label" : "Micropesanteur Fondamentale et Appliquée",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/00hgbrg14",
         "label" : "Fédération française Matériaux sous hautes vitesses de déformation. Application aux matériaux en conditions extrêmes, Procédés et structures",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/01sgwka45",
         "label" : "Microscopie Fonctionnelle du Vivant",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/01bbf9m56",
         "label" : "Fédération Francilienne de Mécanique - Matériaux, Structures, Procédés",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/03qgfy688",
         "label" : "Fédération Ile de France de recherche sur l'environnement",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/03hkqjb05",
         "label" : "Fédération de Recherche Interactions Fondamentales",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/01x6z5t49",
         "label" : "Fédération de Recherche Spectroscopies de Photoémission",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/02rmk5x23",
         "label" : "Fédération de recherche PLAS@PAR",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/02jwwk370",
         "label" : "Fédération de recherche en sciences mathématiques de Paris centre",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/00mqc8k54",
         "label" : "Fédération de Chimie et Matériaux de Paris-Centre",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/04bzgtz06",
         "label" : "Takuvik Joint International Laboratory",
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

<br />

<br />
