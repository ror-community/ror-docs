---
title: Filtering
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR API filtering
  description: Instructions for filtering lists of ROR records retrieved from the ROR API.
  robots: index
---
# About filtering

Results from standard [queries](doc:api-query) and [advanced queries](doc:api-advanced-query) can be filtered by record status, organization type, [ISO 3166](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code, country name, continent code, and continent name.

* `status` - filter records by record status
* `types` - filter records by organization type
* `country.country_code` and `locations.geonames_details.country_code` - filter records by country code
* `country.country_name` and `locations.geonames_details.country_name` - filter records by country name
* `locations.geonames_details.continent_code` - filter records by continent code
* `locations.geonames_details.continent_name` - filter records by continent name

See [Data structure](doc:ror-data-structure) and [Fields and sub-fields](doc:fields) for more details about these fields.

> 🚧 Download ROR data for complete aggregate lists
>
> ROR API filters are designed to limit the number of results of standard [queries](doc:api-query) and [advanced queries](doc:api-advanced-query), not to be used on their own. If you would like to retrieve a list of all organizations in ROR with a particular status or type or a list of all organizations in a particular country or on a particular continent, download the ROR [Data dump](doc:data-dump) and filter by field name. Using ROR API filters without a query can result in <Anchor label="duplicates and omissions across different pages of results" target="_blank" href="https://github.com/ror-community/ror-roadmap/issues/333">duplicates and omissions across different pages of results</Anchor>.

Note that in version 2 of the ROR API and schema, values formerly in the v1 field `country.country_code` are now in the field `locations.geonames_details.country_code`and values formerly in the v1 field `country.country_name` are now in the field `locations.geonames_details.country_name`. To maintain continuity for v1 users, we have ensured that the v1 and v2 field names for country names and country codes can be used interchangeably as filters in v2 of the ROR API.

> 📘 Filter format
>
> `https://api.ror.org/v2/organizations?query=[query]&filter=[filter]:[value]`

# Filter by record status

Limit search results by records of a particular status. Available statuses are _active_, _inactive_, and _withdrawn_. By default, queries of the ROR API return only records with _active_ status. Requests to [Retrieve a single record](doc:api-single) by its ROR ID will always return the record regardless of its status without need for the [all_status parameter](doc:api-list#retrieve-a-list-of-records-with-all-statuses) or for status filters.

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query=Sorbonne&filter=status:inactive' | json_pp
```

Retrieves a list of records for inactive research organizations with the keyword "Sorbonne" in a `names` field.

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
         "established" : 2010,
         "external_ids" : [
            {
               "all" : [
                  "501100009516"
               ],
               "preferred" : "501100009516",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.469994.f"
               ],
               "preferred" : "grid.469994.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1788 6194"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3491149"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/001z21q04",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.sorbonne-paris-cite.fr/en"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Sorbonne_Paris_Cit%C3%A9_University_(group)"
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
               "value" : "Center for Research and Higher Education Sorbonne Paris Cité"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "PRES"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Pôle de Recherche et d’Enseignement Supérieur Sorbonne Paris Cité"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Sorbonne Paris Cité"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Université Sorbonne Paris Cité"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03saykv37",
               "label" : "Laboratoire Vision Action Cognition",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00kneq391",
               "label" : "Laboratory Orofacial Pathologies, Imaging and Biotherapies",
               "type" : "child"
            }
         ],
         "status" : "inactive",
         "types" : [
            "funder",
            "other"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2023-12-07",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2025-08-26",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2019,
         "external_ids" : [
            {
               "all" : [
                  "Q123695093"
               ],
               "preferred" : "Q123695093",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01zpxh127",
         "links" : [
            {
               "type" : "website",
               "value" : "https://sante.sorbonne-universite.fr/structures-de-recherche/groupe-de-recherche-clinique-amylose-aa-sorbonne-universite-graasu"
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
                  "label"
               ],
               "value" : "Clinical Research Group on Inflammatory Amyloidosis (AA) Sorbonne University"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "GRAASU"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "GRC 28"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "GRC Amylose AA Sorbonne Université"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Groupe de recherche clinique Amylose AA Sorbonne Université"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "parent"
            }
         ],
         "status" : "inactive",
         "types" : [
            "facility"
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
            "id" : "fr",
            "title" : "France"
         }
      ],
      "statuses" : [
         {
            "count" : 2,
            "id" : "inactive",
            "title" : "inactive"
         }
      ],
      "types" : [
         {
            "count" : 1,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 1,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 1,
            "id" : "other",
            "title" : "other"
         }
      ]
   },
   "number_of_results" : 2,
   "time_taken" : 3
}
```

# Filter by organization type

Limit search results by organization type. Available organization types:

* education
* funder
* healthcare
* company
* archive
* nonprofit
* government
* facility
* other

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query=Pasteur&filter=types:facility' | json_pp
```

Retrieves a list of active research facilities with the keyword "Pasteur" in a `names` field.

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
         "established" : 1913,
         "external_ids" : [
            {
               "all" : [
                  "grid.418828.f"
               ],
               "preferred" : "grid.418828.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q587434"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00akhwn95",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.saovabha.com/en/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Queen_Saovabha_Memorial_Institute"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "TH",
                  "country_name" : "Thailand",
                  "country_subdivision_code" : "10",
                  "country_subdivision_name" : "Bangkok",
                  "lat" : 13.75398,
                  "lng" : 100.50144,
                  "name" : "Bangkok"
               },
               "geonames_id" : 1609350
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Pasteur Institute"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "QSMI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Queen Saovabha Memorial Institute"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "Sathan Pasteur"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "Sathan Saovabha"
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
               "date" : "2024-04-30",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1903,
         "external_ids" : [
            {
               "all" : [
                  "0000 0005 0955 754X"
               ],
               "preferred" : "0000 0005 0955 754X",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q4201403"
               ],
               "preferred" : "Q4201403",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/046fcjp93",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.saude.sp.gov.br/instituto-pasteur/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://pt.wikipedia.org/wiki/Instituto_Pasteur_(S%C3%A3o_Paulo)"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SP",
                  "country_subdivision_name" : "São Paulo",
                  "lat" : -23.5475,
                  "lng" : -46.63611,
                  "name" : "São Paulo"
               },
               "geonames_id" : 3448439
            }
         ],
         "names" : [
            {
               "lang" : "pt",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Instituto Pasteur"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Instituto Pasteur São Paulo"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Pasteur Institute"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Pasteur Institute São Paulo"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
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
         "established" : 1854,
         "external_ids" : [
            {
               "all" : [
                  "501100013340"
               ],
               "preferred" : "501100013340",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.8970.6"
               ],
               "preferred" : "grid.8970.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2159 9858"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3151788"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05k9skc85",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.pasteur-lille.fr/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Pasteur_Institute_of_Lille"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "HDF",
                  "country_subdivision_name" : "Hauts-de-France",
                  "lat" : 50.63297,
                  "lng" : 3.05858,
                  "name" : "Lille"
               },
               "geonames_id" : 2998324
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Institut Pasteur de Lille"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Pasteur Institute of Lille"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Pasteur-Lille"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00dyt5s15",
               "label" : "Center for Infection and Immunity of Lille",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05gd4yq49",
               "label" : "Institut de Biologie de Lille",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0300mzg60",
               "label" : "Integrated Genomics and Metabolic Diseases Modeling",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05xanhp90",
               "label" : "Récepteurs Nucléaires, Maladies Cardiovasculaires et Diabète",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05x9zmx47",
               "label" : "CANTHER - Hétérogénéité, Plasticité et Résistance aux Thérapies des Cancers",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04k5h2q42",
               "label" : "(Epi)génomique fonctionnelle métabolique et des dysfonctions dans le diabète de type 2 et des maladies associées",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0455vgw31",
               "label" : "IMPact de l'Environnement Chimique sur la Santé humaine",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0086epd30",
               "label" : "Médicaments et Molécules pour Agir sur les Systèmes Vivants",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/056hav897",
               "label" : "Plateformes Lilloises en Biologie et Santé",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03rvrjk28",
               "label" : "Facteurs de risque et déterminants moléculaires des maladies liées au vieillissement",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03pbmtd68",
               "label" : "Recherche translationnelle sur le diabète",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "facility",
            "funder"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2023-08-17",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2025-10-06",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2005,
         "external_ids" : [
            {
               "all" : [
                  "Q51780559"
               ],
               "preferred" : "Q51780559",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01c1p7750",
         "links" : [
            {
               "type" : "website",
               "value" : "https://hopitauxcentre-u-pariscite.aphp.fr/cic-vaccinologie-cochin-pasteur/"
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
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "CIC 1417"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "CIC Cochin Pasteur"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "CIC biothérapie et vaccinologie Cochin-Pasteur"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Centre d'Investigation Clinique de Vaccinologie Cochin-Pasteur"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02vjkv261",
               "label" : "Inserm",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05f82e368",
               "label" : "Université Paris Cité",
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
         "established" : 1894,
         "external_ids" : [
            {
               "all" : [
                  "grid.418520.a"
               ],
               "preferred" : "grid.418520.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2163 7615"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/052b46n78",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.pasteur.dz/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "DZ",
                  "country_name" : "Algeria",
                  "country_subdivision_code" : "16",
                  "country_subdivision_name" : "Algiers",
                  "lat" : 36.73225,
                  "lng" : 3.08746,
                  "name" : "Algiers"
               },
               "geonames_id" : 2507480
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut Pasteur d'Algérie"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
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
         "established" : 1921,
         "external_ids" : [
            {
               "all" : [
                  "501100010679"
               ],
               "preferred" : "501100010679",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.420169.8"
               ],
               "preferred" : "grid.420169.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9562 2611"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q17013593"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00wqczk30",
         "links" : [
            {
               "type" : "website",
               "value" : "http://en.pasteur.ac.ir/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Pasteur_Institute_of_Iran"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "IR",
                  "country_name" : "Iran",
                  "country_subdivision_code" : "23",
                  "country_subdivision_name" : "Tehran",
                  "lat" : 35.69439,
                  "lng" : 51.42151,
                  "name" : "Tehran"
               },
               "geonames_id" : 112931
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IPI"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Institut Pasteur D'Iran"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "PII"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Pasteur Institute of Iran"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "facility",
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
         "established" : 2006,
         "external_ids" : [
            {
               "all" : [
                  "501100016217"
               ],
               "preferred" : "501100016217",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.418532.9"
               ],
               "preferred" : "grid.418532.9",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0403 6035"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5917646"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04dpm2z73",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.pasteur.edu.uy/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "UY",
                  "country_name" : "Uruguay",
                  "country_subdivision_code" : "MO",
                  "country_subdivision_name" : "Montevideo Department",
                  "lat" : -34.90328,
                  "lng" : -56.18816,
                  "name" : "Montevideo"
               },
               "geonames_id" : 3441575
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "alias"
               ],
               "value" : "IP Montevideo"
            },
            {
               "lang" : "es",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut Pasteur de Montevideo"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "facility",
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
         "established" : 1953,
         "external_ids" : [
            {
               "all" : [
                  "grid.418537.c"
               ],
               "preferred" : "grid.418537.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 7535 978X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30281766"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03ht2dx40",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.pasteur-kh.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "KH",
                  "country_name" : "Cambodia",
                  "country_subdivision_code" : "12",
                  "country_subdivision_name" : "Phnom Penh",
                  "lat" : 11.56245,
                  "lng" : 104.91601,
                  "name" : "Phnom Penh"
               },
               "geonames_id" : 1821306
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IPC"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut Pasteur du Cambodge"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
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
         "established" : 1990,
         "external_ids" : [
            {
               "all" : [
                  "grid.418539.2"
               ],
               "preferred" : "grid.418539.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9089 1740"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30281767"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04yb4j419",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.pasteur.ma/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "MA",
                  "country_name" : "Morocco",
                  "country_subdivision_code" : "06",
                  "country_subdivision_name" : "Casablanca-Settat",
                  "lat" : 33.58831,
                  "lng" : -7.61138,
                  "name" : "Casablanca"
               },
               "geonames_id" : 2553604
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IPM"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut Pasteur du Maroc"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
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
               "date" : "2024-07-08",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "pasteur.la"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/02qkn0e91",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.pasteur.la"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "LA",
                  "country_name" : "Laos",
                  "country_subdivision_code" : "VT",
                  "country_subdivision_name" : "Vientiane Prefecture",
                  "lat" : 17.96667,
                  "lng" : 102.6,
                  "name" : "Vientiane"
               },
               "geonames_id" : 1651944
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Foundation Institut Pasteur du Laos"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Institut Pasteur du Laos"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Institut Pasteur of Laos"
            },
            {
               "lang" : "lo",
               "types" : [
                  "label"
               ],
               "value" : "ສະຖາບັນ ປັດສະເຕີ ລາວ"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
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
               "date" : "2024-07-08",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "ipn.org.vn"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "Q10832773"
               ],
               "preferred" : "Q10832773",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02m5qpk42",
         "links" : [
            {
               "type" : "website",
               "value" : "https://ipn.org.vn"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "VN",
                  "country_name" : "Vietnam",
                  "country_subdivision_code" : "34",
                  "country_subdivision_name" : "Khánh Hòa Province",
                  "lat" : 12.24507,
                  "lng" : 109.19432,
                  "name" : "Nha Trang"
               },
               "geonames_id" : 1572151
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Institut Pasteur de Nha Trang"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Pasteur Institute in Nha Trang"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Pasteur Institute of Nha Trang"
            },
            {
               "lang" : "vi",
               "types" : [
                  "alias"
               ],
               "value" : "Viện Pasteur Nha Trang"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
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
         "established" : 2000,
         "external_ids" : [
            {
               "all" : [
                  "grid.482283.7"
               ],
               "preferred" : "grid.482283.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30273934"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03wwjjr94",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.hkupasteur.hku.hk/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "HK",
                  "country_name" : "Hong Kong",
                  "country_subdivision_code" : null,
                  "country_subdivision_name" : null,
                  "lat" : 22.27832,
                  "lng" : 114.17469,
                  "name" : "Hong Kong"
               },
               "geonames_id" : 1819729
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HKU-PRP"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "HKU-Pasteur Research Pole"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02zhqgq86",
               "label" : "University of Hong Kong",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
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
         "established" : 1893,
         "external_ids" : [
            {
               "all" : [
                  "501100023300"
               ],
               "preferred" : "501100023300",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.418517.e"
               ],
               "preferred" : "grid.418517.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2298 7385"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/04pwyer06",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.pasteur.tn/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "TN",
                  "country_name" : "Tunisia",
                  "country_subdivision_code" : "11",
                  "country_subdivision_name" : "Tunis Governorate",
                  "lat" : 36.81897,
                  "lng" : 10.16579,
                  "name" : "Tunis"
               },
               "geonames_id" : 2464470
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IPT"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut Pasteur de Tunis"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "facility",
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
               "date" : "2025-10-28",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "pasteur-yaounde.org"
         ],
         "established" : 1959,
         "external_ids" : [
            {
               "all" : [
                  "grid.418179.2"
               ],
               "preferred" : "grid.418179.2",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/0259hk390",
         "links" : [
            {
               "type" : "website",
               "value" : "https://pasteur-yaounde.org"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "CM",
                  "country_name" : "Cameroon",
                  "country_subdivision_code" : "CE",
                  "country_subdivision_name" : "Centre",
                  "lat" : 3.86667,
                  "lng" : 11.51667,
                  "name" : "Yaoundé"
               },
               "geonames_id" : 2220957
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CPC"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Centre Pasteur du Cameroun"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
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
         "established" : 1955,
         "external_ids" : [
            {
               "all" : [
                  "grid.418534.f"
               ],
               "preferred" : "grid.418534.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0443 0155"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30281765"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04sqtjj61",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.institutpasteur.nc/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "NC",
                  "country_name" : "New Caledonia",
                  "country_subdivision_code" : "S",
                  "country_subdivision_name" : "South Province",
                  "lat" : -22.27407,
                  "lng" : 166.44884,
                  "name" : "Noumea"
               },
               "geonames_id" : 2139521
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IPNC"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut Pasteur de Nouvelle Calédonie"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
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
         "established" : 1940,
         "external_ids" : [
            {
               "all" : [
                  "grid.418525.f"
               ],
               "preferred" : "grid.418525.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2206 8813"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30281759"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01fp8z436",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.pasteur-cayenne.fr/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "GF",
                  "country_name" : "French Guiana",
                  "country_subdivision_code" : null,
                  "country_subdivision_name" : "Guyane",
                  "lat" : 4.93333,
                  "lng" : -52.33333,
                  "name" : "Cayenne"
               },
               "geonames_id" : 3382160
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut Pasteur de la Guyane"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
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
         "established" : 1924,
         "external_ids" : [
            {
               "all" : [
                  "grid.452920.8"
               ],
               "preferred" : "grid.452920.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 5930 4500"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30296498"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/042cxsy45",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.pasteur-guadeloupe.fr/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "GP",
                  "country_name" : "Guadeloupe",
                  "country_subdivision_code" : null,
                  "country_subdivision_name" : "Guadeloupe",
                  "lat" : 16.273,
                  "lng" : -61.50507,
                  "name" : "Les Abymes"
               },
               "geonames_id" : 3578959
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IPGP"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut Pasteur de la Guadeloupe"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0495fxg12",
               "label" : "Institut Pasteur",
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
         "established" : 1986,
         "external_ids" : [
            {
               "all" : [
                  "grid.452539.c"
               ],
               "preferred" : "grid.452539.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0621 0957"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/032t7yz93",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.louis-pasteur.or.jp/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "26",
                  "country_subdivision_name" : "Kyoto",
                  "lat" : 35.02107,
                  "lng" : 135.75385,
                  "name" : "Kyoto"
               },
               "geonames_id" : 1857910
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Louis Pasteur Center for Medical Research"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "公益財団法人ルイ・パストゥール医学研究センター"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "facility"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 7,
            "id" : "as",
            "title" : "Asia"
         },
         {
            "count" : 4,
            "id" : "af",
            "title" : "Africa"
         },
         {
            "count" : 3,
            "id" : "sa",
            "title" : "South America"
         },
         {
            "count" : 2,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 1,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 1,
            "id" : "oc",
            "title" : "Oceania"
         }
      ],
      "countries" : [
         {
            "count" : 2,
            "id" : "fr",
            "title" : "France"
         },
         {
            "count" : 1,
            "id" : "br",
            "title" : "Brazil"
         },
         {
            "count" : 1,
            "id" : "cm",
            "title" : "Cameroon"
         },
         {
            "count" : 1,
            "id" : "dz",
            "title" : "Algeria"
         },
         {
            "count" : 1,
            "id" : "gf",
            "title" : "French Guiana"
         },
         {
            "count" : 1,
            "id" : "gp",
            "title" : "Guadeloupe"
         },
         {
            "count" : 1,
            "id" : "hk",
            "title" : "Hong Kong"
         },
         {
            "count" : 1,
            "id" : "ir",
            "title" : "Iran"
         },
         {
            "count" : 1,
            "id" : "jp",
            "title" : "Japan"
         },
         {
            "count" : 1,
            "id" : "kh",
            "title" : "Cambodia"
         }
      ],
      "statuses" : [
         {
            "count" : 18,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 18,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 4,
            "id" : "funder",
            "title" : "funder"
         }
      ]
   },
   "number_of_results" : 18,
   "time_taken" : 4
}
```

# Filter by country code

Limit search results by ISO country code. Available country codes are those in the [ISO 3166 alpha-2 list](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2).

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query=Mandela&filter=country.country_code:ZA' | json_pp
```

Returns a list of active research organizations in South Africa with the keyword "Mandela" in a `names` field.

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
            "mandela.ac.za"
         ],
         "established" : 2005,
         "external_ids" : [
            {
               "all" : [
                  "501100001340",
                  "501100013969"
               ],
               "preferred" : "501100013969",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.412139.c"
               ],
               "preferred" : "grid.412139.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2191 3608"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1331614"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03r1jm528",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.mandela.ac.za"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Nelson_Mandela_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "EC",
                  "country_subdivision_name" : "Eastern Cape",
                  "lat" : -33.96109,
                  "lng" : 25.61494,
                  "name" : "Port Elizabeth"
               },
               "geonames_id" : 964420
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "NMU"
            },
            {
               "lang" : "af",
               "types" : [
                  "label"
               ],
               "value" : "Nelson Mandela Metropolitaanse Universiteit"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Nelson Mandela Metropolitan University"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Nelson Mandela University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02qgf1459",
               "label" : "Dora Nginza Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00f0wme23",
               "label" : "Livingstone Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01ypqqm93",
               "label" : "Port Elizabeth Provincial Hospital",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.461156.1"
               ],
               "preferred" : "grid.461156.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0490 0241"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q6990620"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/036z4hp15",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.ecdoh.gov.za/hospitals/71/Nelson_Mandela_Academic_Hospital"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Nelson_Mandela_Academic_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "EC",
                  "country_subdivision_name" : "Eastern Cape",
                  "lat" : -31.58893,
                  "lng" : 28.78443,
                  "name" : "Mthatha"
               },
               "geonames_id" : 946058
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Nelson Mandela Academic Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02svzjn28",
               "label" : "Walter Sisulu University",
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
               "date" : "2020-03-15",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2010,
         "external_ids" : [
            {
               "all" : [
                  "grid.507727.0"
               ],
               "preferred" : "grid.507727.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q19787384"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01pxptg16",
         "links" : [
            {
               "type" : "website",
               "value" : "https://minds-africa.org"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Mandela_Institute_for_Development_Studies"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZA",
                  "country_name" : "South Africa",
                  "country_subdivision_code" : "GP",
                  "country_subdivision_name" : "Gauteng",
                  "lat" : -26.104,
                  "lng" : 28.054,
                  "name" : "Sandton"
               },
               "geonames_id" : 957654
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "MINDS"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Mandela Institute for Development Studies"
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
            "count" : 3,
            "id" : "af",
            "title" : "Africa"
         }
      ],
      "countries" : [
         {
            "count" : 3,
            "id" : "za",
            "title" : "South Africa"
         }
      ],
      "statuses" : [
         {
            "count" : 3,
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
         },
         {
            "count" : 1,
            "id" : "healthcare",
            "title" : "healthcare"
         }
      ]
   },
   "number_of_results" : 3,
   "time_taken" : 2
}
```

# Filter by country name

Limit search results by country name. Available country names are those <Anchor label="provided by Geonames" target="_blank" href="https://www.geonames.org/countries/">provided by Geonames</Anchor>.

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query=Recherche&filter=locations.geonames_details.country_name:Senegal' | json_pp
```

Returns a list of active research organizations in Senegal with the keyword "Recherche" in a `names` field.

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
         "established" : 1937,
         "external_ids" : [
            {
               "all" : [
                  "grid.418291.7"
               ],
               "preferred" : "grid.418291.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0456 337X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q28974592"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/015q23935",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.senegal.ird.fr/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Institut_de_recherche_pour_le_d%C3%A9veloppement"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "French Research Institute for Development"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IRD"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut de Recherche pour le Développement"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/011xg1225",
               "label" : "Montpellier Interdisciplinary center on Sustainable Agri-food systems: social and nutritional sciences",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03vte9x46",
               "label" : "Observatoire des Sciences de l'Univers de Grenoble",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05q3vnk25",
               "label" : "Institut de Recherche pour le Développement",
               "type" : "parent"
            }
         ],
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.473221.1"
               ],
               "preferred" : "grid.473221.1",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/045rh7r61",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.cerd.dj/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "DJ",
                  "country_name" : "Djibouti",
                  "country_subdivision_code" : "DJ",
                  "country_subdivision_name" : "Djibouti",
                  "lat" : 11.58901,
                  "lng" : 43.14503,
                  "name" : "Djibouti"
               },
               "geonames_id" : 223817
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CERD"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Centre d'Étude et de Recherche de Djibouti"
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
         "established" : 2004,
         "external_ids" : [
            {
               "all" : [
                  "grid.463194.d"
               ],
               "preferred" : "grid.463194.d",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04aa9w798",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.cres-sn.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CRES"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Consortium pour la recherche économique et sociale"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.436924.e"
               ],
               "preferred" : "grid.436924.e",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/05nkfrm56",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.mesr.gouv.sn/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministère de l'Enseignement Superieur et de la Recherche"
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
               "date" : "2019-02-17",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2017,
         "external_ids" : [
            {
               "all" : [
                  "grid.503074.5"
               ],
               "preferred" : "grid.503074.5",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/01psmkn05",
         "links" : [
            {
               "type" : "website",
               "value" : "https://iressef.org/en/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IRESSEF"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Institut de Recherche en Santé, de Surveillance Épidémiologique et de Formation"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institute of Health Research, Epidemiological Surveillance and Training"
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
         "established" : 1973,
         "external_ids" : [
            {
               "all" : [
                  "501100001902"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.469327.d"
               ],
               "preferred" : "grid.469327.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 1401 0777"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q2044761"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02rs18x11",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.codesria.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Council_for_the_Development_of_Social_Science_Research_in_Africa"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CODESRIA"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Conseil pour le développement de la recherche en sciences sociales en Afrique"
            },
            {
               "lang" : "pt",
               "types" : [
                  "label"
               ],
               "value" : "Conselho para o Desenvolvimento da Pesquisa em Ciências Sociais em África"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Council for the Development of Social Science Research in Africa"
            },
            {
               "lang" : "ar",
               "types" : [
                  "label"
               ],
               "value" : "مجلس تنمية البحوث الإجتماعية في أفريقي"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
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
         "established" : 1987,
         "external_ids" : [
            {
               "all" : [
                  "grid.463266.3"
               ],
               "preferred" : "grid.463266.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2289 3215"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/00hcr7c15",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.coraf.org/en/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CORAF/WECARD"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Conseil ouest et centre africain pour la recherche et le développement agricoles"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "West and Central African Council for Agricultural Research and Development"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "other"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 7,
            "id" : "af",
            "title" : "Africa"
         }
      ],
      "countries" : [
         {
            "count" : 6,
            "id" : "sn",
            "title" : "Senegal"
         },
         {
            "count" : 1,
            "id" : "dj",
            "title" : "Djibouti"
         }
      ],
      "statuses" : [
         {
            "count" : 7,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 3,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 2,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 1,
            "id" : "facility",
            "title" : "facility"
         },
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
   "number_of_results" : 7,
   "time_taken" : 30
}
(base) amandafrench@MacBook-Pro-8 Websites % curl 'https://api.ror.org/v2/organizations?query=Recherche&filter=locations.geonames_details.country_name:Senegal' | json_pp 
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  7093  100  7093    0     0  20205      0 --:--:-- --:--:-- --:--:-- 20207
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
         "established" : 1937,
         "external_ids" : [
            {
               "all" : [
                  "grid.418291.7"
               ],
               "preferred" : "grid.418291.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0456 337X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q28974592"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/015q23935",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.senegal.ird.fr/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Institut_de_recherche_pour_le_d%C3%A9veloppement"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "French Research Institute for Development"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IRD"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut de Recherche pour le Développement"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/011xg1225",
               "label" : "Montpellier Interdisciplinary center on Sustainable Agri-food systems: social and nutritional sciences",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03vte9x46",
               "label" : "Observatoire des Sciences de l'Univers de Grenoble",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05q3vnk25",
               "label" : "Institut de Recherche pour le Développement",
               "type" : "parent"
            }
         ],
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
         "established" : 2004,
         "external_ids" : [
            {
               "all" : [
                  "grid.463194.d"
               ],
               "preferred" : "grid.463194.d",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04aa9w798",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.cres-sn.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CRES"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Consortium pour la recherche économique et sociale"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.436924.e"
               ],
               "preferred" : "grid.436924.e",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/05nkfrm56",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.mesr.gouv.sn/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministère de l'Enseignement Superieur et de la Recherche"
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
               "date" : "2019-02-17",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2017,
         "external_ids" : [
            {
               "all" : [
                  "grid.503074.5"
               ],
               "preferred" : "grid.503074.5",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/01psmkn05",
         "links" : [
            {
               "type" : "website",
               "value" : "https://iressef.org/en/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IRESSEF"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Institut de Recherche en Santé, de Surveillance Épidémiologique et de Formation"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institute of Health Research, Epidemiological Surveillance and Training"
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
         "established" : 1987,
         "external_ids" : [
            {
               "all" : [
                  "grid.463266.3"
               ],
               "preferred" : "grid.463266.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2289 3215"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/00hcr7c15",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.coraf.org/en/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CORAF/WECARD"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Conseil ouest et centre africain pour la recherche et le développement agricoles"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "West and Central African Council for Agricultural Research and Development"
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
         "established" : 1973,
         "external_ids" : [
            {
               "all" : [
                  "501100001902"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.469327.d"
               ],
               "preferred" : "grid.469327.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 1401 0777"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q2044761"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02rs18x11",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.codesria.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Council_for_the_Development_of_Social_Science_Research_in_Africa"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CODESRIA"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Conseil pour le développement de la recherche en sciences sociales en Afrique"
            },
            {
               "lang" : "pt",
               "types" : [
                  "label"
               ],
               "value" : "Conselho para o Desenvolvimento da Pesquisa em Ciências Sociais em África"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Council for the Development of Social Science Research in Africa"
            },
            {
               "lang" : "ar",
               "types" : [
                  "label"
               ],
               "value" : "مجلس تنمية البحوث الإجتماعية في أفريقي"
            }
         ],
         "relationships" : [],
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
            "count" : 6,
            "id" : "af",
            "title" : "Africa"
         }
      ],
      "countries" : [
         {
            "count" : 6,
            "id" : "sn",
            "title" : "Senegal"
         }
      ],
      "statuses" : [
         {
            "count" : 6,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 3,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 2,
            "id" : "other",
            "title" : "other"
         },
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
   "number_of_results" : 6,
   "time_taken" : 3
}

```

# Filter by continent code

Limit search results by continent code. Continent codes and names are provided by [GeoNames](https://geonames.org): AF (Africa), AN (Antarctica), AS (Asia), EU (Europe), NA (North America), OC (Oceania), and SA (South America).

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query=%22Ministry+of=Health%22&filter=locations.geonames_details.continent_code:AF' | json_pp
```

Returns a list of active research organizations in Africa with the exact term "Ministry of Health" in a `names` field.

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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.436176.1"
               ],
               "preferred" : "grid.436176.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30290560"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04es3ne34",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.minsa.gov.ao/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "AO",
                  "country_name" : "Angola",
                  "country_subdivision_code" : "LUA",
                  "country_subdivision_name" : "Luanda",
                  "lat" : -8.83682,
                  "lng" : 13.23432,
                  "name" : "Luanda"
               },
               "geonames_id" : 2240449
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "MINSA"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health"
            },
            {
               "lang" : "pt",
               "types" : [
                  "label"
               ],
               "value" : "Ministério da Saúde"
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
         "established" : 2000,
         "external_ids" : [
            {
               "all" : [
                  "grid.436179.e"
               ],
               "preferred" : "grid.436179.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30290562"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04yadxf37",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.gov.ls/ministry-of-health/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "LS",
                  "country_name" : "Lesotho",
                  "country_subdivision_code" : "A",
                  "country_subdivision_name" : "Maseru District",
                  "lat" : -29.31667,
                  "lng" : 27.48333,
                  "name" : "Maseru"
               },
               "geonames_id" : 932505
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health"
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
         "established" : 1986,
         "external_ids" : [
            {
               "all" : [
                  "grid.415722.7"
               ],
               "preferred" : "grid.415722.7",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/0357r2107",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.malawi.gov.mw/index.php?option=com_content&view=article&id=50&Itemid=22"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "MW",
                  "country_name" : "Malawi",
                  "country_subdivision_code" : "C",
                  "country_subdivision_name" : "Central Region",
                  "lat" : -13.96692,
                  "lng" : 33.78725,
                  "name" : "Lilongwe"
               },
               "geonames_id" : 927967
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/045z18t19",
               "label" : "Malawi Epidemiology and Intervention Research Unit",
               "type" : "child"
            }
         ],
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "501100005975",
                  "501100005976"
               ],
               "preferred" : "501100005975",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.415727.2"
               ],
               "preferred" : "grid.415727.2",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/02eyff421",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.health.go.ke/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Ministry_of_Health_(Kenya)"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "KE",
                  "country_name" : "Kenya",
                  "country_subdivision_code" : "30",
                  "country_subdivision_name" : "Nairobi County",
                  "lat" : -1.28333,
                  "lng" : 36.81667,
                  "name" : "Nairobi"
               },
               "geonames_id" : 184745
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Ministry of Public Health and Sanitation"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05q89dp90",
               "label" : "Pharmacy and Poisons Board",
               "type" : "child"
            }
         ],
         "status" : "active",
         "types" : [
            "funder",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.415752.0"
               ],
               "preferred" : "grid.415752.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0457 1249"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/059f2k568",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.misau.gov.mz/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "MZ",
                  "country_name" : "Mozambique",
                  "country_subdivision_code" : "MPM",
                  "country_subdivision_name" : "Maputo City",
                  "lat" : -25.96553,
                  "lng" : 32.58322,
                  "name" : "Maputo"
               },
               "geonames_id" : 1040652
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "MISAU"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health"
            },
            {
               "lang" : "pt",
               "types" : [
                  "alias"
               ],
               "value" : "Ministério da Saúde"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.415794.a"
               ],
               "preferred" : "grid.415794.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q6867107"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00hpqmv06",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.moh.gov.zm/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Ministry_of_Health_(Zambia)"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ZM",
                  "country_name" : "Zambia",
                  "country_subdivision_code" : "09",
                  "country_subdivision_name" : "Lusaka Province",
                  "lat" : -15.40669,
                  "lng" : 28.28713,
                  "name" : "Lusaka"
               },
               "geonames_id" : 909137
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.415807.f"
               ],
               "preferred" : "grid.415807.f",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04551r843",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.moh.gov.bw/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "BW",
                  "country_name" : "Botswana",
                  "country_subdivision_code" : "GA",
                  "country_subdivision_name" : "Gaborone",
                  "lat" : -24.65451,
                  "lng" : 25.90859,
                  "name" : "Gaborone"
               },
               "geonames_id" : 933773
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.463475.7"
               ],
               "preferred" : "grid.463475.7",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/053ykx779",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.gov.sz/index.php?option=com_content&view=article&id=267&Itemid=403"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SZ",
                  "country_name" : "Eswatini",
                  "country_subdivision_code" : "HH",
                  "country_subdivision_name" : "Hhohho Region",
                  "lat" : -26.31667,
                  "lng" : 31.13333,
                  "name" : "Mbabane"
               },
               "geonames_id" : 934985
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health"
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
         "established" : 1957,
         "external_ids" : [
            {
               "all" : [
                  "grid.415765.4"
               ],
               "preferred" : "grid.415765.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0721 5002"
               ],
               "preferred" : "0000 0001 0721 5002",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q6867086"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05c7h4935",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.moh.gov.gh"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Ministry_of_Health_(Ghana)"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "GH",
                  "country_name" : "Ghana",
                  "country_subdivision_code" : "AA",
                  "country_subdivision_name" : "Greater Accra",
                  "lat" : 5.55602,
                  "lng" : -0.1969,
                  "name" : "Accra"
               },
               "geonames_id" : 2306104
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Ghanaian Ministry of Health"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Ministry of Health of Ghana"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "MoH"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/052ss8w32",
               "label" : "Ghana Health Service",
               "type" : "child"
            }
         ],
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
               "date" : "2025-06-24",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.450284.f"
               ],
               "preferred" : "grid.450284.f",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04rkgkn20",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.health.gov.sc/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SC",
                  "country_name" : "Seychelles",
                  "country_subdivision_code" : "16",
                  "country_subdivision_name" : "La Rivière Anglaise",
                  "lat" : -4.62001,
                  "lng" : 55.45501,
                  "name" : "Victoria"
               },
               "geonames_id" : 241131
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01na3y974",
               "label" : "Seychelles Public Health Authority",
               "type" : "child"
            }
         ],
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
               "date" : "2025-08-26",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "501100015550"
               ],
               "preferred" : "501100015550",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.415705.2"
               ],
               "preferred" : "grid.415705.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q28223289"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00hy3gq97",
         "links" : [
            {
               "type" : "website",
               "value" : "http://health.go.ug/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Ministry_of_Health_(Uganda)"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "UG",
                  "country_name" : "Uganda",
                  "country_subdivision_code" : "C",
                  "country_subdivision_name" : "Central Region",
                  "lat" : 0.31628,
                  "lng" : 32.58219,
                  "name" : "Kampala"
               },
               "geonames_id" : 232422
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Uganda Ministry of Health"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03f6y6166",
               "label" : "Lira Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03r08g441",
               "label" : "Uganda Heart Institute",
               "type" : "child"
            }
         ],
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
               "date" : "2025-10-28",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "moh.gov.rw"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.421714.5"
               ],
               "preferred" : "grid.421714.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q5421497"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05prysf28",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.moh.gov.rw/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Ministry_of_Health_(Rwanda)"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "RW",
                  "country_name" : "Rwanda",
                  "country_subdivision_code" : "01",
                  "country_subdivision_name" : "Kigali",
                  "lat" : -1.94995,
                  "lng" : 30.05885,
                  "name" : "Kigali"
               },
               "geonames_id" : 202061
            }
         ],
         "names" : [
            {
               "lang" : "rw",
               "types" : [
                  "label"
               ],
               "value" : "Minisiteri y'Ubuzima"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Ministère de la Santé"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/038vngd42",
               "label" : "Centre Hospitalier Universitaire de Kigali",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03jggqf79",
               "label" : "Rwanda Biomedical Center",
               "type" : "child"
            }
         ],
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.434433.7"
               ],
               "preferred" : "grid.434433.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1764 1074"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5440303"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02v6nd536",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.health.gov.ng/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Federal_Ministry_of_Health_(Nigeria)"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "NG",
                  "country_name" : "Nigeria",
                  "country_subdivision_code" : "FC",
                  "country_subdivision_name" : "FCT",
                  "lat" : 9.05785,
                  "lng" : 7.49508,
                  "name" : "Abuja"
               },
               "geonames_id" : 2352778
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Federal Ministry of Health"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05j78sg27",
               "label" : "National Primary Health Care Development Agency",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05sjgdh57",
               "label" : "Nigeria Centre for Disease Control",
               "type" : "child"
            }
         ],
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.414827.c"
               ],
               "preferred" : "grid.414827.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q6867100"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01d59nd22",
         "links" : [
            {
               "type" : "website",
               "value" : "http://fmoh.gov.sd/En/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SD",
                  "country_name" : "Sudan",
                  "country_subdivision_code" : "KH",
                  "country_subdivision_name" : "Khartoum",
                  "lat" : 15.55177,
                  "lng" : 32.53241,
                  "name" : "Khartoum"
               },
               "geonames_id" : 379252
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Federal Ministry of Health"
            },
            {
               "lang" : "ar",
               "types" : [
                  "label"
               ],
               "value" : "وزارة الصحة الإتحادية"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.414835.f"
               ],
               "preferred" : "grid.414835.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0439 6364"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/017yk1e31",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.moh.gov.et/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "ET",
                  "country_name" : "Ethiopia",
                  "country_subdivision_code" : "AA",
                  "country_subdivision_name" : "Addis Ababa",
                  "lat" : 9.02497,
                  "lng" : 38.74689,
                  "name" : "Addis Ababa"
               },
               "geonames_id" : 344979
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "FMOH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Federal Ministry of Health"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03k3h8z07",
               "label" : "Oromiyaa Regional Health Bureau",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/058p87c41",
               "label" : "South Ethiopia Regional State Health Bureau",
               "type" : "child"
            }
         ],
         "status" : "active",
         "types" : [
            "government"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.511838.6"
               ],
               "preferred" : "grid.511838.6",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/05c3e8627",
         "links" : [
            {
               "type" : "website",
               "value" : "https://togoleseministryofhealthlome.myewebsite.com/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "TG",
                  "country_name" : "Togo",
                  "country_subdivision_code" : "M",
                  "country_subdivision_name" : "Maritime",
                  "lat" : 6.12874,
                  "lng" : 1.22154,
                  "name" : "Lomé"
               },
               "geonames_id" : 2365267
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "MOH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Togolese Ministry of Health"
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
         "established" : 1976,
         "external_ids" : [
            {
               "all" : [
                  "grid.490661.c"
               ],
               "preferred" : "grid.490661.c",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/040frxb47",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.nigerstatemoh.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "NG",
                  "country_name" : "Nigeria",
                  "country_subdivision_code" : "NI",
                  "country_subdivision_name" : "Niger State",
                  "lat" : 9.61524,
                  "lng" : 6.54776,
                  "name" : "Minna"
               },
               "geonames_id" : 2330100
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Niger State Ministry of Health"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.452667.4"
               ],
               "preferred" : "grid.452667.4",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04j0ce487",
         "links" : [
            {
               "type" : "website",
               "value" : "http://nsmoh.org.ng/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "NG",
                  "country_name" : "Nigeria",
                  "country_subdivision_code" : "NA",
                  "country_subdivision_name" : "Nasarawa State",
                  "lat" : 8.4939,
                  "lng" : 8.51532,
                  "name" : "Lafia"
               },
               "geonames_id" : 2332515
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Nasarawa State Ministry of Health"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "SMOH"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.463455.5"
               ],
               "preferred" : "grid.463455.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q21044897"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00yv7s489",
         "links" : [
            {
               "type" : "website",
               "value" : "http://health.gov.sl/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Ministry_of_Health_and_Sanitation_(Sierra_Leone)"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SL",
                  "country_name" : "Sierra Leone",
                  "country_subdivision_code" : "W",
                  "country_subdivision_name" : "Western Area",
                  "lat" : 8.48714,
                  "lng" : -13.2356,
                  "name" : "Freetown"
               },
               "geonames_id" : 2409306
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministry of Health and Sanitation"
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
               "date" : "2024-06-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/02hydzw41",
         "links" : [],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "NG",
                  "country_name" : "Nigeria",
                  "country_subdivision_code" : "OG",
                  "country_subdivision_name" : "Ogun State",
                  "lat" : 7.15571,
                  "lng" : 3.34509,
                  "name" : "Abeokuta"
               },
               "geonames_id" : 2352947
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "acronym"
               ],
               "value" : "OGSMH"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Ogun State Ministry of Health"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "government"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 31,
            "id" : "af",
            "title" : "Africa"
         }
      ],
      "countries" : [
         {
            "count" : 4,
            "id" : "ng",
            "title" : "Nigeria"
         },
         {
            "count" : 2,
            "id" : "sc",
            "title" : "Seychelles"
         },
         {
            "count" : 2,
            "id" : "tz",
            "title" : "Tanzania"
         },
         {
            "count" : 1,
            "id" : "ao",
            "title" : "Angola"
         },
         {
            "count" : 1,
            "id" : "bw",
            "title" : "Botswana"
         },
         {
            "count" : 1,
            "id" : "ci",
            "title" : "Ivory Coast"
         },
         {
            "count" : 1,
            "id" : "eg",
            "title" : "Egypt"
         },
         {
            "count" : 1,
            "id" : "et",
            "title" : "Ethiopia"
         },
         {
            "count" : 1,
            "id" : "gh",
            "title" : "Ghana"
         },
         {
            "count" : 1,
            "id" : "gm",
            "title" : "Gambia"
         }
      ],
      "statuses" : [
         {
            "count" : 31,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 31,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 1,
            "id" : "funder",
            "title" : "funder"
         }
      ]
   },
   "number_of_results" : 31,
   "time_taken" : 28
}

```

# Filter by continent name

Limit search results by continent name. Continent codes and names are provided by [GeoNames](https://geonames.org): AF (Africa), AN (Antarctica), AS (Asia), EU (Europe), NA (North America), OC (Oceania), and SA (South America).

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query=Laboratorio&filter=locations.geonames_details.continent_name:South+America' | json_pp
```

Returns a list of active research organizations in South America with the keyword "Laboratorio" in a `names` field.

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
         "established" : 1997,
         "external_ids" : [
            {
               "all" : [
                  "grid.456714.5"
               ],
               "preferred" : "grid.456714.5",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/00nsej663",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.labbacchi.com.br/conspat/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SP",
                  "country_subdivision_name" : "São Paulo",
                  "lat" : -22.88583,
                  "lng" : -48.445,
                  "name" : "Botucatu"
               },
               "geonames_id" : 3469136
            }
         ],
         "names" : [
            {
               "lang" : "pt",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratório Bacchi"
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
               "date" : "2024-11-18",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "labcedro.com.br"
         ],
         "established" : 1986,
         "external_ids" : [],
         "id" : "https://ror.org/01t80g375",
         "links" : [
            {
               "type" : "website",
               "value" : "https://labcedro.com.br"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Maranhão",
                  "lat" : -2.52972,
                  "lng" : -44.30278,
                  "name" : "São Luís"
               },
               "geonames_id" : 3388368
            }
         ],
         "names" : [
            {
               "lang" : "pt",
               "types" : [
                  "alias"
               ],
               "value" : "Cedro"
            },
            {
               "lang" : "pt",
               "types" : [
                  "alias"
               ],
               "value" : "Lab Cedro"
            },
            {
               "lang" : "pt",
               "types" : [
                  "label"
               ],
               "value" : "Laboratório Cedro"
            },
            {
               "lang" : "pt",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Laboratório Cedro (Brazil)"
            },
            {
               "lang" : "pt",
               "types" : [
                  "label"
               ],
               "value" : "Laboratório Cedro, Ltda."
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
               "date" : "2023-11-08",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2014,
         "external_ids" : [],
         "id" : "https://ror.org/01khfdf61",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.laboratorioflores.cl"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "CL",
                  "country_name" : "Chile",
                  "country_subdivision_code" : "RM",
                  "country_subdivision_name" : "Santiago Metropolitan",
                  "lat" : -33.45694,
                  "lng" : -70.64827,
                  "name" : "Santiago"
               },
               "geonames_id" : 3871336
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Fundación Flores"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "LABFLORES"
            },
            {
               "lang" : "es",
               "types" : [
                  "alias"
               ],
               "value" : "Laboratorio Flores"
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
               "date" : "2021-09-23",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2000,
         "external_ids" : [
            {
               "all" : [
                  "grid.512178.8"
               ],
               "preferred" : "grid.512178.8",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/00tr5rr68",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.dnacenter.com.br/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "RN",
                  "country_subdivision_name" : "Rio Grande do Norte",
                  "lat" : -5.795,
                  "lng" : -35.20944,
                  "name" : "Natal"
               },
               "geonames_id" : 3394023
            }
         ],
         "names" : [
            {
               "lang" : "pt",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratório DNA Center"
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
               "date" : "2019-02-17",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1947,
         "external_ids" : [
            {
               "all" : [
                  "grid.501306.1"
               ],
               "preferred" : "grid.501306.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q10315156"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04f02tt58",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.teuto.com.br/en"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "GO",
                  "country_subdivision_name" : "Goiás",
                  "lat" : -16.32667,
                  "lng" : -48.95278,
                  "name" : "Anápolis"
               },
               "geonames_id" : 3472287
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratório Teuto (Brazil)"
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
               "date" : "2023-08-17",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/02pzme005",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.gub.uy/ministerio-industria-energia-mineria/tramites-y-servicios/servicios/laboratorio-tecnogestion"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "UY",
                  "country_name" : "Uruguay",
                  "country_subdivision_code" : "MO",
                  "country_subdivision_name" : "Montevideo Department",
                  "lat" : -34.90328,
                  "lng" : -56.18816,
                  "name" : "Montevideo"
               },
               "geonames_id" : 3441575
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratorio de Tecnogestión"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "MIEM-LSMRI"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03e8mf369",
               "label" : "Ministerio de Industria, Energía y Minería",
               "type" : "parent"
            }
         ],
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
         "established" : 1985,
         "external_ids" : [
            {
               "all" : [
                  "grid.472887.6"
               ],
               "preferred" : "grid.472887.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0480 4831"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/055v9fh02",
         "links" : [
            {
               "type" : "website",
               "value" : "http://lnapadrao.lna.br/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "MG",
                  "country_subdivision_name" : "Minas Gerais",
                  "lat" : -22.42556,
                  "lng" : -45.45278,
                  "name" : "Itajubá"
               },
               "geonames_id" : 3460834
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LNA"
            },
            {
               "lang" : "pt",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratório Nacional de Astrofísica"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "National Astrophysics Laboratory"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/050zdnc69",
               "label" : "Ministry of Science, Technology and Innovation",
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
         "established" : 2008,
         "external_ids" : [
            {
               "all" : [
                  "grid.456710.1"
               ],
               "preferred" : "grid.456710.1",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04dj9qm44",
         "links" : [
            {
               "type" : "website",
               "value" : "http://biosintesis.com.br/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SP",
                  "country_subdivision_name" : "São Paulo",
                  "lat" : -23.5475,
                  "lng" : -46.63611,
                  "name" : "São Paulo"
               },
               "geonames_id" : 3448439
            }
         ],
         "names" : [
            {
               "lang" : "pt",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratório Biosintesis P&D"
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
               "date" : "2025-03-26",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-06-24",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "geoint.mx"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/022b6wh61",
         "links" : [
            {
               "type" : "website",
               "value" : "https://geoint.mx"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "MX",
                  "country_name" : "Mexico",
                  "country_subdivision_code" : "AGU",
                  "country_subdivision_name" : "Aguascalientes",
                  "lat" : 21.88262,
                  "lng" : -102.2843,
                  "name" : "Aguascalientes"
               },
               "geonames_id" : 4019233
            },
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "MX",
                  "country_name" : "Mexico",
                  "country_subdivision_code" : "CMX",
                  "country_subdivision_name" : "Mexico City",
                  "lat" : 19.42847,
                  "lng" : -99.12766,
                  "name" : "Mexico City"
               },
               "geonames_id" : 3530597
            },
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "VE",
                  "country_name" : "Venezuela",
                  "country_subdivision_code" : "L",
                  "country_subdivision_name" : "Mérida",
                  "lat" : 8.57899,
                  "lng" : -71.16922,
                  "name" : "Merida"
               },
               "geonames_id" : 3632308
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "alias"
               ],
               "value" : "GeoINT"
            },
            {
               "lang" : "es",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Laboratorio Nacional de GeoInteligencia"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "National Geointelligence Laboratory"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/059ex5q34",
               "label" : "Secretaría de Ciencia, Humanidades, Tecnología e Innovación",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "government"
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
         "established" : 2000,
         "external_ids" : [
            {
               "all" : [
                  "501100011876"
               ],
               "preferred" : "501100011876",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.509794.6"
               ],
               "preferred" : "grid.509794.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0445 080X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q65171876"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04kjcjc51",
         "links" : [
            {
               "type" : "website",
               "value" : "https://lnbio.cnpem.br/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SP",
                  "country_subdivision_name" : "São Paulo",
                  "lat" : -22.90556,
                  "lng" : -47.06083,
                  "name" : "Campinas"
               },
               "geonames_id" : 3467865
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Brazilian Biosciences National Laboratory"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LNBio"
            },
            {
               "lang" : "pt",
               "types" : [
                  "label"
               ],
               "value" : "Laboratório Nacional de Biociências"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05m235j20",
               "label" : "Brazilian Center for Research in Energy and Materials",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "facility",
            "funder"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2024-02-13",
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
                  "501100011877"
               ],
               "preferred" : "501100011877",
               "type" : "fundref"
            },
            {
               "all" : [
                  "0000 0004 0559 4469"
               ],
               "preferred" : "0000 0004 0559 4469",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q47462549"
               ],
               "preferred" : "Q47462549",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03e51yr27",
         "links" : [
            {
               "type" : "website",
               "value" : "https://lnnano.cnpem.br"
            },
            {
               "type" : "wikipedia",
               "value" : "https://pt.wikipedia.org/wiki/Laborat%C3%B3rio_Nacional_de_Nanotecnologia"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SP",
                  "country_subdivision_name" : "São Paulo",
                  "lat" : -22.90556,
                  "lng" : -47.06083,
                  "name" : "Campinas"
               },
               "geonames_id" : 3467865
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Brazilian Nanotechnology National Laboratory"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Brazilian National Laboratory of Nanotechnology"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LNN"
            },
            {
               "lang" : "pt",
               "types" : [
                  "alias"
               ],
               "value" : "LNNano"
            },
            {
               "lang" : "pt",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratório Nacional de Nanotecnologia"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/050zdnc69",
               "label" : "Ministry of Science, Technology and Innovation",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05m235j20",
               "label" : "Brazilian Center for Research in Energy and Materials",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "facility",
            "funder",
            "government"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2023-06-22",
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
                  "0000 0001 0357 2504"
               ],
               "preferred" : "0000 0001 0357 2504",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5968611"
               ],
               "preferred" : "Q5968611",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05gaped89",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.latu.org.uy"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "UY",
                  "country_name" : "Uruguay",
                  "country_subdivision_code" : "MO",
                  "country_subdivision_name" : "Montevideo Department",
                  "lat" : -34.90328,
                  "lng" : -56.18816,
                  "name" : "Montevideo"
               },
               "geonames_id" : 3441575
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LATU"
            },
            {
               "lang" : "es",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratorio Tecnológico del Uruguay"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.450337.6"
               ],
               "preferred" : "grid.450337.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30294862"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05b9w0m88",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.linea.gov.br/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "RJ",
                  "country_subdivision_name" : "Rio de Janeiro",
                  "lat" : -22.90642,
                  "lng" : -43.18223,
                  "name" : "Rio de Janeiro"
               },
               "geonames_id" : 3451190
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LIneA"
            },
            {
               "lang" : "pt",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratório Interinstitucional de e-Astronomia"
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
         "established" : 1999,
         "external_ids" : [
            {
               "all" : [
                  "grid.456718.9"
               ],
               "preferred" : "grid.456718.9",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/00wp8rt14",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.lsitec.org.br/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SP",
                  "country_subdivision_name" : "São Paulo",
                  "lat" : -23.5475,
                  "lng" : -46.63611,
                  "name" : "São Paulo"
               },
               "geonames_id" : 3448439
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Association of the Technological Integrated Systems Laboratory"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LSI TEC"
            },
            {
               "lang" : "pt",
               "types" : [
                  "label"
               ],
               "value" : "Laboratório de Sistemas Integráveis Tecnológico"
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
         "domains" : [
            "lncc.br"
         ],
         "established" : 1998,
         "external_ids" : [
            {
               "all" : [
                  "grid.452576.7"
               ],
               "preferred" : "grid.452576.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0602 9007"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/0498ekt05",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.gov.br/lncc/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "RJ",
                  "country_subdivision_name" : "Rio de Janeiro",
                  "lat" : -22.505,
                  "lng" : -43.17861,
                  "name" : "Petrópolis"
               },
               "geonames_id" : 3454031
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LNCC"
            },
            {
               "lang" : "pt",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratório Nacional de Computação Científica"
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
               "date" : "2020-12-21",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1997,
         "external_ids" : [
            {
               "all" : [
                  "501100011873"
               ],
               "preferred" : "501100011873",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.509791.3"
               ],
               "preferred" : "grid.509791.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9593 7568"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q6467316"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01p6gzq21",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.lnls.cnpem.br/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SP",
                  "country_subdivision_name" : "São Paulo",
                  "lat" : -22.90556,
                  "lng" : -47.06083,
                  "name" : "Campinas"
               },
               "geonames_id" : 3467865
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Brazilian Synchrotron Light Laboratory"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LNLS"
            },
            {
               "lang" : "pt",
               "types" : [
                  "label"
               ],
               "value" : "Laboratório Nacional de Luz Síncrotron"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0451nna70",
               "label" : "Sirius",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05m235j20",
               "label" : "Brazilian Center for Research in Energy and Materials",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "facility",
            "funder"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2024-06-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/049hngf66",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.fau.unlp.edu.ar/contenido/institucional/institutos-centros-y-laboratorios/lpge/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "AR",
                  "country_name" : "Argentina",
                  "country_subdivision_code" : "B",
                  "country_subdivision_name" : "Buenos Aires",
                  "lat" : -34.92145,
                  "lng" : -57.95453,
                  "name" : "La Plata"
               },
               "geonames_id" : 3432043
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "acronym"
               ],
               "value" : "LPGE"
            },
            {
               "lang" : "es",
               "types" : [
                  "acronym"
               ],
               "value" : "LPGE UNLP-CONICET"
            },
            {
               "lang" : "es",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Laboratorio de Planificación y Gestión Estratégica"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01tjs6929",
               "label" : "Universidad Nacional de La Plata",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03cqe8w59",
               "label" : "Consejo Nacional de Investigaciones Científicas y Técnicas",
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
         "established" : 2010,
         "external_ids" : [
            {
               "all" : [
                  "grid.452574.5"
               ],
               "preferred" : "grid.452574.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1797 1452"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/00w1v8g84",
         "links" : [
            {
               "type" : "website",
               "value" : "http://ctbe.cnpem.br/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SP",
                  "country_subdivision_name" : "São Paulo",
                  "lat" : -22.90556,
                  "lng" : -47.06083,
                  "name" : "Campinas"
               },
               "geonames_id" : 3467865
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CTBE"
            },
            {
               "lang" : "pt",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratório Nacional de Ciência e Tecnologia do Bioetanol"
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
               "date" : "2025-06-24",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-06-24",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "linta.cic.gba.gob.ar"
         ],
         "established" : 1991,
         "external_ids" : [],
         "id" : "https://ror.org/03w9ctg72",
         "links" : [
            {
               "type" : "website",
               "value" : "https://linta.cic.gba.gob.ar"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "AR",
                  "country_name" : "Argentina",
                  "country_subdivision_code" : "B",
                  "country_subdivision_name" : "Buenos Aires",
                  "lat" : -34.92126,
                  "lng" : -57.95442,
                  "name" : "La Plata"
               },
               "geonames_id" : 3432043
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "acronym"
               ],
               "value" : "LINTA"
            },
            {
               "lang" : "es",
               "types" : [
                  "acronym"
               ],
               "value" : "LINTA-CIC"
            },
            {
               "lang" : "es",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Laboratorio de Investigaciones del Territorio y el Ambiente"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02s7sax82",
               "label" : "Comisión de Investigaciones Científicas",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "facility",
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
         "established" : 1996,
         "external_ids" : [
            {
               "all" : [
                  "grid.456529.9"
               ],
               "preferred" : "grid.456529.9",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 1456 6310"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/035y1f917",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.celafiscs.org.br/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "SA",
                  "continent_name" : "South America",
                  "country_code" : "BR",
                  "country_name" : "Brazil",
                  "country_subdivision_code" : "SP",
                  "country_subdivision_name" : "São Paulo",
                  "lat" : -23.62306,
                  "lng" : -46.55111,
                  "name" : "São Caetano do Sul"
               },
               "geonames_id" : 3449324
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CELAFISCS"
            },
            {
               "lang" : "pt",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Centro de Estudos do Laboratório de Aptidão Física de São Caetano do Sul"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "other"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 20,
            "id" : "sa",
            "title" : "South America"
         },
         {
            "count" : 1,
            "id" : "na",
            "title" : "North America"
         }
      ],
      "countries" : [
         {
            "count" : 14,
            "id" : "br",
            "title" : "Brazil"
         },
         {
            "count" : 2,
            "id" : "ar",
            "title" : "Argentina"
         },
         {
            "count" : 2,
            "id" : "uy",
            "title" : "Uruguay"
         },
         {
            "count" : 1,
            "id" : "cl",
            "title" : "Chile"
         },
         {
            "count" : 1,
            "id" : "mx",
            "title" : "Mexico"
         },
         {
            "count" : 1,
            "id" : "ve",
            "title" : "Venezuela"
         }
      ],
      "statuses" : [
         {
            "count" : 20,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 10,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 6,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 3,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 3,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 2,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 1,
            "id" : "nonprofit",
            "title" : "nonprofit"
         }
      ]
   },
   "number_of_results" : 20,

```

# Combine filters

You can combine multiple filters of any kind in a single request.

> 📘 Format: Combined filters
>
> `https://api.ror.org/v2/organizations?filter=<filter>:<value>,<filter>:<value>`

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query=Recherche&filter=country.country_name:Senegal,country.country_name:Djibouti' | json_pp
```

Returns a list of active research organizations in both Senegal and Djibouti with the keyword "Recherche" in a `names` field.

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
         "established" : 1937,
         "external_ids" : [
            {
               "all" : [
                  "grid.418291.7"
               ],
               "preferred" : "grid.418291.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0456 337X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q28974592"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/015q23935",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.senegal.ird.fr/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Institut_de_recherche_pour_le_d%C3%A9veloppement"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "French Research Institute for Development"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IRD"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut de Recherche pour le Développement"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/011xg1225",
               "label" : "Montpellier Interdisciplinary center on Sustainable Agri-food systems: social and nutritional sciences",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03vte9x46",
               "label" : "Observatoire des Sciences de l'Univers de Grenoble",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05q3vnk25",
               "label" : "Institut de Recherche pour le Développement",
               "type" : "parent"
            }
         ],
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.473221.1"
               ],
               "preferred" : "grid.473221.1",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/045rh7r61",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.cerd.dj/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "DJ",
                  "country_name" : "Djibouti",
                  "country_subdivision_code" : "DJ",
                  "country_subdivision_name" : "Djibouti",
                  "lat" : 11.58901,
                  "lng" : 43.14503,
                  "name" : "Djibouti"
               },
               "geonames_id" : 223817
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CERD"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Centre d'Étude et de Recherche de Djibouti"
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
         "established" : 2004,
         "external_ids" : [
            {
               "all" : [
                  "grid.463194.d"
               ],
               "preferred" : "grid.463194.d",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04aa9w798",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.cres-sn.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CRES"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Consortium pour la recherche économique et sociale"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.436924.e"
               ],
               "preferred" : "grid.436924.e",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/05nkfrm56",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.mesr.gouv.sn/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ministère de l'Enseignement Superieur et de la Recherche"
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
               "date" : "2019-02-17",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2017,
         "external_ids" : [
            {
               "all" : [
                  "grid.503074.5"
               ],
               "preferred" : "grid.503074.5",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/01psmkn05",
         "links" : [
            {
               "type" : "website",
               "value" : "https://iressef.org/en/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IRESSEF"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Institut de Recherche en Santé, de Surveillance Épidémiologique et de Formation"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institute of Health Research, Epidemiological Surveillance and Training"
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
         "established" : 1973,
         "external_ids" : [
            {
               "all" : [
                  "501100001902"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.469327.d"
               ],
               "preferred" : "grid.469327.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 1401 0777"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q2044761"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02rs18x11",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.codesria.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Council_for_the_Development_of_Social_Science_Research_in_Africa"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CODESRIA"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Conseil pour le développement de la recherche en sciences sociales en Afrique"
            },
            {
               "lang" : "pt",
               "types" : [
                  "label"
               ],
               "value" : "Conselho para o Desenvolvimento da Pesquisa em Ciências Sociais em África"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Council for the Development of Social Science Research in Africa"
            },
            {
               "lang" : "ar",
               "types" : [
                  "label"
               ],
               "value" : "مجلس تنمية البحوث الإجتماعية في أفريقي"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
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
         "established" : 1987,
         "external_ids" : [
            {
               "all" : [
                  "grid.463266.3"
               ],
               "preferred" : "grid.463266.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2289 3215"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/00hcr7c15",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.coraf.org/en/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "SN",
                  "country_name" : "Senegal",
                  "country_subdivision_code" : "DK",
                  "country_subdivision_name" : "Dakar",
                  "lat" : 14.6937,
                  "lng" : -17.44406,
                  "name" : "Dakar"
               },
               "geonames_id" : 2253354
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CORAF/WECARD"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Conseil ouest et centre africain pour la recherche et le développement agricoles"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "West and Central African Council for Agricultural Research and Development"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "other"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 7,
            "id" : "af",
            "title" : "Africa"
         }
      ],
      "countries" : [
         {
            "count" : 6,
            "id" : "sn",
            "title" : "Senegal"
         },
         {
            "count" : 1,
            "id" : "dj",
            "title" : "Djibouti"
         }
      ],
      "statuses" : [
         {
            "count" : 7,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 3,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 2,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 1,
            "id" : "facility",
            "title" : "facility"
         },
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
   "number_of_results" : 7,
   "time_taken" : 3
}

```

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query=Heidelberg&filter=types:company,country.country_code:DE' | json_pp
```

Returns a list of active companies in Germany with the keyword "Heidelberg" in a `names` field.

```json
{
   "items" : [
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
         "established" : 1997,
         "external_ids" : [
            {
               "all" : [
                  "grid.512010.7"
               ],
               "preferred" : "grid.512010.7",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/036sabf92",
         "links" : [
            {
               "type" : "website",
               "value" : "https://heidelberg-pharma.com/de/"
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
                  "lat" : 49.47307,
                  "lng" : 8.60896,
                  "name" : "Ladenburg"
               },
               "geonames_id" : 2881980
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Heidelberg Pharma (Germany)"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "WILEX Biotechnology"
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
         "established" : 1990,
         "external_ids" : [
            {
               "all" : [
                  "501100010313"
               ],
               "preferred" : "501100010313",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.509383.4"
               ],
               "preferred" : "grid.509383.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0611 0788"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1453394"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03thsxs59",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.heidelbergengineering.com/"
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
                  "lat" : 49.40768,
                  "lng" : 8.69079,
                  "name" : "Heidelberg"
               },
               "geonames_id" : 2907911
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Heidelberg Engineering (Germany)"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "company",
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
         "established" : 1850,
         "external_ids" : [
            {
               "all" : [
                  "grid.471219.8"
               ],
               "preferred" : "grid.471219.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 8835 7733"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q697106"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03erscp63",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.heidelberg.com/global/en/index.jsp"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Heidelberger_Druckmaschinen"
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
                  "lat" : 49.40768,
                  "lng" : 8.69079,
                  "name" : "Heidelberg"
               },
               "geonames_id" : 2907911
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Heidelberg Printing Machines AG"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Heidelberger Druckmaschinen (Germany)"
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
            "srh.de"
         ],
         "established" : 1966,
         "external_ids" : [
            {
               "all" : [
                  "grid.466450.5"
               ],
               "preferred" : "grid.466450.5",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/02q38zd09",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.srh.de"
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
                  "lat" : 49.40768,
                  "lng" : 8.69079,
                  "name" : "Heidelberg"
               },
               "geonames_id" : 2907911
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Stiftung Rehabilitation Heidelberg (Germany)"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "company"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 4,
            "id" : "eu",
            "title" : "Europe"
         }
      ],
      "countries" : [
         {
            "count" : 4,
            "id" : "de",
            "title" : "Germany"
         }
      ],
      "statuses" : [
         {
            "count" : 4,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 4,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 1,
            "id" : "funder",
            "title" : "funder"
         }
      ]
   },
   "number_of_results" : 4,
   "time_taken" : 6
}
```

<br />
