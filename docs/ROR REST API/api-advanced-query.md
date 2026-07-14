---
title: Advanced query parameter
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR API Advanced query parameter
  description: Instructions for using the advanced query parameter of the ROR API.
  robots: index
---
# About the advanced query parameter

The advanced query parameter allows thorough and precise searching of any and all ROR record fields. Complex queries using field names, wildcards, and Boolean operators can be constructed using [Elasticsearch query string syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax). The advanced query approach is recommended for the following purposes:

- Analysis of the ROR registry to answer research questions
- Searching for records with very specific characteristics or combinations of characteristics

While the [Query parameter](doc:api-query) is designed to help users find organization names quickly with a lightweight keyword search, the advanced query parameter is designed to help users construct more complex and powerful searches. The query parameter searches only the `names` and `external_ids` fields in a ROR record, whereas the advanced query parameter allows for searching of **all fields and sub-fields** in ROR records, including location fields, web address fields, relationship fields, and more.

See [All ROR fields and sub-fields](doc:fields) for a complete alphabetical list of the fields and sub-fields that can be searched with the advanced query parameter. All advanced queries support [Filtering](doc:api-filtering) and [Paging.](doc:api-paging)

# Formatting searches

All request strings must be [URL-encoded](https://www.w3schools.com/tags/ref_urlencode.asp).

## Special characters

Some organization names contain characters like &, (), : and /, which have special meaning in URI syntax, Elasticsearch syntax or both. To avoid error responses or bad results:

- Be sure to [URL-encode](https://www.w3schools.com/tags/ref_urlencode.asp) all advanced query parameter values.
- Escape any [Elasticsearch reserved characters](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_reserved_characters) in the organization name with a URL-encoded backslash \ character. Reserved characters include `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`
- When searching for URLs and ROR IDs as values, make sure that colons and slashes are both escaped and URL-encoded. For example, `https://ror.org/` is `https%5C%3A%5C%2F%5C%2Fror.org%5C%2F`.

## Spaces and quotation marks

[Elasticsearch query string syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax) will treat words separated by a space as separate parts of an advanced query. It is therefore advisable to surround multi-word search terms of the ROR API with URL-encoded quotation marks.

## Wildcards, Boolean operators, and date ranges

Consult the [Elasticsearch query string syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax) documentation for any additional help not provided by the examples below on how to search within single fields and use wildcards, Boolean operators, and date ranges with the advanced query parameter of the ROR REST API.

## Case sensitivity

Currently, the advanced query parameter is case-sensitive, meaning that the same search may return different results depending on the letter case of the input string. For best results, capitalize location names (_Panama_ not _panama_) and be aware of uppercase and lowercase differences in other field data. We [may change this behavior in future](https://github.com/ror-community/ror-roadmap/issues/175) to support case insensitivity for all searches.

# Search by date

In version 2 of the ROR API, you can search by created and last modified dates using an [Elasticsearch range query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_ranges). Note that the reserved characters `[ ] { }` must be used in range queries. This is the use case they are reserved for! These characters may need to be URL-encoded or escaped depending on the language/application you are using.

<Callout icon="📘" theme="info">
  ## Advanced query parameter format, search by date

  `https://api.ror.org/v2/organizations?query.advanced=admin.[created or last modified].date:{dates}`
</Callout>

## Example

Search for all records of any [status](https://ror.readme.io/v2/docs/ror-data-structure#status)  modified between two dates.

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=admin.last_modified.date:%5B2024-12-27%20TO%202025-01-27%5D&all_status' | json_pp
```

The response is a list of all records -- including active, inactive, and withdrawn records -- modified between 2024-12-27 and 2025-01-27.

```
{
   "items" : [
      {
         "admin" : {
            "created" : {
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/00nnxmp87",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.pref.kagawa.lg.jp/sangi/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "37",
                  "country_subdivision_name" : "Kagawa",
                  "lat" : 34.33333,
                  "lng" : 134.05,
                  "name" : "Takamatsu"
               },
               "geonames_id" : 1851100
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Kagawa Prefectural Industrial Technology Center"
            },
            {
               "lang" : "ja",
               "types" : [
                  "alias"
               ],
               "value" : "Kagawaken Sangyo Gijutsu Senta"
            },
            {
               "lang" : "ja",
               "types" : [
                  "alias"
               ],
               "value" : "カガワケン サンギョウ ギジュツ センター"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "香川県産業技術センター"
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
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "spf.org"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/02st0pe74",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.spf.org"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "13",
                  "country_subdivision_name" : "Tokyo",
                  "lat" : 35.6895,
                  "lng" : 139.69171,
                  "name" : "Tokyo"
               },
               "geonames_id" : 1850147
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Sasakawa Peace Foundation"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "The Sasakawa Peace Foundation"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "公益財団法人笹川平和財団"
            },
            {
               "lang" : "ja",
               "types" : [
                  "alias"
               ],
               "value" : "笹川平和財団"
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
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "j-milk.jp"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/05814ha56",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.j-milk.jp"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "13",
                  "country_subdivision_name" : "Tokyo",
                  "lat" : 35.6895,
                  "lng" : 139.69171,
                  "name" : "Tokyo"
               },
               "geonames_id" : 1850147
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Japan Dairy Association"
            },
            {
               "lang" : "ja",
               "types" : [
                  "alias"
               ],
               "value" : "Jミルク"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "一般社団法人Jミルク"
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
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "chukyo-eye.or.jp"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/02ekfha90",
         "links" : [
            {
               "type" : "website",
               "value" : "https://chukyo-eye.or.jp"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "23",
                  "country_subdivision_name" : "Aichi",
                  "lat" : 35.18147,
                  "lng" : 136.90641,
                  "name" : "Nagoya"
               },
               "geonames_id" : 1856057
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Chukyo Eye Clinic"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "医療法人いさな会中京眼科"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/02kb86z46",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.city.tajimi.lg.jp/ishoken/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "21",
                  "country_subdivision_name" : "Gifu",
                  "lat" : 35.31667,
                  "lng" : 137.13333,
                  "name" : "Tajimi"
               },
               "geonames_id" : 1851193
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Tajimi City Pottery Design and Technical Center"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "多治見市陶磁器意匠研究所"
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
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "icljp.landslides.org"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/04zee8776",
         "links" : [
            {
               "type" : "website",
               "value" : "https://icljp.landslides.org"
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
                  "label",
                  "ror_display"
               ],
               "value" : "International Consortium on Landslides"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "特定非営利活動法人国際斜斜面災害研究機構"
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
               "date" : "2020-03-15",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2020,
         "external_ids" : [
            {
               "all" : [
                  "grid.507676.5"
               ],
               "preferred" : "grid.507676.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q74452784"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/043htjv09",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.u-cergy.fr/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/CY_Cergy_Paris_University"
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
                  "lat" : 49.03894,
                  "lng" : 2.07805,
                  "name" : "Cergy-Pontoise"
               },
               "geonames_id" : 8555643
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "CY Cergy Paris University"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "CY Cergy Paris Université"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03qgt2624",
               "label" : "Analyse, Géométrie et Modélisation",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/053p8kg34",
               "label" : "Laboratoire de Didactique André Revuz",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01qm1tk92",
               "label" : "Centre de Recherches Sociologiques sur le Droit et les Institutions Pénales",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0592ata02",
               "label" : "Equipes Traitement de l'Information et Systèmes",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00wwxk695",
               "label" : "Laboratoire Analyse et Modélisation pour la Biologie et l'Environnement",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01g5pq328",
               "label" : "Laboratoire d’Etudes du Rayonnement et de la Matière en Astrophysique et Atmosphères",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03vam5b06",
               "label" : "Laboratoire des systèmes et applications des technologies de l'information et de l'énergie",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00vctdx48",
               "label" : "Théorie Économique, Modélisation et Applications",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00mgd1d10",
               "label" : "Laboratoire Paragraphe",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05nhcdk38",
               "label" : "Laboratoire de Physique Théorique et Modélisation",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02k8f5n40",
               "label" : "Laboratoire Cognitions Humaine et Artificielle",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05mzs1s34",
               "label" : "Centre de recherche multidisciplinaire en sciences humaines et sociales",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0347ywk92",
               "label" : "Bien-être, Organisations, Numérique, Habitabilité, Éducation, Universalité, Relations, Savoirs - BONHEURS",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/053t9n429",
               "label" : "Biomolécules : Conception, Isolement, Synthèse",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/044yw8e10",
               "label" : "Centre de Philosophie Juridique et Politique",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01nrtdp55",
               "label" : "GDR NBODY : Problème quantique à N corps en chimie et physique",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "education"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "hospital.kisarazu.chiba.jp"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/0104w5s79",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.hospital.kisarazu.chiba.jp"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "12",
                  "country_subdivision_name" : "Chiba",
                  "lat" : 35.38329,
                  "lng" : 139.93254,
                  "name" : "Kisarazu"
               },
               "geonames_id" : 1859393
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Kimitsu Chuo Hospital"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "国保直営総合病院君津中央病院"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2018-11-14",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1951,
         "external_ids" : [
            {
               "all" : [
                  "grid.444566.1"
               ],
               "preferred" : "grid.444566.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0375 3788"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7081869"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04t9wmk29",
         "links" : [
            {
               "type" : "website",
               "value" : "https://owc.ac.jp/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Okayama_Gakuin_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "33",
                  "country_subdivision_name" : "Okayama",
                  "lat" : 34.58333,
                  "lng" : 133.76667,
                  "name" : "Kurashiki"
               },
               "geonames_id" : 1858311
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Okayama Gakuin University"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "岡山学院大学"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/022nkr288",
               "label" : "Okayama College",
               "type" : "child"
            }
         ],
         "status" : "active",
         "types" : [
            "education"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "nagachu.jp"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/03pgfk695",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.nagachu.jp"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "15",
                  "country_subdivision_name" : "Niigata",
                  "lat" : 37.45,
                  "lng" : 138.85,
                  "name" : "Nagaoka"
               },
               "geonames_id" : 1856199
            }
         ],
         "names" : [
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "JA新潟厚生連長岡中央綜合病院"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Nagaoka Chuo General hospital"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "pref.miyagi.jp"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/016nnmm16",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.pref.miyagi.jp"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "04",
                  "country_subdivision_name" : "Miyagi",
                  "lat" : 38.26667,
                  "lng" : 140.86667,
                  "name" : "Sendai"
               },
               "geonames_id" : 2111149
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Miyagi Prefectural Government"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "宮城県庁"
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
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "kuji-hp.com"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/04z4ygh26",
         "links" : [
            {
               "type" : "website",
               "value" : "https://kuji-hp.com"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "03",
                  "country_subdivision_name" : "Iwate",
                  "lat" : 40.18778,
                  "lng" : 141.76889,
                  "name" : "Kuji"
               },
               "geonames_id" : 2129427
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Iwate Prefectural Kuji Hospital"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "岩手県立久慈病院"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2019-11-07",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "landwirtschaft-mv.de"
         ],
         "established" : 1992,
         "external_ids" : [
            {
               "all" : [
                  "grid.506447.4"
               ],
               "preferred" : "grid.506447.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0426 9162"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q114659275"
               ],
               "preferred" : "Q114659275",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00s21eg45",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.landwirtschaft-mv.de"
            },
            {
               "type" : "wikipedia",
               "value" : "https://de.wikipedia.org/wiki/Landesforschungsanstalt_f%C3%BCr_Landwirtschaft_und_Fischerei_Mecklenburg-Vorpommern"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "SH",
                  "country_subdivision_name" : "Schleswig-Holstein",
                  "lat" : 53.45,
                  "lng" : 10.5,
                  "name" : "Gülzow"
               },
               "geonames_id" : 2913783
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LFA"
            },
            {
               "lang" : "de",
               "types" : [
                  "acronym"
               ],
               "value" : "LFA MV"
            },
            {
               "lang" : "de",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Landesforschungsanstalt für Landwirtschaft und Fischerei"
            },
            {
               "lang" : "de",
               "types" : [
                  "alias"
               ],
               "value" : "Landesforschungsanstalt für Landwirtschaft und Fischerei Mecklenburg-Vorpommern"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Mecklenburg-Vorpommern Research Centre for Agriculture and Fisheries"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Research Centre for Agriculture and Fisheries"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "State Institute for Agriculture and Fishing Research Mecklenburg Vorpommern"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "State Research Institute for Agriculture and Fisheries"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/013k5zx16",
               "label" : "Ministerium für Klimaschutz, Landwirtschaft, ländliche Räume und Umwelt des Landes Mecklenburg-Vorpommern",
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
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "pref.ishikawa.lg.jp"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/05eya0167",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.pref.ishikawa.lg.jp"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "17",
                  "country_subdivision_name" : "Ishikawa",
                  "lat" : 36.6,
                  "lng" : 136.61667,
                  "name" : "Kanazawa"
               },
               "geonames_id" : 1860243
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Ishikawa Prefectural Government"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "石川県庁"
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
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "jnmcc.or.jp"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/01stv0b46",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.jnmcc.or.jp"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "13",
                  "country_subdivision_name" : "Tokyo",
                  "lat" : 35.6895,
                  "lng" : 139.69171,
                  "name" : "Tokyo"
               },
               "geonames_id" : 1850147
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Nuclear Material Control Center"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "公益財団法人核物質管理センター"
            },
            {
               "lang" : "ja",
               "types" : [
                  "alias"
               ],
               "value" : "核物質管理センター"
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
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "jusendo.or.jp"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/05wg82p98",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.jusendo.or.jp"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "07",
                  "country_subdivision_name" : "Fukushima",
                  "lat" : 37.4,
                  "lng" : 140.38333,
                  "name" : "Kōriyama"
               },
               "geonames_id" : 2112141
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Jusendo Hospital"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "公益財団法人湯浅報恩会寿泉堂綜合病院"
            },
            {
               "lang" : "ja",
               "types" : [
                  "alias"
               ],
               "value" : "湯浅報恩会寿泉堂綜合病院"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "fgh.fujimoto.com"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/00d57gd44",
         "links" : [
            {
               "type" : "website",
               "value" : "https://fgh.fujimoto.com"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "45",
                  "country_subdivision_name" : "Miyazaki",
                  "lat" : 31.73333,
                  "lng" : 131.06667,
                  "name" : "Miyakonojō"
               },
               "geonames_id" : 1856775
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Fujimoto General Hospital"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "一般社団法人藤元メディカルシステム藤元総合病院"
            },
            {
               "lang" : "ja",
               "types" : [
                  "alias"
               ],
               "value" : "藤元メディカルシステム藤元総合病院"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "gero-hp.jp"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/04xkn8h91",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.gero-hp.jp"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "21",
                  "country_subdivision_name" : "Gifu",
                  "lat" : 35.8,
                  "lng" : 137.23333,
                  "name" : "Gero"
               },
               "geonames_id" : 1848055
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Gifu Prefectural Gero Hospital"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "地方独立行政法人岐阜県立下呂温泉病院"
            },
            {
               "lang" : "ja",
               "types" : [
                  "alias"
               ],
               "value" : "岐阜県立下呂温泉病院"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "kaseikyohp.jp"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/00gq2av75",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.kaseikyohp.jp"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "46",
                  "country_subdivision_name" : "Kagoshima",
                  "lat" : 31.56667,
                  "lng" : 130.55,
                  "name" : "Kagoshima"
               },
               "geonames_id" : 1860827
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Kagoshima Seikyo Hospital"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "鹿児島医療生活協同組合鹿児島生協病院"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2025-01-12",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-01-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "tvma.or.jp"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/041cyqf41",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.tvma.or.jp"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "JP",
                  "country_name" : "Japan",
                  "country_subdivision_code" : "13",
                  "country_subdivision_name" : "Tokyo",
                  "lat" : 35.6895,
                  "lng" : 139.69171,
                  "name" : "Tokyo"
               },
               "geonames_id" : 1850147
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Tokyo Veterinary Medical Association"
            },
            {
               "lang" : "ja",
               "types" : [
                  "label"
               ],
               "value" : "公益社団法人東京都獣医師会"
            },
            {
               "lang" : "ja",
               "types" : [
                  "alias"
               ],
               "value" : "東京都獣医師会"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "nonprofit"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 1284,
            "id" : "as",
            "title" : "Asia"
         },
         {
            "count" : 184,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 53,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 15,
            "id" : "af",
            "title" : "Africa"
         },
         {
            "count" : 9,
            "id" : "sa",
            "title" : "South America"
         },
         {
            "count" : 5,
            "id" : "oc",
            "title" : "Oceania"
         }
      ],
      "countries" : [
         {
            "count" : 1241,
            "id" : "jp",
            "title" : "Japan"
         },
         {
            "count" : 45,
            "id" : "fr",
            "title" : "France"
         },
         {
            "count" : 40,
            "id" : "it",
            "title" : "Italy"
         },
         {
            "count" : 26,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 24,
            "id" : "ca",
            "title" : "Canada"
         },
         {
            "count" : 19,
            "id" : "de",
            "title" : "Germany"
         },
         {
            "count" : 13,
            "id" : "ch",
            "title" : "Switzerland"
         },
         {
            "count" : 11,
            "id" : "pt",
            "title" : "Portugal"
         },
         {
            "count" : 9,
            "id" : "gb",
            "title" : "United Kingdom"
         },
         {
            "count" : 8,
            "id" : "cn",
            "title" : "China"
         }
      ],
      "statuses" : [
         {
            "count" : 1522,
            "id" : "active",
            "title" : "active"
         },
         {
            "count" : 26,
            "id" : "inactive",
            "title" : "inactive"
         },
         {
            "count" : 1,
            "id" : "withdrawn",
            "title" : "withdrawn"
         }
      ],
      "types" : [
         {
            "count" : 439,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 406,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 339,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 258,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 95,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 78,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 14,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 13,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 9,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 1549,
   "time_taken" : 2
}
```

## Example

Search for active records created before or after a specific date by using a wildcard in the range query.

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=admin.created.date:%7B%2A%20TO%202019-12-31%7D' | json_pp
```

The response is a list of active records created before 2019-12-31.

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
         "established" : 1887,
         "external_ids" : [
            {
               "all" : [
                  "501100001780",
                  "100008690",
                  "100010552"
               ],
               "preferred" : "501100001780",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1017.7"
               ],
               "preferred" : "grid.1017.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2163 3550"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1057890"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04ttjf776",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.rmit.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/RMIT_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "RMIT"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "RMIT University"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Royal Melbourne Institute of Technology University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/039p7nx39",
               "label" : "ARC Centre of Excellence for Automated Decision-Making and Society",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03m3ca021",
               "label" : "RMIT Europe",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/004axh929",
               "label" : "RMIT Vietnam",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/010mv7n52",
               "label" : "Austin Hospital",
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
         "established" : 1964,
         "external_ids" : [
            {
               "all" : [
                  "501100001215"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1018.8"
               ],
               "preferred" : "grid.1018.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2342 0938"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1478723"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01rxfrp27",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.latrobe.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/La_Trobe_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "La Trobe University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/010mv7n52",
               "label" : "Austin Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/0484pjq71",
               "label" : "Box Hill Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03grnna41",
               "label" : "Royal Women's Hospital",
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
         "established" : 1967,
         "external_ids" : [
            {
               "all" : [
                  "501100001790"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1023.0"
               ],
               "preferred" : "grid.1023.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2193 0854"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1053985"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/023q4bk22",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.cqu.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Central_Queensland_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "QLD",
                  "country_subdivision_name" : "Queensland",
                  "lat" : -23.38032,
                  "lng" : 150.50595,
                  "name" : "Rockhampton"
               },
               "geonames_id" : 2151437
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CQU"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "CQUniversity"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Central Queensland University"
            }
         ],
         "relationships" : [],
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
         "established" : 1987,
         "external_ids" : [
            {
               "all" : [
                  "501100001789"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1033.1"
               ],
               "preferred" : "grid.1033.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0405 3820"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q892188"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/006jxzx88",
         "links" : [
            {
               "type" : "website",
               "value" : "http://bond.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bond_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "QLD",
                  "country_subdivision_name" : "Queensland",
                  "lat" : -28.00029,
                  "lng" : 153.43088,
                  "name" : "Gold Coast"
               },
               "geonames_id" : 2165087
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bond University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05eq01d13",
               "label" : "Gold Coast Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/0257s2812",
               "label" : "Robina Hospital",
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
         "established" : 1989,
         "external_ids" : [
            {
               "all" : [
                  "501100001769"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1037.5"
               ],
               "preferred" : "grid.1037.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0368 0777"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1066188"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00wfvh315",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.csu.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Charles_Sturt_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "NSW",
                  "country_subdivision_name" : "New South Wales",
                  "lat" : -33.41665,
                  "lng" : 149.5806,
                  "name" : "Bathurst"
               },
               "geonames_id" : 2176632
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CSU"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Charles Sturt University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05newpx76",
               "label" : "Wagga Wagga Base Hospital",
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
         "established" : 1986,
         "external_ids" : [
            {
               "all" : [
                  "100008561"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1056.2"
               ],
               "preferred" : "grid.1056.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2224 8486"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3151717"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05ktbsm52",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.burnet.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Burnet_Institute"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Burnet Institute"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02bfwt286",
               "label" : "Monash University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01ej9dk98",
               "label" : "University of Melbourne",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "funder",
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
         "established" : 1998,
         "external_ids" : [
            {
               "all" : [
                  "grid.1064.3"
               ],
               "preferred" : "grid.1064.3",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/00nx6aa03",
         "links" : [
            {
               "type" : "website",
               "value" : "http://research.mater.org.au/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "QLD",
                  "country_subdivision_name" : "Queensland",
                  "lat" : -27.46794,
                  "lng" : 153.02809,
                  "name" : "Brisbane"
               },
               "geonames_id" : 2174003
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Mater Research"
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
         "established" : 1951,
         "external_ids" : [
            {
               "all" : [
                  "grid.1073.5"
               ],
               "preferred" : "grid.1073.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0626 201X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7592065"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02k3cxs74",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.svi.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/St._Vincent's_Institute_of_Medical_Research"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.79839,
                  "lng" : 144.97833,
                  "name" : "Fitzroy"
               },
               "geonames_id" : 2166584
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "SVI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "St Vincents Institute of Medical Research"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/001kjn539",
               "label" : "St Vincent's Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01ej9dk98",
               "label" : "University of Melbourne",
               "type" : "related"
            }
         ],
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
         "established" : 1989,
         "external_ids" : [
            {
               "all" : [
                  "501100001208",
                  "100009021"
               ],
               "preferred" : "501100001208",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1076.0"
               ],
               "preferred" : "grid.1076.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0626 1885"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q17006364"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/046fa4y88",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.hri.org.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Heart_Research_Institute"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "NSW",
                  "country_subdivision_name" : "New South Wales",
                  "lat" : -33.89835,
                  "lng" : 151.17754,
                  "name" : "Newtown"
               },
               "geonames_id" : 2155386
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HRI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "The Heart Research Institute"
            }
         ],
         "relationships" : [],
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
         "established" : 1989,
         "external_ids" : [
            {
               "all" : [
                  "grid.1088.1"
               ],
               "preferred" : "grid.1088.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0622 6844"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q27982338"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02d439m40",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.epipsi.gr/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "GR",
                  "country_name" : "Greece",
                  "country_subdivision_code" : "I",
                  "country_subdivision_name" : "Attica",
                  "lat" : 37.98376,
                  "lng" : 23.72784,
                  "name" : "Athens"
               },
               "geonames_id" : 264371
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UMHRI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University Mental Health Research Institute"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education"
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
         "established" : 1904,
         "external_ids" : [
            {
               "all" : [
                  "501100000767"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1121.3"
               ],
               "preferred" : "grid.1121.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0396 1069"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q243278"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04h08p482",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.rolls-royce.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Rolls-Royce_Holdings"
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
                  "lat" : 52.92277,
                  "lng" : -1.47663,
                  "name" : "Derby"
               },
               "geonames_id" : 2651347
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Rolls-Royce (United Kingdom)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01x3p5p96",
               "label" : "Rolls-Royce (Canada)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05jeza980",
               "label" : "Rolls-Royce (Germany)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/018a0bk66",
               "label" : "Rolls-Royce (Norway)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/055ef6019",
               "label" : "Rolls-Royce (Sweden)",
               "type" : "child"
            }
         ],
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
         "established" : 1909,
         "external_ids" : [
            {
               "all" : [
                  "501100000775"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1236.6"
               ],
               "preferred" : "grid.1236.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0790 9434"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q152057"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01zctcs90",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.bp.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/BP"
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
               "value" : "BP (United Kingdom)"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "BP Amoco"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "The British Petroleum Company"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/051177492",
               "label" : "BP (Canada)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03f9m1k43",
               "label" : "BP (France)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/048q66y67",
               "label" : "BP (Germany)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05dfvhh79",
               "label" : "BP (Spain)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/051659894",
               "label" : "BP (United States)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0503ft796",
               "label" : "Castrol (United Kingdom)",
               "type" : "child"
            }
         ],
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
         "established" : 1873,
         "external_ids" : [
            {
               "all" : [
                  "grid.1281.a"
               ],
               "preferred" : "grid.1281.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q821293"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05m7zw681",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.riotinto.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Rio_Tinto_Group"
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
               "value" : "Rio Tinto (United Kingdom)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03z28c516",
               "label" : "Rio Tinto (Australia)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00mv4xe50",
               "label" : "Rio Tinto (Canada)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01gykgk63",
               "label" : "Rio Tinto (France)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01z3j2f47",
               "label" : "Rio Tinto (Switzerland)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02vhqqh86",
               "label" : "Rio Tinto (United States)",
               "type" : "child"
            }
         ],
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
         "domains" : [],
         "established" : 1946,
         "external_ids" : [
            {
               "all" : [
                  "grid.1403.6"
               ],
               "preferred" : "grid.1403.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0462 5657"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/03awtex73",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.arup.com/Global_locations/USA.aspx"
            },
            {
               "type" : "wikipedia",
               "value" : "https://www.arup.com/offices/united-states-of-america"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.3751,
                  "lng" : -71.10561,
                  "name" : "Cambridge"
               },
               "geonames_id" : 4931972
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "Arup"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Arup Group (United States)"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "Ove N. Arup Consulting Engineers"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/057d3rj91",
               "label" : "Arup Group (United Kingdom)",
               "type" : "parent"
            }
         ],
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
         "domains" : [],
         "established" : 1980,
         "external_ids" : [
            {
               "all" : [
                  "501100000577",
                  "501100000842"
               ],
               "preferred" : "501100000577",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1453.3"
               ],
               "preferred" : "grid.1453.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 1091 7144"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q593786"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00kv9pj15",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.btplc.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/BT_Group"
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
               "value" : "BT Group (United Kingdom)"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "British Telecommunications"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01xzpd518",
               "label" : "BT Archives",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03308db07",
               "label" : "BT Research",
               "type" : "child"
            }
         ],
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
         "established" : 1906,
         "external_ids" : [
            {
               "all" : [
                  "grid.1491.d"
               ],
               "preferred" : "grid.1491.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0642 1746"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q6786513"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03mjtdk61",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.mater.org.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Mater_Health_Services"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "QLD",
                  "country_subdivision_name" : "Queensland",
                  "lat" : -27.46794,
                  "lng" : 153.02809,
                  "name" : "Brisbane"
               },
               "geonames_id" : 2174003
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Mater Health Services"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05wqhv079",
               "label" : "Mater Adult Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04mxrtj31",
               "label" : "Mater Children's Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01k4cfw02",
               "label" : "Mater Misericordiae Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03j4rdg62",
               "label" : "Mater Mothers' Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03w94w157",
               "label" : "Mater Private Hospital",
               "type" : "child"
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
               "date" : "2018-11-14",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1826,
         "external_ids" : [
            {
               "all" : [
                  "grid.1504.0"
               ],
               "preferred" : "grid.1504.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q733487"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04yyp8h20",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.pilkington.com/en-GB/uk"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Pilkington"
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
                  "lat" : 53.45,
                  "lng" : -2.73333,
                  "name" : "St Helens"
               },
               "geonames_id" : 2638785
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Pilkington (United Kingdom)"
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
         "domains" : [],
         "established" : 1976,
         "external_ids" : [
            {
               "all" : [
                  "grid.1543.3"
               ],
               "preferred" : "grid.1543.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0522 6113"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/022rkxt86",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.trojantechnologies.com/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "CA",
                  "country_name" : "Canada",
                  "country_subdivision_code" : "ON",
                  "country_subdivision_name" : "Ontario",
                  "lat" : 42.98339,
                  "lng" : -81.23304,
                  "name" : "London"
               },
               "geonames_id" : 6058560
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Trojan Technologies (Canada)"
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
         "domains" : [],
         "established" : 1871,
         "external_ids" : [
            {
               "all" : [
                  "grid.1623.6"
               ],
               "preferred" : "grid.1623.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0432 511X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7713086"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01wddqe20",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.alfredhealth.org.au/the-alfred"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/The_Alfred_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "The Alfred"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "The Alfred Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04scfb908",
               "label" : "Alfred Health",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02bfwt286",
               "label" : "Monash University",
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
               "date" : "2018-11-14",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1989,
         "external_ids" : [
            {
               "all" : [
                  "grid.1694.a"
               ],
               "preferred" : "grid.1694.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q8031120"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03kwrfk72",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.wch.sa.gov.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Women's_and_Children's_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "SA",
                  "country_subdivision_name" : "South Australia",
                  "lat" : -34.92866,
                  "lng" : 138.59863,
                  "name" : "Adelaide"
               },
               "geonames_id" : 2078025
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "WCH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Women's and Children's Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01kpzv902",
               "label" : "Flinders University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00892tw58",
               "label" : "University of Adelaide",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01p93h210",
               "label" : "University of South Australia",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 38028,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 33563,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 17580,
            "id" : "as",
            "title" : "Asia"
         },
         {
            "count" : 2986,
            "id" : "af",
            "title" : "Africa"
         },
         {
            "count" : 2902,
            "id" : "sa",
            "title" : "South America"
         },
         {
            "count" : 1569,
            "id" : "oc",
            "title" : "Oceania"
         }
      ],
      "countries" : [
         {
            "count" : 29290,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 7076,
            "id" : "gb",
            "title" : "United Kingdom"
         },
         {
            "count" : 4640,
            "id" : "de",
            "title" : "Germany"
         },
         {
            "count" : 3876,
            "id" : "cn",
            "title" : "China"
         },
         {
            "count" : 3630,
            "id" : "jp",
            "title" : "Japan"
         },
         {
            "count" : 3474,
            "id" : "fr",
            "title" : "France"
         },
         {
            "count" : 3153,
            "id" : "ca",
            "title" : "Canada"
         },
         {
            "count" : 2798,
            "id" : "in",
            "title" : "India"
         },
         {
            "count" : 2727,
            "id" : "cz",
            "title" : "Czech Republic"
         },
         {
            "count" : 1982,
            "id" : "ru",
            "title" : "Russia"
         }
      ],
      "statuses" : [
         {
            "count" : 96628,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 27470,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 19163,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 15213,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 13224,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 12327,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 8179,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 7861,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 5715,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 2703,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 96628,
   "time_taken" : 22
}
```

# Search all fields

The advanced query parameter allows you to perform a keyword search of all ROR record fields and sub-fields.

<Callout icon="📘" theme="info">
  ## Advanced query parameter format, all fields

  `https://api.ror.org/v2/organizations?query.advanced=[value]`
</Callout>

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=%22Harvard%20University%22' | json_pp
```

The response returns all instances of the exact phrase "Harvard University" in all fields of all records. Note that the phrase is often found in the `relationships` field of ROR records for organizations that are related to Harvard University.

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
            "harvard.edu"
         ],
         "established" : 1636,
         "external_ids" : [
            {
               "all" : [
                  "100007229",
                  "100010520",
                  "100009802",
                  "100009868",
                  "100008548",
                  "100008549",
                  "100005724",
                  "100005578",
                  "100009116",
                  "100008036",
                  "100009345",
                  "100005668",
                  "100010952",
                  "100008024",
                  "100005487",
                  "100005473",
                  "100005293",
                  "100006075",
                  "100005915",
                  "100005469",
                  "100005650",
                  "100005678",
                  "100005692",
                  "100005802",
                  "100005856",
                  "100005893",
                  "100005941",
                  "100006007",
                  "100006011",
                  "100006274",
                  "100007230",
                  "100007300",
                  "100007887",
                  "100008263",
                  "100005574",
                  "100007299",
                  "100006691",
                  "100007301",
                  "100019552"
               ],
               "preferred" : "100007229",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.38142.3c"
               ],
               "preferred" : "grid.38142.3c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1936 754X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q13371",
                  "Q5676556"
               ],
               "preferred" : "Q13371",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03vek6s52",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.harvard.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Harvard_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.3751,
                  "lng" : -71.10561,
                  "name" : "Cambridge"
               },
               "geonames_id" : 4931972
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard University"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Universidad de Harvard"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/032q5ym94",
               "label" : "Athinoula A. Martinos Center for Biomedical Imaging",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03hj6c016",
               "label" : "Berenson Allen Center for Noninvasive Brain Stimulation",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03c3r2d17",
               "label" : "Center for Astrophysics Harvard & Smithsonian",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05r3dyn47",
               "label" : "Center for Systems Biology",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04wsv7966",
               "label" : "Center for Vascular Biology Research",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/004y4rj95",
               "label" : "Gordon Center for Medical Imaging",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04kj1hn59",
               "label" : "Harvard Stem Cell Institute",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/006v7bf86",
               "label" : "Harvard University Press",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/053tmcn30",
               "label" : "MIT-Harvard Center for Ultracold Atoms",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/053r20n13",
               "label" : "Ragon Institute of MGH, MIT and Harvard",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04r5ess67",
               "label" : "Sleep and Human Health Institute",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04pvzz946",
               "label" : "The NSF AI Institute for Artificial Intelligence and Fundamental Interactions",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05xsxgs79",
               "label" : "Arnold Arboretum",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/059cpzx98",
               "label" : "Harvard Forest Long Term Ecological Research",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/008cfmj78",
               "label" : "Wyss Institute for Biologically Inspired Engineering",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03pvyf116",
               "label" : "Dana-Farber/Harvard Cancer Center",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05b0g2v72",
               "label" : "Real Colegio Complutense",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04drvxt59",
               "label" : "Beth Israel Deaconess Medical Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00dvg7y05",
               "label" : "Boston Children's Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05xckek43",
               "label" : "Boston IVF",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/04rkbns44",
               "label" : "Botswana Harvard AIDS Institute Partnership",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03w44ff23",
               "label" : "Brigham and Women's Faulkner Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/04b6nzv94",
               "label" : "Brigham and Women's Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05a0ya142",
               "label" : "Broad Institute",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/059c3mv67",
               "label" : "Cambridge Health Alliance",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/02jzgtq86",
               "label" : "Dana-Farber Cancer Institute",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01zxdeg39",
               "label" : "Harvard Pilgrim Health Care",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00jjeh629",
               "label" : "Harvard–MIT Division of Health Sciences and Technology",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/02vptss42",
               "label" : "Hebrew SeniorLife",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/044hpwe09",
               "label" : "IIT@Harvard",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/0280a3n32",
               "label" : "Joslin Diabetes Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05tby3y60",
               "label" : "Judge Baker Children's Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03mbq3y29",
               "label" : "Lahey Hospital and Medical Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/04g3dn724",
               "label" : "Massachusetts Eye and Ear Infirmary",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/002pd6e78",
               "label" : "Massachusetts General Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05par7p11",
               "label" : "Massachusetts Green High Performance Computing Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01kta7d96",
               "label" : "McLean Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03hrxmf69",
               "label" : "Newton Wellesley Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/023pf5e38",
               "label" : "Somerville Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/011dvr318",
               "label" : "Spaulding Rehabilitation Hospital",
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
         "established" : 1913,
         "external_ids" : [
            {
               "all" : [
                  "grid.446714.4"
               ],
               "preferred" : "grid.446714.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0694 1061"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1587900"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/006v7bf86",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.hup.harvard.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Harvard_University_Press"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.3751,
                  "lng" : -71.10561,
                  "name" : "Cambridge"
               },
               "geonames_id" : 4931972
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HUP"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard University Press"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "other"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2024-03-13",
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
                  "100013721"
               ],
               "preferred" : "100013721",
               "type" : "fundref"
            },
            {
               "all" : [
                  "Q7300869"
               ],
               "preferred" : "Q7300869",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05b0g2v72",
         "links" : [
            {
               "type" : "website",
               "value" : "https://rcc.harvard.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Real_Colegio_Complutense"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.3751,
                  "lng" : -71.10561,
                  "name" : "Cambridge"
               },
               "geonames_id" : 4931972
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "RCC at Harvard University"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "RCCHU"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Real Colegio Complutense"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Real Colegio Complutense at Harvard University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "parent"
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
               "date" : "2022-03-17",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1872,
         "external_ids" : [
            {
               "all" : [
                  "100009403"
               ],
               "preferred" : "100009403",
               "type" : "fundref"
            },
            {
               "all" : [
                  "0000 0001 2293 3096"
               ],
               "preferred" : "0000 0001 2293 3096",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q568666"
               ],
               "preferred" : "Q568666",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05xsxgs79",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.arboretum.harvard.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Arnold_Arboretum"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.35843,
                  "lng" : -71.05977,
                  "name" : "Boston"
               },
               "geonames_id" : 4930956
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Arnold Arboretum"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Arnold Arboretum of Harvard University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "archive",
            "funder"
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
         "established" : 2004,
         "external_ids" : [
            {
               "all" : [
                  "100008035"
               ],
               "preferred" : "100008035",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.511171.2"
               ],
               "preferred" : "grid.511171.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q40771227"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04kj1hn59",
         "links" : [
            {
               "type" : "website",
               "value" : "https://hsci.harvard.edu/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.3751,
                  "lng" : -71.10561,
                  "name" : "Cambridge"
               },
               "geonames_id" : 4931972
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HSCI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard Stem Cell Institute"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
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
               "date" : "2020-12-21",
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
                  "grid.497274.b"
               ],
               "preferred" : "grid.497274.b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0627 5136"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/02vptss42",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.hebrewseniorlife.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.35843,
                  "lng" : -71.05977,
                  "name" : "Boston"
               },
               "geonames_id" : 4930956
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HSL"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Hebrew SeniorLife"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "related"
            }
         ],
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.416039.9"
               ],
               "preferred" : "grid.416039.9",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04r5ess67",
         "links" : [
            {
               "type" : "website",
               "value" : "https://sleep.med.harvard.edu/ext/tmanual/Institutions/Sleep_Human_Health.htm"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NM",
                  "country_subdivision_name" : "New Mexico",
                  "lat" : 35.08449,
                  "lng" : -106.65114,
                  "name" : "Albuquerque"
               },
               "geonames_id" : 5454711
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Sleep and Human Health Institute"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "parent"
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
               "date" : "2018-11-14",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1980,
         "external_ids" : [
            {
               "all" : [
                  "grid.67104.34"
               ],
               "preferred" : "grid.67104.34",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0415 0102"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5676515"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01zxdeg39",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.harvardpilgrim.org/portal/page?_pageid=1391,1&_dad=portal&_schema=PORTAL"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Harvard_Pilgrim_Health_Care"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.35843,
                  "lng" : -71.05977,
                  "name" : "Boston"
               },
               "geonames_id" : 4930956
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard Pilgrim Health Care"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
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
               "date" : "2018-11-14",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1917,
         "external_ids" : [
            {
               "all" : [
                  "100006690",
                  "100006689"
               ],
               "preferred" : "100006690",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.280934.4"
               ],
               "preferred" : "grid.280934.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0320 631X"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/05tby3y60",
         "links" : [
            {
               "type" : "website",
               "value" : "http://jbcc.harvard.edu/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.35843,
                  "lng" : -71.05977,
                  "name" : "Boston"
               },
               "geonames_id" : 4930956
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Judge Baker Children's Center"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
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
               "date" : "2020-12-21",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1952,
         "external_ids" : [
            {
               "all" : [
                  "100007877",
                  "100007878"
               ],
               "preferred" : "100007877",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.16694.3c"
               ],
               "preferred" : "grid.16694.3c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2183 9479"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1708743"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0280a3n32",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.joslin.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Joslin_Diabetes_Center"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.35843,
                  "lng" : -71.05977,
                  "name" : "Boston"
               },
               "geonames_id" : 4930956
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Joslin Diabetes Center"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
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
         "domains" : [
            "bhp.org.bw"
         ],
         "established" : 1996,
         "external_ids" : [
            {
               "all" : [
                  "grid.462829.3"
               ],
               "preferred" : "grid.462829.3",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04rkbns44",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.bhp.org.bw"
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
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BHP"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Botswana Harvard AIDS Institute Partnership"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "related"
            }
         ],
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
         "established" : 1900,
         "external_ids" : [
            {
               "all" : [
                  "grid.415122.1"
               ],
               "preferred" : "grid.415122.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0378 8518"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5438132"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03w44ff23",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.brighamandwomensfaulkner.org/default.aspx#.V3tN99J97IV"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Faulkner_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.35843,
                  "lng" : -71.05977,
                  "name" : "Boston"
               },
               "geonames_id" : 4930956
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BWFH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Brigham and Women's Faulkner Hospital"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Faulkner Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05wvpxv85",
               "label" : "Tufts University",
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
               "date" : "2018-11-14",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1881,
         "external_ids" : [
            {
               "all" : [
                  "grid.416176.3"
               ],
               "preferred" : "grid.416176.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9957 1751"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q14715774"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03hrxmf69",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.nwh.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Newton-Wellesley_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.33704,
                  "lng" : -71.20922,
                  "name" : "Newton"
               },
               "geonames_id" : 4945283
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "NWH"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Newton Cottage Hospital"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Newton Wellesley Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05wvpxv85",
               "label" : "Tufts University",
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
               "date" : "2021-09-23",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2015,
         "external_ids" : [
            {
               "all" : [
                  "grid.512020.4"
               ],
               "preferred" : "grid.512020.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30625556"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/004y4rj95",
         "links" : [
            {
               "type" : "website",
               "value" : "https://gordon.mgh.harvard.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Gordon_Center_for_Medical_Imaging"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.35843,
                  "lng" : -71.05977,
                  "name" : "Boston"
               },
               "geonames_id" : 4930956
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Gordon Center for Medical Imaging"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/002pd6e78",
               "label" : "Massachusetts General Hospital",
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
         "established" : 1999,
         "external_ids" : [
            {
               "all" : [
                  "grid.483004.b"
               ],
               "preferred" : "grid.483004.b",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/05r3dyn47",
         "links" : [
            {
               "type" : "website",
               "value" : "https://csb.mgh.harvard.edu/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.35843,
                  "lng" : -71.05977,
                  "name" : "Boston"
               },
               "geonames_id" : 4930956
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CSB"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Center for Systems Biology"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/002pd6e78",
               "label" : "Massachusetts General Hospital",
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
         "established" : 1891,
         "external_ids" : [
            {
               "all" : [
                  "grid.461485.a"
               ],
               "preferred" : "grid.461485.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0101 1658"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7559973"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/023pf5e38",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.challiance.org/Locations/SomervilleHospitalcampus.aspx"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Somerville_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.3876,
                  "lng" : -71.0995,
                  "name" : "Somerville"
               },
               "geonames_id" : 4951257
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Somerville Hospital"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/059c3mv67",
               "label" : "Cambridge Health Alliance",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
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
               "date" : "2020-12-21",
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
                  "100019611"
               ],
               "preferred" : "100019611",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.509519.1"
               ],
               "preferred" : "grid.509519.1",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/053tmcn30",
         "links" : [
            {
               "type" : "website",
               "value" : "http://cuaweb.mit.edu/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.3751,
                  "lng" : -71.10561,
                  "name" : "Cambridge"
               },
               "geonames_id" : 4931972
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CUA"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "MIT-Harvard Center for Ultracold Atoms"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/042nb2s44",
               "label" : "Massachusetts Institute of Technology",
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
               "date" : "2021-04-06",
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
                  "grid.509953.3"
               ],
               "preferred" : "grid.509953.3",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/044hpwe09",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.iit.it/research/lines/iit-harvard"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.3751,
                  "lng" : -71.10561,
                  "name" : "Cambridge"
               },
               "geonames_id" : 4931972
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "IIT@Harvard"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/042t93s57",
               "label" : "Italian Institute of Technology",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
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
         "established" : 1970,
         "external_ids" : [
            {
               "all" : [
                  "grid.413735.7"
               ],
               "preferred" : "grid.413735.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0475 2760"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5676651"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00jjeh629",
         "links" : [
            {
               "type" : "website",
               "value" : "http://hst.mit.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Harvard%E2%80%93MIT_Division_of_Health_Sciences_and_Technology"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.3751,
                  "lng" : -71.10561,
                  "name" : "Cambridge"
               },
               "geonames_id" : 4931972
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HST"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Harvard–MIT Division of Health Sciences and Technology"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/042nb2s44",
               "label" : "Massachusetts Institute of Technology",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "education"
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
         "established" : 1923,
         "external_ids" : [
            {
               "all" : [
                  "grid.415731.5"
               ],
               "preferred" : "grid.415731.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0725 1353"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q6473221"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03mbq3y29",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.lahey.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Lahey_Hospital_%26_Medical_Center"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 42.50482,
                  "lng" : -71.19561,
                  "name" : "Burlington"
               },
               "geonames_id" : 4931737
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Lahey Clinic"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Lahey Hospital and Medical Center"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05qwgg493",
               "label" : "Boston University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03vek6s52",
               "label" : "Harvard University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05wvpxv85",
               "label" : "Tufts University",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "healthcare"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 40,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 1,
            "id" : "af",
            "title" : "Africa"
         }
      ],
      "countries" : [
         {
            "count" : 40,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 1,
            "id" : "bw",
            "title" : "Botswana"
         }
      ],
      "statuses" : [
         {
            "count" : 41,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 20,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 17,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 14,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 5,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 2,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 2,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 1,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 41,
   "time_taken" : 21
}
```

<Callout icon="🚧" theme="warn">
  ## Consider refining your advanced query searches

  Searching all fields with the advanced query may produce more results than desired, slow the speed of the search, and affect the performance of the ROR API. We recommend searching one or more specific fields instead, as in the examples below.
</Callout>

# Search a single field

[Elastic Search field name syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_field_names) `fieldname:query` can be used to search within a single field. When using this syntax, `fieldname` must exactly match a field name in the list of [All ROR fields and sub-fields](doc:fields), e.g.,  `locations.geonames_details.name:Melbourne` not `locations:Melbourne`.

<Callout icon="📘" theme="info">
  ## Advanced query parameter format, single field

  `https://api.ror.org/v2/organizations?query.advanced=[fieldname]:[value]`
</Callout>

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=links.value:%22vcrlter.virginia.edu%22' | json_pp
```

The response is a single result with the value `vcrlter.virginia.edu` in the `links` field.

```json
{
   "items" : [
      {
         "admin" : {
            "created" : {
               "date" : "2022-06-16",
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
                  "Q7934197"
               ],
               "preferred" : "Q7934197",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/013zder45",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.vcrlter.virginia.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Virginia_Coast_Reserve_Long-Term_Ecological_Research"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "VA",
                  "country_subdivision_name" : "Virginia",
                  "lat" : 37.28653,
                  "lng" : -75.92243,
                  "name" : "Oyster"
               },
               "geonames_id" : 4777710
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "VCR"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "VCR LTER"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Virginia Coast Reserve LTER"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Virginia Coast Reserve Long Term Ecological Research"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Virginia Coastal LTER"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/039kwqk96",
               "label" : "Long Term Ecological Research Network",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/0153tk833",
               "label" : "University of Virginia",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/0330j0z60",
               "label" : "Environmental Data Initiative",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03hsf0573",
               "label" : "William & Mary",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05qwgg493",
               "label" : "Boston University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00y4zzh67",
               "label" : "George Washington University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/02smfhw86",
               "label" : "Virginia Tech",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/02nkdxk79",
               "label" : "Virginia Commonwealth University",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/0130frc33",
               "label" : "University of North Carolina at Chapel Hill",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/0563w1497",
               "label" : "The Nature Conservancy",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "facility"
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
            "id" : "facility",
            "title" : "facility"
         }
      ]
   },
   "number_of_results" : 1,
   "time_taken" : 1
}
```

## Example

Search for multiple terms in a single field and form complex queries with [Elasticsearch Boolean operator syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_boolean_operators) (AND, OR, NOT). Note that the Boolean operator must be surrounded by URL-encoded spaces and that multiple query terms must be surrounded by parentheses in accordance with [Elasticsearch Boolean operator syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_boolean_operators).

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=id:(https%5C%3A%5C%2F%5C%2Fror.org%5C%2F02bfwt286+OR+https%5C%3A%5C%2F%5C%2Fror.org%5C%2F00rqy9422+OR+https%5C%3A%5C%2F%5C%2Fror.org%5C%2F01sf06y89+OR+https%5C%3A%5C%2F%5C%2Fror.org%5C%2F045s9b323)' | json_pp
```

Alternately, use the wildcard `*` (which can be URL-encoded at `%2A`) in place of the ROR ID protocol and domain.

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=id:(*02bfwt286+OR+*00rqy9422+OR+*01sf06y89+OR+*045s9b323)' | json_pp
```

The response is a list of 4 records, each of which has one of the 4 specified ROR IDs in the `id` field.

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
         "established" : 1980,
         "external_ids" : [
            {
               "all" : [
                  "grid.4347.4"
               ],
               "preferred" : "grid.4347.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 1939 4239"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/045s9b323",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.eng.it/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "IT",
                  "country_name" : "Italy",
                  "country_subdivision_code" : "62",
                  "country_subdivision_name" : "Lazio",
                  "lat" : 41.89193,
                  "lng" : 12.51133,
                  "name" : "Rome"
               },
               "geonames_id" : 3169070
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Engineering (Italy)"
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
         "domains" : [],
         "established" : 1909,
         "external_ids" : [
            {
               "all" : [
                  "501100001794",
                  "501100008407",
                  "501100005268"
               ],
               "preferred" : "501100001794",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1003.2"
               ],
               "preferred" : "grid.1003.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9320 7537"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q866012"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00rqy9422",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.uq.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/University_of_Queensland"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "QLD",
                  "country_subdivision_name" : "Queensland",
                  "lat" : -27.46794,
                  "lng" : 153.02809,
                  "name" : "Brisbane"
               },
               "geonames_id" : 2174003
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UQ"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Queensland"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/053mfxd72",
               "label" : "ARC Centre of Excellence for Children and Families over the Life Course",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03rtykw43",
               "label" : "ARC Centre of Excellence for Engineered Quantum Systems",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01k8nkc73",
               "label" : "ARC Centre of Excellence for Environmental Decisions",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04dxv8v14",
               "label" : "ARC Centre of Excellence for Innovations in Peptide and Protein Science",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/058xdtn86",
               "label" : "ARC Centre of Excellence for Plant Success in Nature and Agriculture",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00rnbty21",
               "label" : "Centre for Quantum Computation and Communication Technology",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00eaqry86",
               "label" : "Centre for Microscopy and Microanalysis",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/010g47133",
               "label" : "Ipswich Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/0290qyp66",
               "label" : "Ochsner Medical Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/04mqb0968",
               "label" : "Princess Alexandra Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05p52kj31",
               "label" : "Royal Brisbane and Women's Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03wxseg04",
               "label" : "Terrestrial Ecosystem Research Network",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01s62z461",
               "label" : "Australian Coral Reef Society",
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
         "established" : 1958,
         "external_ids" : [
            {
               "all" : [
                  "501100001779",
                  "501100001144",
                  "501100007917",
                  "501100000991",
                  "501100001198",
                  "501100006532"
               ],
               "preferred" : "501100001779",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1002.3"
               ],
               "preferred" : "grid.1002.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1936 7857"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q598841"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02bfwt286",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.monash.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Monash_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Monash University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02yasb727",
               "label" : "ARC Centre of Excellence for Integrative Brain Function",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/051e7s436",
               "label" : "ARC Centre of Excellence in Advanced Molecular Imaging",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02qa5kg76",
               "label" : "Australian Regenerative Medicine Institute",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02r3nf527",
               "label" : "IITB-Monash Research Academy",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/048t93218",
               "label" : "National Trauma Research Institute",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0555x2b41",
               "label" : "ANZCA Clinical Trials Network",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/058xdtn86",
               "label" : "ARC Centre of Excellence for Plant Success in Nature and Agriculture",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00jgk9c55",
               "label" : "Monash University European Research Foundation ETS",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0484pjq71",
               "label" : "Box Hill Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05ktbsm52",
               "label" : "Burnet Institute",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/051b68e86",
               "label" : "Frankston Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00aeqjw83",
               "label" : "Jessie McPherson Private Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/036s9kg65",
               "label" : "Monash Medical Centre",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01wddqe20",
               "label" : "The Alfred Hospital",
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
         "domains" : [
            "mq.edu.au"
         ],
         "established" : 1964,
         "external_ids" : [
            {
               "all" : [
                  "501100001230"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1004.5"
               ],
               "preferred" : "grid.1004.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2158 5405"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q741082"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01sf06y89",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.mq.edu.au"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Macquarie_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "NSW",
                  "country_subdivision_name" : "New South Wales",
                  "lat" : -33.86785,
                  "lng" : 151.20732,
                  "name" : "Sydney"
               },
               "geonames_id" : 2147714
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Macquarie University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03nk9pp38",
               "label" : "ARC Centre of Excellence for Core to Crust Fluid Systems",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/044b7p696",
               "label" : "ARC Centre of Excellence in Cognition and its Disorders",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01p2zg436",
               "label" : "ARC Centre of Excellence in Synthetic Biology",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04hy0x592",
               "label" : "Woolcock Institute of Medical Research",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0402tt118",
               "label" : "Sydney Hospital",
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
            "count" : 3,
            "id" : "oc",
            "title" : "Oceania"
         },
         {
            "count" : 1,
            "id" : "eu",
            "title" : "Europe"
         }
      ],
      "countries" : [
         {
            "count" : 3,
            "id" : "au",
            "title" : "Australia"
         },
         {
            "count" : 1,
            "id" : "it",
            "title" : "Italy"
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
            "count" : 3,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 3,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 1,
            "id" : "company",
            "title" : "company"
         }
      ]
   },
   "number_of_results" : 4,
   "time_taken" : 12
}
```

# Search multiple fields

Search multiple fields and form complex queries with [Elasticsearch Boolean operator syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_boolean_operators) (AND, OR, NOT). Note that the Boolean operator must be surrounded by URL-encoded spaces.

<Callout icon="📘" theme="info">
  ## Advanced query parameter format, multiple fields

  `https://api.ror.org/v2/organizations?query.advanced=[fieldname]:[value]+[Boolean operator]+[fieldname]:[value]`
</Callout>

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=names.value:Cornell+AND+locations.geonames_details.name:Ithaca' | json_pp
```

The response is a list of records with the keyword "Cornell" in a `names.value` field **and** the city "Ithaca" in the `locations.geonames_details.name` field. Remember that the `names` field can contain multiple values with different name types.

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
            "cornell.edu"
         ],
         "established" : 1865,
         "external_ids" : [
            {
               "all" : [
                  "100007231",
                  "100007604",
                  "100006480",
                  "100006077",
                  "100006471",
                  "100006923",
                  "100007272",
                  "100008586",
                  "100007232",
                  "100009083",
                  "100008674",
                  "100008585"
               ],
               "preferred" : "100007231",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.5386.8"
               ],
               "preferred" : "grid.5386.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1936 877X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q49115"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05bnh6r87",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.cornell.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Cornell_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 42.44063,
                  "lng" : -76.49661,
                  "name" : "Ithaca"
               },
               "geonames_id" : 5122432
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CU"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cornell University"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Universidad Cornell"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/031m8s392",
               "label" : "New York Sea Grant",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03ycy6g75",
               "label" : "New York Space Grant Consortium",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00j52pq61",
               "label" : "New York State College of Agriculture & Life Sciences",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04r17kf39",
               "label" : "New York State College of Veterinary Medicine",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00zg6dt46",
               "label" : "New York State School of Industrial and Labor Relations",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/047g2xp14",
               "label" : "New York State University College of Human Ecology",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02r109517",
               "label" : "Weill Cornell Medicine",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00k86w020",
               "label" : "Cornell Lab of Ornithology",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03h0qhk21",
               "label" : "Cornell Atkinson Center for Sustainability",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04pw1zg89",
               "label" : "PARADIM",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/012qsrr25",
               "label" : "Cornell University Agricultural Experiment Station",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00m2zh467",
               "label" : "arXiv",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02sp1z620",
               "label" : "Burke Medical Research Institute",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/04dnsc767",
               "label" : "Burke Rehabilitation Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/027zt9171",
               "label" : "Houston Methodist",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/035a72598",
               "label" : "Lincoln Medical Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01skxn174",
               "label" : "Lower Manhattan Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/02yrq0923",
               "label" : "Memorial Sloan Kettering Cancer Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/016m8pd54",
               "label" : "Morgan Stanley Children's Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03f91xw18",
               "label" : "Museum of the Earth",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01j17xg39",
               "label" : "New York Hospital Queens",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/04929s478",
               "label" : "NewYork–Presbyterian Brooklyn Methodist Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03gzbrs57",
               "label" : "NewYork–Presbyterian Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05dvpaj72",
               "label" : "The Rogosin Institute",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05v5hg569",
               "label" : "Weill Cornell Medical College in Qatar",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00yzb1d91",
               "label" : "Wyckoff Heights Medical Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00mkh7345",
               "label" : "Hubbard Brook Long Term Ecological Research",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05cshtm26",
               "label" : "Great Lakes Research Consortium",
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
               "date" : "2024-03-13",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1880,
         "external_ids" : [
            {
               "all" : [
                  "100011622"
               ],
               "preferred" : "100011622",
               "type" : "fundref"
            },
            {
               "all" : [
                  "0000 0001 2170 7652"
               ],
               "preferred" : "0000 0001 2170 7652",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q45134801"
               ],
               "preferred" : "Q45134801",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/012qsrr25",
         "links" : [
            {
               "type" : "website",
               "value" : "https://cals.cornell.edu/agricultural-experiment-station"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/New_York_State_Agricultural_Experiment_Station"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 42.44063,
                  "lng" : -76.49661,
                  "name" : "Ithaca"
               },
               "geonames_id" : 5122432
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CUAES"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Cornell AES"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Cornell AgriTech"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cornell University Agricultural Experiment Station"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05bnh6r87",
               "label" : "Cornell University",
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
               "date" : "2023-07-06",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1915,
         "external_ids" : [
            {
               "all" : [
                  "100017754"
               ],
               "preferred" : "100017754",
               "type" : "fundref"
            },
            {
               "all" : [
                  "0000 0004 1219 4439"
               ],
               "preferred" : "0000 0004 1219 4439",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q2997535"
               ],
               "preferred" : "Q2997535",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00k86w020",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.birds.cornell.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Cornell_Lab_of_Ornithology"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 42.44063,
                  "lng" : -76.49661,
                  "name" : "Ithaca"
               },
               "geonames_id" : 5122432
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CLO"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cornell Lab of Ornithology"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "The Cornell Lab of Ornithology"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05bnh6r87",
               "label" : "Cornell University",
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
               "date" : "2023-09-14",
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
         "id" : "https://ror.org/032bgnz47",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.chess.cornell.edu"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 42.44063,
                  "lng" : -76.49661,
                  "name" : "Ithaca"
               },
               "geonames_id" : 5122432
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CHESS"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "CHESS - Cornell High Energy Synchrotron Source"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cornell High Energy Synchrotron Source"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Cornell High Energy Synchrotron Source (CHESS)"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2020-07-06",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1925,
         "external_ids" : [
            {
               "all" : [
                  "grid.507858.7"
               ],
               "preferred" : "grid.507858.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q5171563"
               ],
               "preferred" : "Q5171563",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/047g2xp14",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.human.cornell.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Cornell_University_College_of_Human_Ecology"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 42.44063,
                  "lng" : -76.49661,
                  "name" : "Ithaca"
               },
               "geonames_id" : 5122432
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Cornell Human Ecology"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Cornell University College of Human Ecology"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HumEc"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "New York State University College of Human Ecology"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05bnh6r87",
               "label" : "Cornell University",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/01q1z8k08",
               "label" : "State University of New York",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "education"
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
         "established" : 2010,
         "external_ids" : [
            {
               "all" : [
                  "100018190",
                  "100013044"
               ],
               "preferred" : "100018190",
               "type" : "fundref"
            },
            {
               "all" : [
                  "Q4815910"
               ],
               "preferred" : "Q4815910",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03h0qhk21",
         "links" : [
            {
               "type" : "website",
               "value" : "https://atkinson.cornell.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Atkinson_Center_for_a_Sustainable_Future"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 42.44063,
                  "lng" : -76.49661,
                  "name" : "Ithaca"
               },
               "geonames_id" : 5122432
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Atkinson Center for a Sustainable Future"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cornell Atkinson Center for Sustainability"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05bnh6r87",
               "label" : "Cornell University",
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
               "date" : "2020-07-06",
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
                  "grid.507859.6"
               ],
               "preferred" : "grid.507859.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0609 3519"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q50809363"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04r17kf39",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.vet.cornell.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Cornell_University_College_of_Veterinary_Medicine"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 42.44063,
                  "lng" : -76.49661,
                  "name" : "Ithaca"
               },
               "geonames_id" : 5122432
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Cornell University College of Veterinary Medicine"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "New York State College of Veterinary Medicine"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05bnh6r87",
               "label" : "Cornell University",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/01q1z8k08",
               "label" : "State University of New York",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "education"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2020-07-06",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1874,
         "external_ids" : [
            {
               "all" : [
                  "grid.507860.c"
               ],
               "preferred" : "grid.507860.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0614 0717"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5171560"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00j52pq61",
         "links" : [
            {
               "type" : "website",
               "value" : "https://cals.cornell.edu/#"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Cornell_University_College_of_Agriculture_and_Life_Sciences"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 42.44063,
                  "lng" : -76.49661,
                  "name" : "Ithaca"
               },
               "geonames_id" : 5122432
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CALS"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Cornell University College of Agriculture and Life Sciences"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "New York State College of Agriculture & Life Sciences"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05bnh6r87",
               "label" : "Cornell University",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/01q1z8k08",
               "label" : "State University of New York",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "education"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2020-07-06",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1945,
         "external_ids" : [
            {
               "all" : [
                  "grid.507863.f"
               ],
               "preferred" : "grid.507863.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2188 4528"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5171574"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00zg6dt46",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ilr.cornell.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Cornell_University_School_of_Industrial_and_Labor_Relations"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 42.44063,
                  "lng" : -76.49661,
                  "name" : "Ithaca"
               },
               "geonames_id" : 5122432
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Cornell University School of Industrial and Labor Relations"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ILR"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "New York State School of Industrial and Labor Relations"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05bnh6r87",
               "label" : "Cornell University",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/01q1z8k08",
               "label" : "State University of New York",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "education"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 9,
            "id" : "na",
            "title" : "North America"
         }
      ],
      "countries" : [
         {
            "count" : 9,
            "id" : "us",
            "title" : "United States"
         }
      ],
      "statuses" : [
         {
            "count" : 9,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 6,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 4,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 3,
            "id" : "facility",
            "title" : "facility"
         }
      ]
   },
   "number_of_results" : 9,
   "time_taken" : 5
}
```

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=names.value:Cornell+NOT+locations.geonames_details.name:Ithaca' | json_pp
```

The response is a list of records with the keyword "Cornell" in a `names.value` field **without** the city "Ithaca" in the `locations.geonames_details.name` field. Remember that the `names` field can contain multiple different values with different name types.

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
            "cornellcollege.edu"
         ],
         "established" : 1853,
         "external_ids" : [
            {
               "all" : [
                  "100013072"
               ],
               "preferred" : "100013072",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.254690.c"
               ],
               "preferred" : "grid.254690.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0436 344X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5171517"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01wvxpc32",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.cornellcollege.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Cornell_College"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "IA",
                  "country_subdivision_name" : "Iowa",
                  "lat" : 41.92195,
                  "lng" : -91.41684,
                  "name" : "Mount Vernon"
               },
               "geonames_id" : 4868251
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cornell College"
            }
         ],
         "relationships" : [],
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
         "established" : 1989,
         "external_ids" : [
            {
               "all" : [
                  "100007273",
                  "100020424"
               ],
               "preferred" : "100020424",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.471410.7"
               ],
               "preferred" : "grid.471410.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2179 7643"
               ],
               "preferred" : "0000 0001 2179 7643",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3567094"
               ],
               "preferred" : "Q3567094",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02r109517",
         "links" : [
            {
               "type" : "website",
               "value" : "https://weill.cornell.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Weill_Cornell_Medicine"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 40.71427,
                  "lng" : -74.00597,
                  "name" : "New York"
               },
               "geonames_id" : 5128581
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "WCM"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Weill Cornell Medicine"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05bnh6r87",
               "label" : "Cornell University",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03zjqec80",
               "label" : "Hospital for Special Surgery",
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
         "established" : 1968,
         "external_ids" : [
            {
               "all" : [
                  "grid.434882.1"
               ],
               "preferred" : "grid.434882.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0481 8993"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30256999"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04cx6n327",
         "links" : [
            {
               "type" : "website",
               "value" : "https://cornellscott.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CT",
                  "country_subdivision_name" : "Connecticut",
                  "lat" : 41.30815,
                  "lng" : -72.92816,
                  "name" : "New Haven"
               },
               "geonames_id" : 4839366
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CSHHC"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cornell Scott-Hill Health Center"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
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
         "established" : 1914,
         "external_ids" : [
            {
               "all" : [
                  "grid.486913.1"
               ],
               "preferred" : "grid.486913.1",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/05snsev90",
         "links" : [
            {
               "type" : "website",
               "value" : "http://sullivancce.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 41.8012,
                  "lng" : -74.74655,
                  "name" : "Liberty"
               },
               "geonames_id" : 5124323
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CCESC"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cornell Cooperative Extension Sullivan County"
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
         "established" : 1917,
         "external_ids" : [
            {
               "all" : [
                  "grid.448516.a"
               ],
               "preferred" : "grid.448516.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2222 496X"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/0581e0971",
         "links" : [
            {
               "type" : "website",
               "value" : "http://ccesuffolk.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 40.91704,
                  "lng" : -72.66204,
                  "name" : "Riverhead"
               },
               "geonames_id" : 5133926
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CCE"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cornell Cooperative Extension of Suffolk County"
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.486908.b"
               ],
               "preferred" : "grid.486908.b",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/055hhfr85",
         "links" : [
            {
               "type" : "website",
               "value" : "http://ccejefferson.org"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 43.97478,
                  "lng" : -75.91076,
                  "name" : "Watertown"
               },
               "geonames_id" : 5143396
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CCE"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cornell Cooperative Extension Association of Jefferson County"
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
         "established" : 2001,
         "external_ids" : [
            {
               "all" : [
                  "100019460"
               ],
               "preferred" : "100019460",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.416973.e"
               ],
               "preferred" : "grid.416973.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0582 4340"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q4118834"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05v5hg569",
         "links" : [
            {
               "type" : "website",
               "value" : "http://qatar-weill.cornell.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Weill_Cornell_Medical_College_in_Qatar"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "QA",
                  "country_name" : "Qatar",
                  "country_subdivision_code" : "DA",
                  "country_subdivision_name" : "Baladīyat ad Dawḩah",
                  "lat" : 25.28545,
                  "lng" : 51.53096,
                  "name" : "Doha"
               },
               "geonames_id" : 290030
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "WCMC-Q"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Weill Cornell Medical College in Qatar"
            },
            {
               "lang" : "ar",
               "types" : [
                  "label"
               ],
               "value" : "كلية طب وايل كورنيل"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05bnh6r87",
               "label" : "Cornell University",
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
            "count" : 6,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 1,
            "id" : "as",
            "title" : "Asia"
         }
      ],
      "countries" : [
         {
            "count" : 6,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 1,
            "id" : "qa",
            "title" : "Qatar"
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
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 3,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 3,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 1,
            "id" : "healthcare",
            "title" : "healthcare"
         }
      ]
   },
   "number_of_results" : 7,
   "time_taken" : 3
}
```

# Search all sub-fields of a parent field

[Elasticsearch field name syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_field_names) can be used to search all sub-fields of a parent field.

<Callout icon="📘" theme="info">
  ## Advanced query parameter format, sub-fields of a parent field

  `https://api.ror.org/v2/organizations?query.advanced=[parent fieldname]%5c*:[value]`

  Note that \ characters must be URL-encoded.
</Callout>

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=locations.%5c*:Melbourne' | json_pp
```

The response is a list of records in which the term "Melbourne" appears in one of the sub-fields of the `locations` field.

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
         "established" : 1958,
         "external_ids" : [
            {
               "all" : [
                  "501100001779",
                  "501100001144",
                  "501100007917",
                  "501100000991",
                  "501100001198",
                  "501100006532"
               ],
               "preferred" : "501100001779",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1002.3"
               ],
               "preferred" : "grid.1002.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1936 7857"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q598841"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02bfwt286",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.monash.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Monash_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Monash University"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02yasb727",
               "label" : "ARC Centre of Excellence for Integrative Brain Function",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/051e7s436",
               "label" : "ARC Centre of Excellence in Advanced Molecular Imaging",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02qa5kg76",
               "label" : "Australian Regenerative Medicine Institute",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02r3nf527",
               "label" : "IITB-Monash Research Academy",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/048t93218",
               "label" : "National Trauma Research Institute",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0555x2b41",
               "label" : "ANZCA Clinical Trials Network",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/058xdtn86",
               "label" : "ARC Centre of Excellence for Plant Success in Nature and Agriculture",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00jgk9c55",
               "label" : "Monash University European Research Foundation ETS",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0484pjq71",
               "label" : "Box Hill Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05ktbsm52",
               "label" : "Burnet Institute",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/051b68e86",
               "label" : "Frankston Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/00aeqjw83",
               "label" : "Jessie McPherson Private Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/036s9kg65",
               "label" : "Monash Medical Centre",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01wddqe20",
               "label" : "The Alfred Hospital",
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
         "established" : 2007,
         "external_ids" : [
            {
               "all" : [
                  "501100001073"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.468607.b"
               ],
               "preferred" : "grid.468607.b",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/052xe5r02",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.fwpa.com.au/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "FWPA"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Forest and Wood Products (Australia)"
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
               "date" : "2023-03-30",
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
                  "100008718"
               ],
               "preferred" : "100008718",
               "type" : "fundref"
            },
            {
               "all" : [
                  "0000 0004 5905 1873"
               ],
               "preferred" : "0000 0004 5905 1873",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/02z8wa502",
         "links" : [],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Judith Jane Mason and Harold Stannett Williams Memorial Foundation"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Mason Foundation"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
            "other"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2022-08-03",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2018,
         "external_ids" : [],
         "id" : "https://ror.org/0282y0p95",
         "links" : [
            {
               "type" : "website",
               "value" : "https://scienceforall.world"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "SFA"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "STARDIT"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "SciFall"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Science for All"
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
         "established" : 1949,
         "external_ids" : [
            {
               "all" : [
                  "501100004224"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1055.1"
               ],
               "preferred" : "grid.1055.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0397 8434"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7175569"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02a8bt934",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.petermac.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Peter_MacCallum_Cancer_Centre"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Peter Mac"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Peter MacCallum Cancer Centre"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Peter MacCallum Cancer Institute"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
            "healthcare"
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
                  "100014555"
               ],
               "preferred" : "100014555",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1058.c"
               ],
               "preferred" : "grid.1058.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9442 535X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q22669992"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/048fyec77",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.mcri.edu.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Murdoch_Children%27s_Research_Institute"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "MCRI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Murdoch Children's Research Institute"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01mmz5j21",
               "label" : "Victorian Clinical Genetics Services",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02rktxt32",
               "label" : "Royal Children's Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01ej9dk98",
               "label" : "University of Melbourne",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "funder",
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
         "established" : 1927,
         "external_ids" : [
            {
               "all" : [
                  "100011371"
               ],
               "preferred" : "100011371",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.419296.1"
               ],
               "preferred" : "grid.419296.1",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0637 6498"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q15995438"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02ef40e75",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.surgeons.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Royal_Australasian_College_of_Surgeons"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Australian College of Surgeons"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "RACS"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Royal Australasian College of Surgeons"
            }
         ],
         "relationships" : [],
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
         "established" : 1970,
         "external_ids" : [
            {
               "all" : [
                  "100012128"
               ],
               "preferred" : "100012128",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.427570.3"
               ],
               "preferred" : "grid.427570.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 6426 4806"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30286368"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04ez6qq41",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.austinmrf.org.au/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "AMRF"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Austin Medical Research Foundation"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
            "healthcare"
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
         "established" : 2015,
         "external_ids" : [
            {
               "all" : [
                  "501100018823"
               ],
               "preferred" : "501100018823",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.511012.6"
               ],
               "preferred" : "grid.511012.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0744 2459"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q80130041"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01mqx8q10",
         "links" : [
            {
               "type" : "website",
               "value" : "https://agriculture.vic.gov.au/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Agriculture Victoria"
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
         "established" : 1939,
         "external_ids" : [
            {
               "all" : [
                  "100013108"
               ],
               "preferred" : "100013108",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.427583.f"
               ],
               "preferred" : "grid.427583.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9508 9589"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q4824008"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03ep9w883",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.aco.org.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Australian_College_of_Optometry"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Australian College of Optometry"
            }
         ],
         "relationships" : [],
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
         "established" : 1958,
         "external_ids" : [
            {
               "all" : [
                  "100013185"
               ],
               "preferred" : "100013185",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.255966.b"
               ],
               "preferred" : "grid.255966.b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2229 7296"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3152650"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04atsbb87",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.fit.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Florida_Institute_of_Technology"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "FL",
                  "country_subdivision_name" : "Florida",
                  "lat" : 28.08363,
                  "lng" : -80.60811,
                  "name" : "Melbourne"
               },
               "geonames_id" : 4163971
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "FIT"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Florida Institute of Technology"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Florida Tech"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Institut Technologique de Floride"
            }
         ],
         "relationships" : [],
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
         "established" : 1916,
         "external_ids" : [
            {
               "all" : [
                  "501100020480"
               ],
               "preferred" : "501100020480",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.1135.6"
               ],
               "preferred" : "grid.1135.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 1512 2287"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q908265"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/044tc0x05",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.csl.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/CSL_Limited"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "CSL (Australia)"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "ZLB Behring"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04nvba109",
               "label" : "CSL (Germany)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/024fpfc76",
               "label" : "CSL (Japan)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05y55b611",
               "label" : "CSL (Russia)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01400tq86",
               "label" : "CSL (Switzerland)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05mrzft53",
               "label" : "CSL (United Kingdom)",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01jxhxv92",
               "label" : "CSL (United States)",
               "type" : "child"
            }
         ],
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
         "established" : 2008,
         "external_ids" : [
            {
               "all" : [
                  "501100015735"
               ],
               "preferred" : "501100015735",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.452643.2"
               ],
               "preferred" : "grid.452643.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q7927236"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05gkzhv48",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.melbournebioinformatics.org.au"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Victorian_Life_Sciences_Computation_Initiative"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Melbourne Bioinformatics"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "VLSCI"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Victorian Life Sciences Computation Initiative"
            }
         ],
         "relationships" : [],
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
                  "501100021276"
               ],
               "preferred" : "501100021276",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.417072.7"
               ],
               "preferred" : "grid.417072.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0645 2884"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/02p4mwa83",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.westernhealth.org.au/Pages/default.aspx"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "WH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Western Health"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0221nva21",
               "label" : "Footscray Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/033abcd54",
               "label" : "Sunshine Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05qv7gx64",
               "label" : "Western Hospital",
               "type" : "child"
            }
         ],
         "status" : "active",
         "types" : [
            "funder",
            "healthcare"
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
                  "100013574"
               ],
               "preferred" : "100013574",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.493443.d"
               ],
               "preferred" : "grid.493443.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9143 3311"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/00dvgej48",
         "links" : [
            {
               "type" : "website",
               "value" : "https://aija.org.au/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "AIJA"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "The Australasian Institute of Judicial Administration"
            }
         ],
         "relationships" : [],
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
         "established" : 1882,
         "external_ids" : [
            {
               "all" : [
                  "501100020211"
               ],
               "preferred" : "501100020211",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.410678.c"
               ],
               "preferred" : "grid.410678.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9374 3516"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q50379732"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05dbj6g52",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.austin.org.au/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Austin Health"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/010mv7n52",
               "label" : "Austin Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04dhg0348",
               "label" : "Heidelberg Repatriation Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03a2tac74",
               "label" : "Florey Institute of Neuroscience and Mental Health",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/01ej9dk98",
               "label" : "University of Melbourne",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "funder",
            "healthcare"
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
         "established" : 1966,
         "external_ids" : [
            {
               "all" : [
                  "501100022580"
               ],
               "preferred" : "501100022580",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.491055.c"
               ],
               "preferred" : "grid.491055.c",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04kdqzg37",
         "links" : [
            {
               "type" : "website",
               "value" : "http://williambucklandfoundation.org.au/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "The William Buckland Foundation"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "WBF"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
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
         "established" : 1946,
         "external_ids" : [
            {
               "all" : [
                  "501100022869"
               ],
               "preferred" : "501100022869",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.464664.2"
               ],
               "preferred" : "grid.464664.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2161 5547"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7373769"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02n0sa445",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ranzcp.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Royal_Australian_and_New_Zealand_College_of_Psychiatrists"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "RANZCP"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Royal Australian and New Zealand College of Psychiatrists"
            }
         ],
         "relationships" : [],
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
         "established" : 1854,
         "external_ids" : [
            {
               "all" : [
                  "501100022973"
               ],
               "preferred" : "501100022973",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.462871.e"
               ],
               "preferred" : "grid.462871.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0432 5689"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1200052"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03tmpzb72",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.slv.vic.gov.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/State_Library_of_Victoria"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "State Library of Victoria"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02xvc5r05",
               "label" : "Government of Victoria",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "archive",
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
         "established" : 2002,
         "external_ids" : [
            {
               "all" : [
                  "grid.482043.9"
               ],
               "preferred" : "grid.482043.9",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 9249 772X"
               ],
               "preferred" : "0000 0004 9249 772X",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q17022254"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/036s3gz19",
         "links" : [
            {
               "type" : "website",
               "value" : "http://apo.org.au/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Policy_Online"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "OC",
                  "continent_name" : "Oceania",
                  "country_code" : "AU",
                  "country_name" : "Australia",
                  "country_subdivision_code" : "VIC",
                  "country_subdivision_name" : "Victoria",
                  "lat" : -37.814,
                  "lng" : 144.96332,
                  "name" : "Melbourne"
               },
               "geonames_id" : 2158177
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "APO"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Analysis & Policy Observatory"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Australian Policy Online"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "archive"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 198,
            "id" : "oc",
            "title" : "Oceania"
         },
         {
            "count" : 26,
            "id" : "na",
            "title" : "North America"
         }
      ],
      "countries" : [
         {
            "count" : 198,
            "id" : "au",
            "title" : "Australia"
         },
         {
            "count" : 26,
            "id" : "us",
            "title" : "United States"
         }
      ],
      "statuses" : [
         {
            "count" : 224,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 96,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 49,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 41,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 30,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 27,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 27,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 22,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 21,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 7,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 224,
   "time_taken" : 3
}
```

# Find records with a non-null value in a field

[Elasticsearch field name syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_field_names) can be used to return records that have any non-null value in a field.

<Callout icon="📘" theme="info">
  ## Advanced query parameter format, non-null values in a field

  `https://api.ror.org/v2/organizations?query.advanced=_exists_:[fieldname]`
</Callout>

<Callout icon="🚧" theme="warn">
  ## Null vs. empty string

  Most ROR record fields that have no value are set to _null_, but `_exists_:[fieldname]` may include results with empty strings for some fields.
</Callout>

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=_exists_:domains' | json_pp
```

The response is a list of ROR records with a value in the `domains` field.

```json
{
   "items" : [
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
         "domains" : [
            "itv.edu.ni"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/04g0har25",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.itv.edu.ni"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "NI",
                  "country_name" : "Nicaragua",
                  "country_subdivision_code" : "MN",
                  "country_subdivision_name" : "Managua Department",
                  "lat" : 12.13282,
                  "lng" : -86.2504,
                  "name" : "Managua"
               },
               "geonames_id" : 3617763
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "acronym"
               ],
               "value" : "ITV"
            },
            {
               "lang" : "es",
               "types" : [
                  "alias"
               ],
               "value" : "Instituto Tecnológico Victoria"
            },
            {
               "lang" : "es",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Instituto Tecnológico Victoria - ITV"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education"
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
         "domains" : [
            "auib.edu.iq"
         ],
         "established" : 2018,
         "external_ids" : [
            {
               "all" : [
                  "0000 0004 8339 2723"
               ],
               "preferred" : "0000 0004 8339 2723",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q107675529"
               ],
               "preferred" : "Q107675529",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/033jerz55",
         "links" : [
            {
               "type" : "website",
               "value" : "https://auib.edu.iq"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/American_University_of_Iraq_-_Baghdad"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "IQ",
                  "country_name" : "Iraq",
                  "country_subdivision_code" : "BG",
                  "country_subdivision_name" : "Baghdad",
                  "lat" : 33.34058,
                  "lng" : 44.40088,
                  "name" : "Baghdad"
               },
               "geonames_id" : 98182
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "acronym"
               ],
               "value" : "AUIB"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "American University of Iraq Baghdad"
            },
            {
               "lang" : "ar",
               "types" : [
                  "label"
               ],
               "value" : "الجامعة الأمريكية في بغداد"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education"
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
            "vsb.cz"
         ],
         "established" : 1849,
         "external_ids" : [
            {
               "all" : [
                  "501100006443",
                  "501100006444"
               ],
               "preferred" : "501100006443",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.440850.d"
               ],
               "preferred" : "grid.440850.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9643 2828"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1247543"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05x8mcb75",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.vsb.cz"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/VSB_–_Technical_University_of_Ostrava"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "CZ",
                  "country_name" : "Czechia",
                  "country_subdivision_code" : "80",
                  "country_subdivision_name" : "Moravskoslezský",
                  "lat" : 49.83465,
                  "lng" : 18.28204,
                  "name" : "Ostrava"
               },
               "geonames_id" : 3068799
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "VSB - Technical University of Ostrava"
            },
            {
               "lang" : "de",
               "types" : [
                  "label"
               ],
               "value" : "VSB - Technische Universität Ostrava"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "VSB - Universidad Técnica de Ostrava"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "VSB - Université Technique d'Ostrava"
            },
            {
               "lang" : "cs",
               "types" : [
                  "label"
               ],
               "value" : "Vysoká škola báňská - Technická univerzita Ostrava"
            },
            {
               "lang" : "cs",
               "types" : [
                  "acronym"
               ],
               "value" : "VŠB"
            },
            {
               "lang" : "cs",
               "types" : [
                  "alias"
               ],
               "value" : "VŠB - TU Ostrava"
            },
            {
               "lang" : "cs",
               "types" : [
                  "alias"
               ],
               "value" : "VŠB - Technická univerzita Ostrava"
            },
            {
               "lang" : "cs",
               "types" : [
                  "acronym"
               ],
               "value" : "VŠB-TUO"
            },
            {
               "lang" : "ru",
               "types" : [
                  "label"
               ],
               "value" : "ВШБ - Tехнический университет Острава"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04vxzbp05",
               "label" : "Czech Welding Institute (Czechia)",
               "type" : "child"
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
               "date" : "2024-04-30",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "glicid.fr"
         ],
         "established" : 2022,
         "external_ids" : [],
         "id" : "https://ror.org/02mhxg157",
         "links" : [
            {
               "type" : "website",
               "value" : "https://glicid.fr"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "PDL",
                  "country_subdivision_name" : "Pays de la Loire",
                  "lat" : 47.21725,
                  "lng" : -1.55336,
                  "name" : "Nantes"
               },
               "geonames_id" : 2990969
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "acronym"
               ],
               "value" : "GLiCID"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Groupement Ligérien pour le Calcul Intensif Distribué"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/03gnr7b55",
               "label" : "Nantes Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/04yrqp957",
               "label" : "Université d'Angers",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/01mtcc283",
               "label" : "Le Mans Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03nh7d505",
               "label" : "École Centrale de Nantes",
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
         "domains" : [
            "ciputra.ac.id"
         ],
         "established" : 1990,
         "external_ids" : [
            {
               "all" : [
                  "grid.444387.8"
               ],
               "preferred" : "grid.444387.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 6812 6160"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q12523310"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01zj4g759",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ciputra.ac.id"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "ID",
                  "country_name" : "Indonesia",
                  "country_subdivision_code" : "JI",
                  "country_subdivision_name" : "East Java",
                  "lat" : -7.24917,
                  "lng" : 112.75083,
                  "name" : "Surabaya"
               },
               "geonames_id" : 1625822
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Ciputra University Surabaya"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UC"
            },
            {
               "lang" : "id",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Universitas Ciputra"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2024-05-27",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "tese.edu.mx"
         ],
         "established" : 1994,
         "external_ids" : [
            {
               "all" : [
                  "Q6140137"
               ],
               "preferred" : "Q6140137",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04mc1gv86",
         "links" : [
            {
               "type" : "website",
               "value" : "https://tese.edu.mx"
            },
            {
               "type" : "wikipedia",
               "value" : "https://es.wikipedia.org/wiki/Tecnol%C3%B3gico_de_Estudios_Superiores_de_Ecatepec"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "MX",
                  "country_name" : "Mexico",
                  "country_subdivision_code" : "MEX",
                  "country_subdivision_name" : "México",
                  "lat" : 19.60492,
                  "lng" : -99.06064,
                  "name" : "Ecatepec"
               },
               "geonames_id" : 3529612
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "alias"
               ],
               "value" : "TES Ecatepec"
            },
            {
               "lang" : "es",
               "types" : [
                  "acronym"
               ],
               "value" : "TESE"
            },
            {
               "lang" : "es",
               "types" : [
                  "alias"
               ],
               "value" : "TecNM Ecatepec"
            },
            {
               "lang" : "es",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Tecnológico de Estudios Superiores de Ecatepec"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education"
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
            "hcahealthcare.com"
         ],
         "established" : 1968,
         "external_ids" : [
            {
               "all" : [
                  "100016646"
               ],
               "preferred" : "100016646",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.414420.7"
               ],
               "preferred" : "grid.414420.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0158 6152"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1630431"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0237c2m81",
         "links" : [
            {
               "type" : "website",
               "value" : "https://hcahealthcare.com"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Hospital_Corporation_of_America"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "TN",
                  "country_subdivision_name" : "Tennessee",
                  "lat" : 36.16589,
                  "lng" : -86.78444,
                  "name" : "Nashville"
               },
               "geonames_id" : 4644585
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HCA"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "HCA Healthcare"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Hospital Corporation of America"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05rsdqb94",
               "label" : "Mercy Hospital",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01w6z0f95",
               "label" : "Menorah Medical Center",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "funder",
            "healthcare"
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
            "buxdu.uz"
         ],
         "established" : 1992,
         "external_ids" : [
            {
               "all" : [
                  "grid.444580.9"
               ],
               "preferred" : "grid.444580.9",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0402 8385"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q20536168"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05k4xnj24",
         "links" : [
            {
               "type" : "website",
               "value" : "https://buxdu.uz"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Bukhara_State_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "UZ",
                  "country_name" : "Uzbekistan",
                  "country_subdivision_code" : "BU",
                  "country_subdivision_name" : "Bukhara",
                  "lat" : 39.77472,
                  "lng" : 64.42861,
                  "name" : "Bukhara"
               },
               "geonames_id" : 1217662
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BSU"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Bukhara State University"
            },
            {
               "lang" : "uz",
               "types" : [
                  "label"
               ],
               "value" : "Buxoro davlat universiteti"
            },
            {
               "lang" : "uz",
               "types" : [
                  "alias"
               ],
               "value" : "Buxoro universiteti"
            },
            {
               "lang" : "ru",
               "types" : [
                  "label"
               ],
               "value" : "Бухарский государственный университет"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2024-05-27",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "szpital.wroc.pl"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/0262j1008",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.szpital.wroc.pl"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "PL",
                  "country_name" : "Poland",
                  "country_subdivision_code" : "02",
                  "country_subdivision_name" : "Lower Silesia",
                  "lat" : 51.1,
                  "lng" : 17.03333,
                  "name" : "Wroclaw"
               },
               "geonames_id" : 3081368
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Provincial Specialist Hospital named after J. Gromkowski"
            },
            {
               "lang" : "pl",
               "types" : [
                  "alias"
               ],
               "value" : "WSS im. J. Gromkowskiego"
            },
            {
               "lang" : "pl",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Wojewódzki Szpital Specjalistyczny im. J. Gromkowskiego we Wrocławiu"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
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
            "us.edu.pl"
         ],
         "established" : 1968,
         "external_ids" : [
            {
               "all" : [
                  "501100015386"
               ],
               "preferred" : "501100015386",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.11866.38"
               ],
               "preferred" : "grid.11866.38",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2259 4135"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q615154",
                  "Q47462529"
               ],
               "preferred" : "Q615154",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0104rcc94",
         "links" : [
            {
               "type" : "website",
               "value" : "https://us.edu.pl/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/University_of_Silesia_in_Katowice"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "PL",
                  "country_name" : "Poland",
                  "country_subdivision_code" : "24",
                  "country_subdivision_name" : "Silesia",
                  "lat" : 50.25841,
                  "lng" : 19.02754,
                  "name" : "Katowice"
               },
               "geonames_id" : 3096472
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "University of Silesia"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "University of Silesia in Katowice"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UŚ"
            }
         ],
         "relationships" : [],
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
         "domains" : [
            "addisca.de"
         ],
         "established" : 2010,
         "external_ids" : [
            {
               "all" : [
                  "grid.491669.5"
               ],
               "preferred" : "grid.491669.5",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/01q2s4214",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.addisca.de"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "HH",
                  "country_subdivision_name" : "Hamburg",
                  "lat" : 53.55073,
                  "lng" : 9.99302,
                  "name" : "Hamburg"
               },
               "geonames_id" : 2911298
            }
         ],
         "names" : [
            {
               "lang" : "de",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Addisca"
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
               "date" : "2024-05-27",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "buft.edu.bd"
         ],
         "established" : 1999,
         "external_ids" : [
            {
               "all" : [
                  "0000 0004 4682 9041"
               ],
               "preferred" : "0000 0004 4682 9041",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q4835554"
               ],
               "preferred" : "Q4835554",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00x54vt20",
         "links" : [
            {
               "type" : "website",
               "value" : "https://buft.edu.bd"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/BGMEA_University_of_Fashion_%26_Technology"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "BD",
                  "country_name" : "Bangladesh",
                  "country_subdivision_code" : "C",
                  "country_subdivision_name" : "Dhaka Division",
                  "lat" : 23.7104,
                  "lng" : 90.40744,
                  "name" : "Dhaka"
               },
               "geonames_id" : 1185241
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "BGMEA University of Fashion & Technology"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "BGMEA University of Fashion and Technology"
            },
            {
               "lang" : "en",
               "types" : [
                  "acronym"
               ],
               "value" : "BUFT"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Bangladesh Garment Manufacturers and Exporters Association University of Fashion and Technology"
            },
            {
               "lang" : "bn",
               "types" : [
                  "acronym"
               ],
               "value" : "বিইউএফটি"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2024-05-27",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "stpmd.apmd.ac.id"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "0000 0001 0366 7685"
               ],
               "preferred" : "0000 0001 0366 7685",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q109913049"
               ],
               "preferred" : "Q109913049",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0286my572",
         "links" : [
            {
               "type" : "website",
               "value" : "https://stpmd.apmd.ac.id"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "ID",
                  "country_name" : "Indonesia",
                  "country_subdivision_code" : "YO",
                  "country_subdivision_name" : "Yogyakarta",
                  "lat" : -7.80139,
                  "lng" : 110.36472,
                  "name" : "Yogyakarta"
               },
               "geonames_id" : 1621177
            }
         ],
         "names" : [
            {
               "lang" : "id",
               "types" : [
                  "acronym"
               ],
               "value" : "APMD"
            },
            {
               "lang" : "id",
               "types" : [
                  "acronym"
               ],
               "value" : "STPMD APMD"
            },
            {
               "lang" : "id",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Sekolah Tinggi Pembangunan Masyarakat Desa APMD"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education"
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
            "cnsf.bf"
         ],
         "established" : 1983,
         "external_ids" : [
            {
               "all" : [
                  "grid.433136.0"
               ],
               "preferred" : "grid.433136.0",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/03j656z92",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.cnsf.bf/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AF",
                  "continent_name" : "Africa",
                  "country_code" : "BF",
                  "country_name" : "Burkina Faso",
                  "country_subdivision_code" : "03",
                  "country_subdivision_name" : "Centre",
                  "lat" : 12.36566,
                  "lng" : -1.53388,
                  "name" : "Ouagadougou"
               },
               "geonames_id" : 2357048
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CNSF"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Centre National de Semences Forestières"
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
               "date" : "2024-05-27",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "gvncollege.edu.in"
         ],
         "established" : 1966,
         "external_ids" : [
            {
               "all" : [
                  "Q48725521"
               ],
               "preferred" : "Q48725521",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02ddybj40",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.gvncollege.org"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/G.Venkataswamy_Naidu_College%2C_Kovilpatti"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "IN",
                  "country_name" : "India",
                  "country_subdivision_code" : "TN",
                  "country_subdivision_name" : "Tamil Nadu",
                  "lat" : 9.17167,
                  "lng" : 77.86989,
                  "name" : "Kovilpatti"
               },
               "geonames_id" : 1265891
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "G. Venkataswamy Naidu College"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "G.Venkataswamy Naidu College (Autonomous)"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "GVN College"
            },
            {
               "lang" : "en",
               "types" : [
                  "acronym"
               ],
               "value" : "GVNC"
            },
            {
               "lang" : "ta",
               "types" : [
                  "label"
               ],
               "value" : "ஜி. வெங்கடசாமி நாயுடு கல்லூரி"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02qgw5c67",
               "label" : "Manonmaniam Sundaranar University",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "education"
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
            "reem.es"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "501100007747"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.483890.e"
               ],
               "preferred" : "grid.483890.e",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/00dgcm453",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.reem.es"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "ES",
                  "country_name" : "Spain",
                  "country_subdivision_code" : "CT",
                  "country_subdivision_name" : "Catalonia",
                  "lat" : 41.38879,
                  "lng" : 2.15899,
                  "name" : "Barcelona"
               },
               "geonames_id" : 3128760
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "acronym"
               ],
               "value" : "REEM"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Red Española de Esclerosis Múltiple"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Spanish Multiple Sclerosis Network"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
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
         "domains" : [
            "uhr.no"
         ],
         "established" : 1958,
         "external_ids" : [
            {
               "all" : [
                  "grid.459139.3"
               ],
               "preferred" : "grid.459139.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0612 2201"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q12008500"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02hwjhx76",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.uhr.no"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "NO",
                  "country_name" : "Norway",
                  "country_subdivision_code" : "03",
                  "country_subdivision_name" : "Oslo",
                  "lat" : 59.91273,
                  "lng" : 10.74609,
                  "name" : "Oslo"
               },
               "geonames_id" : 3143244
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Norwegian Association of Higher Education Institutions"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "UHR"
            },
            {
               "lang" : "no",
               "types" : [
                  "label"
               ],
               "value" : "Universitets- og høgskolerådet"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Universities Norway"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education"
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
            "army.mil"
         ],
         "established" : 1947,
         "external_ids" : [
            {
               "all" : [
                  "grid.427904.c"
               ],
               "preferred" : "grid.427904.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2315 4051"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1328562"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/035w1gb98",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.army.mil/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/United_States_Department_of_the_Army"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "VA",
                  "country_subdivision_name" : "Virginia",
                  "lat" : 38.88101,
                  "lng" : -77.10428,
                  "name" : "Arlington"
               },
               "geonames_id" : 4744709
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "DA"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Departamento del Ejército de los Estados Unidos"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "U.S. Department of the Army"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "United States Department of the Army"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00wa40650",
               "label" : "Defense Forensics and Biometrics Agency",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00afsp483",
               "label" : "United States Army",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04kz4p343",
               "label" : "Institute for Collaborative Biotechnologies",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0447fe631",
               "label" : "United States Department of Defense",
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
         "domains" : [
            "addu.edu.ph"
         ],
         "established" : 1948,
         "external_ids" : [
            {
               "all" : [
                  "grid.443042.5"
               ],
               "preferred" : "grid.443042.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0626 6676"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q268749"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01w4ss835",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.addu.edu.ph"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Ateneo_de_Davao_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "PH",
                  "country_name" : "Philippines",
                  "country_subdivision_code" : "11",
                  "country_subdivision_name" : "Davao Region",
                  "lat" : 7.07306,
                  "lng" : 125.61278,
                  "name" : "Davao City"
               },
               "geonames_id" : 1715348
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "AdDU"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ateneo de Davao University"
            },
            {
               "lang" : "tl",
               "types" : [
                  "label"
               ],
               "value" : "Pamantasang Ateneo de Davao"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "St. Peter's Parochial School Ateneo de Davao"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "education"
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
            "ucdavis.edu"
         ],
         "established" : 1905,
         "external_ids" : [
            {
               "all" : [
                  "100007707",
                  "100010553",
                  "100009752",
                  "100007862",
                  "100009219",
                  "100008956",
                  "100009751",
                  "100013701",
                  "100011080"
               ],
               "preferred" : "100007707",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.27860.3b"
               ],
               "preferred" : "grid.27860.3b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1936 9684"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q129421"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05rrcem69",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ucdavis.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/University_of_California,_Davis"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 38.54491,
                  "lng" : -121.74052,
                  "name" : "Davis"
               },
               "geonames_id" : 5341704
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "UC Davis"
            },
            {
               "lang" : "en",
               "types" : [
                  "acronym"
               ],
               "value" : "UCD"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Universidad de California en Davis"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of California, Davis"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Université de Californie à Davis"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00fyrp007",
               "label" : "NeuroMab",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00xcmeq33",
               "label" : "Tahoe Environmental Research Center",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00pjdza24",
               "label" : "University of California System",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02ha38c24",
               "label" : "San Joaquin General Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05ehe8t08",
               "label" : "UC Davis Children's Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05q8kyc69",
               "label" : "UC Davis Health System",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/05t6gpm70",
               "label" : "University of California Davis Medical Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/026t0fx73",
               "label" : "Veterinary Medical Teaching Hospital",
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
            "count" : 2663,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 2022,
            "id" : "as",
            "title" : "Asia"
         },
         {
            "count" : 1243,
            "id" : "na",
            "title" : "North America"
         },
         {
            "count" : 380,
            "id" : "af",
            "title" : "Africa"
         },
         {
            "count" : 310,
            "id" : "sa",
            "title" : "South America"
         },
         {
            "count" : 73,
            "id" : "oc",
            "title" : "Oceania"
         }
      ],
      "countries" : [
         {
            "count" : 915,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 533,
            "id" : "cn",
            "title" : "China"
         },
         {
            "count" : 367,
            "id" : "in",
            "title" : "India"
         },
         {
            "count" : 307,
            "id" : "gb",
            "title" : "United Kingdom"
         },
         {
            "count" : 305,
            "id" : "pt",
            "title" : "Portugal"
         },
         {
            "count" : 256,
            "id" : "fr",
            "title" : "France"
         },
         {
            "count" : 232,
            "id" : "de",
            "title" : "Germany"
         },
         {
            "count" : 180,
            "id" : "tr",
            "title" : "Turkey"
         },
         {
            "count" : 158,
            "id" : "id",
            "title" : "Indonesia"
         },
         {
            "count" : 156,
            "id" : "it",
            "title" : "Italy"
         }
      ],
      "statuses" : [
         {
            "count" : 6689,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 4423,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 2237,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 694,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 394,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 321,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 269,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 266,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 255,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 85,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 6689,
   "time_taken" : 37
}
```

# Find inactive child organizations of an active parent organization

Many complex queries can be constructed with the advanced query parameter and [Elasticsearch Boolean operator syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_boolean_operators). In this example, Boolean syntax is used to look for inactive records that include a `parent` relationship type to a specific ROR record. Note that certain relationships to inactive records are removed from active records but retained in the inactive records. Learn more about record [relationships](doc:data-structure#relationships) and record [status](doc:data-structure#status) on the [Data structure](doc:ror-data-structure) page.

When searching for ROR IDs and other URLs, be sure to URL-encode and escape colons and slashes. See [Non-escaped reserved characters](https://ror.readme.io/docs/api-advanced-query#/non-escaped-reserved-characters) below for more guidance.

<Callout icon="📘" theme="info">
  ## Advanced query parameter format, find inactive child organizations

  `https://api.ror.org/v2/organizations?query.advanced=relationships.id[parent ROR ID]+AND+relationships.type:parent+AND+status:inactive`
</Callout>

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=relationships.id:%22https%5C%3A%5C%2F%5C%2Fror.org%5C%2F00pg6eq24%22+AND+relationships.type:parent+AND+status:inactive' | json_pp
```

The response is a list of records for inactive child organizations of Université de Strasbourg.

```json
{
   "items" : [
      {
         "admin" : {
            "created" : {
               "date" : "2023-08-17",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2025-02-26",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2013,
         "external_ids" : [
            {
               "all" : [
                  "Q51781322"
               ],
               "preferred" : "Q51781322",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/000n1xh78",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.unistra.fr/index.php?id=19289"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "GES",
                  "country_subdivision_name" : "Grand Est",
                  "lat" : 48.58392,
                  "lng" : 7.74553,
                  "name" : "Strasbourg"
               },
               "geonames_id" : 2973783
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BMNST"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Biopathologie de la myéline, neuroprotection et stratégies thérapeutiques"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02vjkv261",
               "label" : "Inserm",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/00pg6eq24",
               "label" : "Université de Strasbourg",
               "type" : "parent"
            }
         ],
         "status" : "inactive",
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
         "established" : 2009,
         "external_ids" : [
            {
               "all" : [
                  "grid.503388.5"
               ],
               "preferred" : "grid.503388.5",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/0032jvj22",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.regmed.fr/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "GES",
                  "country_subdivision_name" : "Grand Est",
                  "lat" : 48.58392,
                  "lng" : 7.74553,
                  "name" : "Strasbourg"
               },
               "geonames_id" : 2973783
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Active Biomaterials and Tissue Engineering"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Osteoarticular and Dental Regenerative Nanomedicine"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "RNM"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Regenerative NanoMedicine"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02wy4ac67",
               "label" : "Délégation Alsace",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/00pg6eq24",
               "label" : "University of Strasbourg",
               "type" : "parent"
            }
         ],
         "status" : "inactive",
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
                  "grid.497627.9"
               ],
               "preferred" : "grid.497627.9",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/057916623",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.u1118.inserm.fr/en/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "GES",
                  "country_subdivision_name" : "Grand Est",
                  "lat" : 48.58392,
                  "lng" : 7.74553,
                  "name" : "Strasbourg"
               },
               "geonames_id" : 2973783
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Mécanismes Centraux et Périphériques de la Neurodégénérescence"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04kv7c795",
               "label" : "Délégation Régionale Est",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/00pg6eq24",
               "label" : "University of Strasbourg",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05dd6kb95",
               "label" : "Neuroscience et Psychiatrie Translationnelle de Strasbourg",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
         "types" : [
            "education"
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
                  "grid.492953.0"
               ],
               "preferred" : "grid.492953.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0624 5455"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q52606614"
               ],
               "preferred" : "Q52606614",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02zwf7d57",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.u1114.inserm.fr"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "GES",
                  "country_subdivision_name" : "Grand Est",
                  "lat" : 48.58392,
                  "lng" : 7.74553,
                  "name" : "Strasbourg"
               },
               "geonames_id" : 2973783
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Cognitive Neuropsychology and Physiopathology of Schizophrenia"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Neuropsychologie Cognitive et Physiopathologie de la Schizophrénie"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02vjkv261",
               "label" : "Inserm",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/00pg6eq24",
               "label" : "University of Strasbourg",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05dd6kb95",
               "label" : "Neuroscience et Psychiatrie Translationnelle de Strasbourg",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
         "established" : 2009,
         "external_ids" : [
            {
               "all" : [
                  "grid.469417.9"
               ],
               "preferred" : "grid.469417.9",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2299 9140"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/03y81da23",
         "links" : [
            {
               "type" : "website",
               "value" : "https://lhyges.unistra.fr/-Generale-?lang=en"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "GES",
                  "country_subdivision_name" : "Grand Est",
                  "lat" : 48.58392,
                  "lng" : 7.74553,
                  "name" : "Strasbourg"
               },
               "geonames_id" : 2973783
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LHyGes"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Laboratoire d’HYdrologie et de GEochimie"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Laboratory of Hydrology and Geochemistry"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04kdfz702",
               "label" : "Institut National des Sciences de l'Univers",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02jhjzd11",
               "label" : "National School for Water and Environmental Engineering",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/00pg6eq24",
               "label" : "Université de Strasbourg",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/0530qwm02",
               "label" : "Institut Terre & Environnement de Strasbourg",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
                  "grid.463759.a"
               ],
               "preferred" : "grid.463759.a",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/008pt4x48",
         "links" : [
            {
               "type" : "website",
               "value" : "https://chimie.unistra.fr/recherche/les-equipes-de-recherche-par-domaine/domaine-de-chimie-du-solide-et-catalyse/laboratoire-des-materiaux-surfaces-et-procedes-pour-la-catalyse-lmspc/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "GES",
                  "country_subdivision_name" : "Grand Est",
                  "lat" : 48.58392,
                  "lng" : 7.74553,
                  "name" : "Strasbourg"
               },
               "geonames_id" : 2973783
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LMSPC"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratoire des Matériaux Surfaces et Procédés pour la Catalyse"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02wy4ac67",
               "label" : "Délégation Alsace",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/00pg6eq24",
               "label" : "Université de Strasbourg",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02tn0tm63",
               "label" : "Institut de Chimie et Procédés pour l'Energie, l'Environnement et la Santé",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
         "established" : 1997,
         "external_ids" : [
            {
               "all" : [
                  "grid.483496.4"
               ],
               "preferred" : "grid.483496.4",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/01epym565",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.lcm-umr7509.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "GES",
                  "country_subdivision_name" : "Grand Est",
                  "lat" : 48.58392,
                  "lng" : 7.74553,
                  "name" : "Strasbourg"
               },
               "geonames_id" : 2973783
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LCM"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratoire de Chimie Moléculaire"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02cte4b68",
               "label" : "Institut de Chimie",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/00pg6eq24",
               "label" : "University of Strasbourg",
               "type" : "parent"
            }
         ],
         "status" : "inactive",
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
               "date" : "2025-03-17",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2009,
         "external_ids" : [
            {
               "all" : [
                  "grid.463906.e"
               ],
               "preferred" : "grid.463906.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0368 2086"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/0032sc770",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www-lbp.unistra.fr/?lang=fr"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "GES",
                  "country_subdivision_name" : "Grand Est",
                  "lat" : 48.52894,
                  "lng" : 7.71523,
                  "name" : "Illkirch-Graffenstaden"
               },
               "geonames_id" : 3012834
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Laboratoire de Biophotonique et Pharmacologie"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Laboratory of Biophotonics and Pharmacology"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00rydyx93",
               "label" : "Institut des Sciences Biologiques",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/00pg6eq24",
               "label" : "Université de Strasbourg",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/059g0vd42",
               "label" : "Laboratoire de bioimagerie et pathologies",
               "type" : "successor"
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
            "count" : 8,
            "id" : "eu",
            "title" : "Europe"
         }
      ],
      "countries" : [
         {
            "count" : 8,
            "id" : "fr",
            "title" : "France"
         }
      ],
      "statuses" : [
         {
            "count" : 8,
            "id" : "inactive",
            "title" : "inactive"
         }
      ],
      "types" : [
         {
            "count" : 7,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 1,
            "id" : "education",
            "title" : "education"
         }
      ]
   },
   "number_of_results" : 8,
   "time_taken" : 7
}
```

# Paging and filtering

Search results from the advanced query parameter are [paginated](doc:api-paging) and can be [filtered](doc:api-filtering) by status, type, country name, country code, continent name, and continent code. The [all\_status](doc:api-list) parameter can also be appended to advanced query parameter searches in order to retrieve _inactive_ and _withdrawn_ records as well as _active_ records. See [Paging](doc:api-paging) and [Filtering](doc:api-filtering) for more information.

## Example

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=names.lang:es&filter=locations.geonames_details.country_code:US&all_status&page=12' | json_pp
```

The response is the 12th page of a list of active, inactive, and withdrawn records for research organizations in the U.S. with at least one Spanish-language name.

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
         "established" : 1988,
         "external_ids" : [
            {
               "all" : [
                  "grid.487715.8"
               ],
               "preferred" : "grid.487715.8",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04w70m307",
         "links" : [
            {
               "type" : "website",
               "value" : "http://cedo.org/en/home/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/CEDO"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "AZ",
                  "country_subdivision_name" : "Arizona",
                  "lat" : 32.22174,
                  "lng" : -110.92648,
                  "name" : "Tucson"
               },
               "geonames_id" : 5318313
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CEDO"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Centro Intercultural de Estudios de Desiertos y Océanos"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Intercultural Center for the Study of Deserts and Oceans"
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
         "established" : 1936,
         "external_ids" : [
            {
               "all" : [
                  "grid.461382.a"
               ],
               "preferred" : "grid.461382.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0279 9432"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q743945"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05r1ynr77",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.ajcunet.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Association_of_Jesuit_Colleges_and_Universities"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "DC",
                  "country_subdivision_name" : "District of Columbia",
                  "lat" : 38.89511,
                  "lng" : -77.03637,
                  "name" : "Washington"
               },
               "geonames_id" : 4140963
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "AJCU"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Asociación de Universidades Jesuitas"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Association des universités et facultés jésuites"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Association of Jesuit Colleges and Universities"
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
         "established" : 1885,
         "external_ids" : [
            {
               "all" : [
                  "grid.461473.3"
               ],
               "preferred" : "grid.461473.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 8957 9477"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q7902415"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00v19am98",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.ush.utah.gov/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Utah_State_Hospital"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "UT",
                  "country_subdivision_name" : "Utah",
                  "lat" : 40.23384,
                  "lng" : -111.65853,
                  "name" : "Provo"
               },
               "geonames_id" : 5780026
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Hospital del estado de Utah"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "USH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Utah State Hospital"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
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
         "established" : 1925,
         "external_ids" : [
            {
               "all" : [
                  "grid.461594.b"
               ],
               "preferred" : "grid.461594.b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0401 1575"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q9091546"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0272wvz49",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.lattc.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Los_Angeles_Trade%E2%80%93Technical_College"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 34.05223,
                  "lng" : -118.24368,
                  "name" : "Los Angeles"
               },
               "geonames_id" : 5368361
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Frank Wiggins Trade School"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "L.A. Trade–Tech"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LATTC"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Los Angeles Trade Technical College"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Metropolitan Business School"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Universidad Comunitaria Vocacional–Técnica de Los Ángeles"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/058aed112",
               "label" : "Los Angeles Community College District",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "education"
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
         "established" : 1934,
         "external_ids" : [
            {
               "all" : [
                  "grid.461963.f"
               ],
               "preferred" : "grid.461963.f",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2353 8346"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q128831"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03545fq57",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.fcc.gov/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Federal_Communications_Commission"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "DC",
                  "country_subdivision_name" : "District of Columbia",
                  "lat" : 38.89511,
                  "lng" : -77.03637,
                  "name" : "Washington"
               },
               "geonames_id" : 4140963
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Comisión Federal de Comunicaciones"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "FCC"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Federal Communications Commission"
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
         "established" : 1885,
         "external_ids" : [
            {
               "all" : [
                  "grid.462339.9"
               ],
               "preferred" : "grid.462339.9",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2231 2169"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q530512"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05gjbhz21",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.hclib.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Hennepin_County_Library"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MN",
                  "country_subdivision_name" : "Minnesota",
                  "lat" : 44.9133,
                  "lng" : -93.50329,
                  "name" : "Minnetonka"
               },
               "geonames_id" : 5037784
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Biblioteca del Condado de Hennepin"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "HCL"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Hennepin County Library"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/032tz4q83",
               "label" : "Minneapolis Public Library",
               "type" : "child"
            }
         ],
         "status" : "active",
         "types" : [
            "archive"
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
         "established" : 1959,
         "external_ids" : [
            {
               "all" : [
                  "grid.462419.c"
               ],
               "preferred" : "grid.462419.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q1142111"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04f0f7630",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.metoc.navy.mil/jtwc/jtwc.html"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Joint_Typhoon_Warning_Center"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "HI",
                  "country_subdivision_name" : "Hawaii",
                  "lat" : 21.33967,
                  "lng" : -157.96018,
                  "name" : "Hickam Field"
               },
               "geonames_id" : 7262713
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Centro Conjunto de Advertencia de Tifones"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "JTWC"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Joint Typhoon Warning Center"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/014khrd35",
               "label" : "Naval Meteorology and Oceanography Command",
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
               "date" : "2019-02-17",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2003,
         "external_ids" : [
            {
               "all" : [
                  "grid.503635.6"
               ],
               "preferred" : "grid.503635.6",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/00w60d804",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.escr-net.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 40.71427,
                  "lng" : -74.00597,
                  "name" : "New York"
               },
               "geonames_id" : 5128581
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ESCR-Net"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "International Network for Economic, Social and Cultural Rights"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Red Internacional para los Derechos Económicos, Sociales y Culturales"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "Red-DESC"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Réseau International pour les Droits Economiques, Sociaux et Culturels"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "Réseau-DESC"
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
         "established" : 1960,
         "external_ids" : [
            {
               "all" : [
                  "100006197"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.419091.4"
               ],
               "preferred" : "grid.419091.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2238 4912"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q618696"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02epydz83",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.nasa.gov/centers/marshall/home/index.html"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Marshall_Space_Flight_Center"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "AL",
                  "country_subdivision_name" : "Alabama",
                  "lat" : 34.68387,
                  "lng" : -86.64764,
                  "name" : "Redstone Arsenal"
               },
               "geonames_id" : 7260623
            }
         ],
         "names" : [
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Centre de Vol Spatial Marshall"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Centro Marshall de Vuelos Espaciales"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "MSFC"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Marshall Space Flight Center"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/045t78n53",
               "label" : "Michoud Assembly Facility",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/027ka1x80",
               "label" : "National Aeronautics and Space Administration",
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
               "date" : "2018-11-14",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2014,
         "external_ids" : [
            {
               "all" : [
                  "100007346",
                  "100006204"
               ],
               "preferred" : "100007346",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.455981.4"
               ],
               "preferred" : "grid.455981.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0805 6998"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q305443"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03em45j53",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.nasa.gov/centers/armstrong/home/index.html"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Armstrong_Flight_Research_Center"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 34.92609,
                  "lng" : -117.93507,
                  "name" : "Edwards"
               },
               "geonames_id" : 5345449
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "AFRC"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Armstrong Flight Research Center"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Centro Dryden de Investigaciones de Vuelo"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "DFRC"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Dryden Flight Research Center"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "High-Speed Flight Research Station"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/027ka1x80",
               "label" : "National Aeronautics and Space Administration",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/01wmcm405",
               "label" : "Muroc Army Air Base",
               "type" : "predecessor"
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
               "date" : "2023-07-06",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1922,
         "external_ids" : [
            {
               "all" : [
                  "100013468"
               ],
               "preferred" : "100013468",
               "type" : "fundref"
            },
            {
               "all" : [
                  "0000 0001 0739 950X"
               ],
               "preferred" : "0000 0001 0739 950X",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q4809797"
               ],
               "preferred" : "Q4809797",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05g27v878",
         "links" : [
            {
               "type" : "website",
               "value" : "https://afonet.org"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Association_of_Field_Ornithologists"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "MA",
                  "country_subdivision_name" : "Massachusetts",
                  "lat" : 41.63526,
                  "lng" : -70.92701,
                  "name" : "New Bedford"
               },
               "geonames_id" : 4945121
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "AFO"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "AOC"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Asociación de Ornitólogos de Campo"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Association of Field Ornithologists"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Association of Field Ornithologists, Inc."
            },
            {
               "lang" : "pt",
               "types" : [
                  "label"
               ],
               "value" : "Associação de Ornitólogos de Campo"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "New England Bird Banding Association"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "The Association of Field Ornithologists"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
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
         "established" : 1939,
         "external_ids" : [
            {
               "all" : [
                  "100006195"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.419075.e"
               ],
               "preferred" : "grid.419075.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 1955 7990"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q181052"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02acart68",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.nasa.gov/centers/ames/home/index.html"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Ames_Research_Center"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.38605,
                  "lng" : -122.08385,
                  "name" : "Mountain View"
               },
               "geonames_id" : 5375480
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ARC"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ames Research Center"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Centro de Investigación Ames"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "NASA Ames"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04hccab49",
               "label" : "NASA Research Park",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0540g1c48",
               "label" : "Radiation Biophysics Laboratory",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/027ka1x80",
               "label" : "National Aeronautics and Space Administration",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02yhmtm27",
               "label" : "Ames Aeronautical Laboratory",
               "type" : "predecessor"
            },
            {
               "id" : "https://ror.org/022qmyy53",
               "label" : "Research Institute for Advanced Computer Science",
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
               "date" : "2023-07-06",
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
                  "100013755"
               ],
               "preferred" : "100013755",
               "type" : "fundref"
            },
            {
               "all" : [
                  "Q7973891"
               ],
               "preferred" : "Q7973891",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04zfk0m49",
         "links" : [
            {
               "type" : "website",
               "value" : "https://waterbirds.org"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Waterbird_Society"
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
                  "lat" : 43.80136,
                  "lng" : -91.23958,
                  "name" : "La Crosse"
               },
               "geonames_id" : 5258957
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Colonial Waterbird Group"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Colonial Waterbird Society"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Sociedad de Aves Marítimas"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "The Waterbird Society"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "WBS"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Waterbird Society"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
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
         "established" : 1787,
         "external_ids" : [
            {
               "all" : [
                  "100007921",
                  "100008031",
                  "100008014",
                  "100007295",
                  "100007125",
                  "100008603",
                  "100010554"
               ],
               "preferred" : "100007921",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.21925.3d"
               ],
               "preferred" : "grid.21925.3d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 1936 9000"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q235034"
               ],
               "preferred" : "Q235034",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01an3r305",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.pitt.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/University_of_Pittsburgh"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "PA",
                  "country_subdivision_name" : "Pennsylvania",
                  "lat" : 40.44062,
                  "lng" : -79.99589,
                  "name" : "Pittsburgh"
               },
               "geonames_id" : 5206379
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Universidad de Pittsburgh"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "University of Pittsburgh"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Université de Pittsburgh"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/00jfeg660",
               "label" : "Center for the Neural Basis of Cognition",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04js6xx21",
               "label" : "McGowan Institute for Regenerative Medicine",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0019bf448",
               "label" : "University of Pittsburgh at Bradford",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02b394754",
               "label" : "University of Pittsburgh at Greensburg",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03n9scz70",
               "label" : "University of Pittsburgh at Johnstown",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0242vta36",
               "label" : "University of Pittsburgh at Titusville",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03bw34a45",
               "label" : "UPMC Hillman Cancer Center",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04a0qsn58",
               "label" : "St. Margaret Memorial Hospital",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03bw34a45",
               "label" : "UPMC Hillman Cancer Center",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/04ehecz88",
               "label" : "University of Pittsburgh Medical Center",
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
         "established" : 1869,
         "external_ids" : [
            {
               "all" : [
                  "100020616"
               ],
               "preferred" : "100020616",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.263856.c"
               ],
               "preferred" : "grid.263856.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0806 3768"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1472347"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/049kefs16",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.siu.edu/"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Southern_Illinois_University_Carbondale"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "IL",
                  "country_subdivision_name" : "Illinois",
                  "lat" : 37.72727,
                  "lng" : -89.21675,
                  "name" : "Carbondale"
               },
               "geonames_id" : 4235193
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "SIU"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "SIU Carbondale"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Southern Illinois University Carbondale"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Universidad del Sur de Illinois Carbondale"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Université du sud de l'illinois à carbondale"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05vz28418",
               "label" : "Southern Illinois University System",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03wtwre51",
               "label" : "Decatur Memorial Hospital",
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
         "established" : 1804,
         "external_ids" : [
            {
               "all" : [
                  "100008076",
                  "100008941"
               ],
               "preferred" : "100008076",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.20627.31"
               ],
               "preferred" : "grid.20627.31",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0668 7841"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1075339"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01jr3y717",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ohio.edu"
            },
            {
               "type" : "wikipedia",
               "value" : "http://en.wikipedia.org/wiki/Ohio_University"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "OH",
                  "country_subdivision_name" : "Ohio",
                  "lat" : 39.32924,
                  "lng" : -82.10126,
                  "name" : "Athens"
               },
               "geonames_id" : 4505542
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Ohio University"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Universidad de Ohio"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Université de l'ohio"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/006hbbw23",
               "label" : "Ohio University Eastern",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05fxqx152",
               "label" : "Ohio University Chillicothe",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/039eaqm23",
               "label" : "Ohio University Southern",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03kf4rc21",
               "label" : "Ohio University Lancaster",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0561zqn56",
               "label" : "University System of Ohio",
               "type" : "parent"
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
         "established" : 1979,
         "external_ids" : [
            {
               "all" : [
                  "100000138",
                  "100005245",
                  "100005249",
                  "100005250",
                  "100007351",
                  "100005248",
                  "100005252",
                  "100005254",
                  "100005247"
               ],
               "preferred" : "100000138",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.419881.d"
               ],
               "preferred" : "grid.419881.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2176 2483"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q861556"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/021adze67",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.ed.gov/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/United_States_Department_of_Education"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "DC",
                  "country_subdivision_name" : "District of Columbia",
                  "lat" : 38.89511,
                  "lng" : -77.03637,
                  "name" : "Washington"
               },
               "geonames_id" : 4140963
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Departamento de Educación de los Estados Unidos"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "DoED"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Département de l'Éducation des États-unis"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "ED "
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "United States Department of Education"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04et59085",
               "label" : "Institute of Education Sciences",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00fyvea65",
               "label" : "National Library of Education",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02rcrvv70",
               "label" : "Government of the United States of America",
               "type" : "parent"
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
                  "grid.430355.4"
               ],
               "preferred" : "grid.430355.4",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/02ptchg16",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.stri.si.edu/english"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Smithsonian_Tropical_Research_Institute"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "DC",
                  "country_subdivision_name" : "District of Columbia",
                  "lat" : 38.89511,
                  "lng" : -77.03637,
                  "name" : "Washington"
               },
               "geonames_id" : 4140963
            }
         ],
         "names" : [
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Instituto Smithsonian de Investigaciones Tropicales"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "STRI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Smithsonian Tropical Research Institute"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/035jbxr46",
               "label" : "Smithsonian Tropical Research Institute",
               "type" : "successor"
            }
         ],
         "status" : "withdrawn",
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
         "established" : 1862,
         "external_ids" : [
            {
               "all" : [
                  "100000199",
                  "100006509",
                  "100007014",
                  "100006102",
                  "100006103",
                  "100006101",
                  "100009167",
                  "100009169",
                  "100006104",
                  "100009172"
               ],
               "preferred" : "100000199",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.417548.b"
               ],
               "preferred" : "grid.417548.b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0478 6311"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q501542"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01na82s61",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.usda.gov/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/United_States_Department_of_Agriculture"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "DC",
                  "country_subdivision_name" : "District of Columbia",
                  "lat" : 38.89511,
                  "lng" : -77.03637,
                  "name" : "Washington"
               },
               "geonames_id" : 4140963
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Agriculture Department"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Departamento de Agricultura de los Estados Unidos"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Département de l'Agriculture des États-Unis"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "USDA"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "United States Department of Agriculture"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01xx4fy37",
               "label" : "Agricultural Marketing Service",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02d2m2044",
               "label" : "Agricultural Research Service",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/0599wfz09",
               "label" : "Animal and Plant Health Inspection Service",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02bgabj83",
               "label" : "Center for Nutrition Policy and Promotion",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05ycxzd89",
               "label" : "Economic Research Service",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/050m26017",
               "label" : "Farm Service Agency",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/045k2ma45",
               "label" : "Food Safety and Inspection Service",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00t9khr87",
               "label" : "Food and Nutrition Service",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00t77bz53",
               "label" : "Foreign Agricultural Service",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/056mcm922",
               "label" : "Grain Inspection, Packers and Stockyards Administration",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00z6b1508",
               "label" : "National Agricultural Library",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04dpymk59",
               "label" : "National Agricultural Statistics Service",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05qx3fv49",
               "label" : "National Institute of Food and Agriculture",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03j7rgg33",
               "label" : "Natural Resources Conservation Service",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05222ev03",
               "label" : "Risk Management Agency",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03zmjc935",
               "label" : "US Forest Service",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02qhkmh27",
               "label" : "USDA Rural Development",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00e7ec781",
               "label" : "United States Department of Agriculture Climate Hubs",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02rcrvv70",
               "label" : "Government of the United States of America",
               "type" : "parent"
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
         "established" : 1869,
         "external_ids" : [
            {
               "all" : [
                  "100005835",
                  "100016451"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.241963.b"
               ],
               "preferred" : "grid.241963.b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2152 1081"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q217717"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03thb3e06",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.amnh.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/American_Museum_of_Natural_History"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "NY",
                  "country_subdivision_name" : "New York",
                  "lat" : 40.71427,
                  "lng" : -74.00597,
                  "name" : "New York"
               },
               "geonames_id" : 5128581
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "AMNH"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "American Museum of Natural History"
            },
            {
               "lang" : "es",
               "types" : [
                  "label"
               ],
               "value" : "Museo Americano de Historia Natural"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label"
               ],
               "value" : "Musée américain d'histoire naturelle"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "archive",
            "funder"
         ]
      }
   ],
   "meta" : {
      "continents" : [
         {
            "count" : 428,
            "id" : "na",
            "title" : "North America"
         }
      ],
      "countries" : [
         {
            "count" : 428,
            "id" : "us",
            "title" : "United States"
         }
      ],
      "statuses" : [
         {
            "count" : 426,
            "id" : "active",
            "title" : "active"
         },
         {
            "count" : 2,
            "id" : "withdrawn",
            "title" : "withdrawn"
         }
      ],
      "types" : [
         {
            "count" : 302,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 282,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 64,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 23,
            "id" : "archive",
            "title" : "archive"
         },
         {
            "count" : 17,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 16,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 13,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 13,
            "id" : "other",
            "title" : "other"
         }
      ]
   },
   "number_of_results" : 428,
   "time_taken" : 3
}
```

# Errors and troubleshooting

## Incorrect field names

The following cases will cause a field name validation error:

- Performing fielded searches that contain field names not listed in the table of [All ROR fields and sub-fields](doc:fields)
- Searching a parent field name without using a wildcard operator, e.g., `locations:Melbourne` rather than `locations.%5c*:Melbourne`

A query with an incorrect field name will return an error.

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=locations.city:Melbourne' | json_pp
```

```json
{
   "errors" : [
      "string 'locations.city' contains an illegal field name"
   ]
}
```

## Multiple query parameters

Use either the `query` or the `query.advanced` parameter, not both. A query that attempts to use both will return an error.

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=names.value:Cornell&query=Ithaca' | json_pp
```

```json
{
   "errors" : [
      "query and query.advanced parameters cannot be combined. please use either query OR query.advanced"
   ]
}
```

## Non-escaped reserved characters

[Elasticsearch reserved characters](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#_reserved_characters) `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /` used within your search terms must be escaped with a URL-encoded `\` character, `%5c`. If these characters are not escaped, the following issues may occur:

- Illegal field name error due to non-escaped `:` character, which is interpreted as a field name
- 500 error due to non-terminated or non-escaped special characters
- Unexpected or non-relevant results due to special characters being interpreted as query operators rather than as part of your search terms

Be especially sure to escape and URL-encode colons and slashes when searching for URLs and ROR IDs.

### Example

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=relationships.id:%22https://ror.org/02en5vm52%22&filter=status:inactive' | json_pp

{
   "errors" : [
      "string '\"https' contains an illegal field name"
   ]
}
```

The query for a ROR ID in the `relationships.id` field fails because the colon is not escaped, even though the search term is surrounded by quotation marks. Escaping forward slashes might not be necessary when the search term is surrounded by quotation marks but might be necessary when the search term is not surrounded by quotation marks.

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=relationships.id:https%5C%3A%5C%2F%5C%2Fror.org%5C%2F02en5vm52&filter=status:inactive' | json_pp
```

When the colon and slashes in the ROR ID are escaped and URL-encoded in this query, the ROR API returns a list of inactive records with the ROR ID for Sorbonne Université in the `relationships.id` field.

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
         "established" : 2015,
         "external_ids" : [
            {
               "all" : [
                  "grid.463964.a"
               ],
               "preferred" : "grid.463964.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0468 1077"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/01vnn4t89",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.lsta.lab.upmc.fr/en/index.html"
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
                  "acronym"
               ],
               "value" : "LSTA"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratoire de Statistique Théorique et Appliquée"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/050jcm728",
               "label" : "Institut National des Sciences Mathématiques et de leurs Interactions",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne University",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02vnd0e65",
               "label" : "Laboratoire de Probabilités, Statistique et Modélisation",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
         "established" : 2007,
         "external_ids" : [
            {
               "all" : [
                  "grid.503242.2"
               ],
               "preferred" : "grid.503242.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0369 188X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3152182"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00801j392",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.iscc.cnrs.fr/"
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
                  "acronym"
               ],
               "value" : "ISCC"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Institut des sciences de la communication"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Institute for Communication Sciences"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02ft5tp06",
               "label" : "Délégation Ile-de-France Ouest et Nord",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne University",
               "type" : "parent"
            }
         ],
         "status" : "inactive",
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
               "date" : "2025-06-24",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2009,
         "external_ids" : [
            {
               "all" : [
                  "grid.463975.a"
               ],
               "preferred" : "grid.463975.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 6003 953X"
               ],
               "preferred" : "0000 0004 6003 953X",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30262347"
               ],
               "preferred" : "Q30262347",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05vd6h165",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.chimie.ens.fr/recherche/laboratoire-lbm/"
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
               "value" : "LBM Laboratory"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Laboratoire LBM"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratoire des Biomolécules"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02vjkv261",
               "label" : "Inserm",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02cte4b68",
               "label" : "Institut de Chimie",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05a0dhs15",
               "label" : "École Normale Supérieure - PSL",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05ebxsr90",
               "label" : "Chimie Physique et Chimie du Vivant",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
               "date" : "2025-06-24",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1998,
         "external_ids" : [
            {
               "all" : [
                  "grid.462619.e"
               ],
               "preferred" : "grid.462619.e",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0368 9974"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30261511"
               ],
               "preferred" : "Q30261511",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/033t7vz72",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.chimie.ens.fr/recherche/laboratoire-pasteur/"
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
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Laboratoire PASTEUR"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "PASTEUR"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Processus d'Activation Sélective par Transfert d'Énergie Uni-électronique ou Radiatif"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02cte4b68",
               "label" : "Institut de Chimie",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05a0dhs15",
               "label" : "École Normale Supérieure - PSL",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05ebxsr90",
               "label" : "Chimie Physique et Chimie du Vivant",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
               "date" : "2025-06-24",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1997,
         "external_ids" : [
            {
               "all" : [
                  "grid.462949.4"
               ],
               "preferred" : "grid.462949.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0370 0838"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30261609"
               ],
               "preferred" : "Q30261609",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/024hnwe62",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ibps.sorbonne-universite.fr/fr/Recherche/umr-biologie-developpement"
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
               "value" : "Developmental Biology Laboratory"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "LBD"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratoire de Biologie du Développement"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02vjkv261",
               "label" : "Inserm",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/00rydyx93",
               "label" : "Institut des Sciences Biologiques",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03p96ke84",
               "label" : "Développement Adaptation et Vieillissement",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
               "date" : "2025-02-26",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1998,
         "external_ids" : [
            {
               "all" : [
                  "grid.464088.6"
               ],
               "preferred" : "grid.464088.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9482 2072"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30262445"
               ],
               "preferred" : "Q30262445",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03tdef037",
         "links" : [
            {
               "type" : "website",
               "value" : "https://syrte.obspm.fr/spip"
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
                  "acronym"
               ],
               "value" : "SYRTE"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Systèmes de Référence Temps-Espace"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04kdfz702",
               "label" : "Institut National des Sciences de l'Univers",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/01ph39d13",
               "label" : "Laboratoire National de Métrologie et d'Essais",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/029nkcm90",
               "label" : "Observatoire de Paris",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03hgsyv65",
               "label" : "Laboratoire Temps Espace",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
         "established" : 2009,
         "external_ids" : [
            {
               "all" : [
                  "grid.493859.a"
               ],
               "preferred" : "grid.493859.a",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/03ct0te27",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.sb-roscoff.fr/fr/phosphorylation-de-proteines-et-pathologies-humaines"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "FR",
                  "country_name" : "France",
                  "country_subdivision_code" : "BRE",
                  "country_subdivision_name" : "Brittany",
                  "lat" : 48.72381,
                  "lng" : -3.98709,
                  "name" : "Roscoff"
               },
               "geonames_id" : 2982809
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "P3H"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Phosphorylation de protéines et Pathologies Humaines"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02t220m45",
               "label" : "Délégation Bretagne et Pays de la Loire",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne University",
               "type" : "parent"
            }
         ],
         "status" : "inactive",
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
                  "grid.464085.b"
               ],
               "preferred" : "grid.464085.b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0367 1053"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/031ve0e22",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.ird.fr/infos-pratiques/archives/unites-fermees/unites-fermees-en-decembre-2013/148-systematique-adaptation-evolution"
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
                  "acronym"
               ],
               "value" : "SAE"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Systematics, Adaptation, Evolution"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Systématique, adaptation, évolution"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05q3vnk25",
               "label" : "Institut de Recherche pour le Développement",
               "type" : "parent"
            },
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
         "established" : 1965,
         "external_ids" : [
            {
               "all" : [
                  "grid.503446.0"
               ],
               "preferred" : "grid.503446.0",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q51782084"
               ],
               "preferred" : "Q51782084",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00kapac04",
         "links" : [
            {
               "type" : "website",
               "value" : "http://krctnn.com"
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
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Des Maladies Rénales Rares aux Maladies Fréquentes, Remodelage et Réparation"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "KRCTNN"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02e0y6e06",
               "label" : "Délégation Paris 5",
               "type" : "parent"
            },
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
         "established" : 2014,
         "external_ids" : [
            {
               "all" : [
                  "Q51784461"
               ],
               "preferred" : "Q51784461",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02y2c2646",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ibps.sorbonne-universite.fr/fr/Recherche/umr-8256"
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
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Adaptation Biologique et Vieillissement"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "B2A"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Biological Adaptation and Ageing"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02feahw73",
               "label" : "Centre National de la Recherche Scientifique",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02vjkv261",
               "label" : "Inserm",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03p96ke84",
               "label" : "Développement Adaptation et Vieillissement",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
               "date" : "2025-08-26",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1998,
         "external_ids" : [
            {
               "all" : [
                  "grid.462516.2"
               ],
               "preferred" : "grid.462516.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0672 5780"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q3152052"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/002zc3t08",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.imcce.fr"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Institut_de_m%C3%A9canique_c%C3%A9leste_et_de_calcul_des_%C3%A9ph%C3%A9m%C3%A9rides"
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
                  "acronym"
               ],
               "value" : "IMCCE"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut de Mécanique Céleste et de Calcul des Éphémérides"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/04kdfz702",
               "label" : "Institut National des Sciences de l'Univers",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/029nkcm90",
               "label" : "Observatoire de Paris",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02kzqn938",
               "label" : "Université de Lille",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02feahw73",
               "label" : "Centre National de la Recherche Scientifique",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03hgsyv65",
               "label" : "Laboratoire Temps Espace",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
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
               "date" : "2025-08-26",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2002,
         "external_ids" : [
            {
               "all" : [
                  "grid.503281.d"
               ],
               "preferred" : "grid.503281.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0370 8645"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q51780993"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01g5pq328",
         "links" : [
            {
               "type" : "website",
               "value" : "https://lerma.obspm.fr"
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
                  "acronym"
               ],
               "value" : "LERMA"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Laboratoire d’Etudes du Rayonnement et de la Matière en Astrophysique et Atmosphères"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Laboratory for Studies of Radiation and Matter in Astrophysics and Atmospheres"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/043htjv09",
               "label" : "CY Cergy Paris Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/04kdfz702",
               "label" : "Institut National des Sciences de l'Univers",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/029nkcm90",
               "label" : "Observatoire de Paris",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/05a0dhs15",
               "label" : "École Normale Supérieure - PSL",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03a26mh11",
               "label" : "Laboratoire de Physique de l'ENS",
               "type" : "successor"
            },
            {
               "id" : "https://ror.org/017zz7s54",
               "label" : "LIRA - Laboratoire d'instrumentation et de recherche en astrophysique",
               "type" : "successor"
            },
            {
               "id" : "https://ror.org/00cpdar66",
               "label" : "Laboratoire d'Étude de l'Univers et des Phénomènes Extrêmes",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
         "types" : [
            "facility"
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
         "established" : 2015,
         "external_ids" : [
            {
               "all" : [
                  "Q123695066"
               ],
               "preferred" : "Q123695066",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04nc8v966",
         "links" : [
            {
               "type" : "website",
               "value" : "https://sante.sorbonne-universite.fr/structures-de-recherche/psydev"
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
               "value" : "GRC 15"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "GRC PSYDEV"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "PSYDEV"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Troubles psychiatriques et développement"
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
                  "Q123694994"
               ],
               "preferred" : "Q123694994",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00b4yme84",
         "links" : [
            {
               "type" : "website",
               "value" : "https://sante.sorbonne-universite.fr/structures-de-recherche/grc-27-greco"
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
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "GRC 27"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "GRC GRECO"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "GRECO"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Groupe de REcherche en Cardio Oncologie"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Groupe de REcherche en Cardio Oncologie - GRC 27"
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
         "established" : 2014,
         "external_ids" : [
            {
               "all" : [
                  "Q51781440"
               ],
               "preferred" : "Q51781440",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05edayg07",
         "links" : [],
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
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Fédération de Physico-Chimie Analytique et Biologique"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "PCAB"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Physico-Chimie Analytique et Biologique"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05q65zh81",
               "label" : "Chimie ParisTech",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/04ex24z53",
               "label" : "Collège de France",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/03wkt5x30",
               "label" : "Muséum national d'Histoire naturelle",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/02feahw73",
               "label" : "Centre National de la Recherche Scientifique",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/02vjkv261",
               "label" : "Inserm",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "related"
            },
            {
               "id" : "https://ror.org/013cjyk83",
               "label" : "Université Paris Sciences et Lettres",
               "type" : "related"
            }
         ],
         "status" : "inactive",
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
               "date" : "2025-08-26",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2007,
         "external_ids" : [
            {
               "all" : [
                  "grid.462192.a"
               ],
               "preferred" : "grid.462192.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0520 8345"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30261398"
               ],
               "preferred" : "Q30261398",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03x9frp33",
         "links" : [
            {
               "type" : "website",
               "value" : "https://ifm-institute.org"
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
                  "acronym"
               ],
               "value" : "IFM"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut du Fer à Moulin"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02vjkv261",
               "label" : "Inserm",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02pgsjq66",
               "label" : "Neuro-SU",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
         "types" : [
            "facility"
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
         "established" : 2014,
         "external_ids" : [
            {
               "all" : [
                  "Q51781831"
               ],
               "preferred" : "Q51781831",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02sps6z09",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.ibps.sorbonne-universite.fr/fr/Recherche/umr-8246"
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
                  "acronym"
               ],
               "value" : "NPS"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "NPS-IBPS"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Neuroscience Paris Seine"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Neuroscience Paris Seine-IBPS"
            },
            {
               "lang" : "fr",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Neurosciences Paris-Seine"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02feahw73",
               "label" : "Centre National de la Recherche Scientifique",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02vjkv261",
               "label" : "Inserm",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02pgsjq66",
               "label" : "Neuro-SU",
               "type" : "successor"
            }
         ],
         "status" : "inactive",
         "types" : [
            "facility"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2025-06-24",
               "schema_version" : "2.1"
            },
            "last_modified" : {
               "date" : "2025-08-26",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "enec.cnrs.fr"
         ],
         "established" : 2006,
         "external_ids" : [
            {
               "all" : [
                  "0000 0001 1463 8724"
               ],
               "preferred" : "0000 0001 1463 8724",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q51779660"
               ],
               "preferred" : "Q51779660",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/00yphsh03",
         "links" : [],
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
               "lang" : "fr",
               "types" : [
                  "acronym"
               ],
               "value" : "ENeC"
            },
            {
               "lang" : "fr",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Espaces, Nature et Culture"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "FRE 2026"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Laboratoire ENeC"
            },
            {
               "lang" : "fr",
               "types" : [
                  "alias"
               ],
               "value" : "Laboratoire Espaces, nature et culture"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Spaces, Nature and Culture"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "UMR 8185"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/04tb23024",
               "label" : "Laboratoire Médiations",
               "type" : "related"
            }
         ],
         "status" : "inactive",
         "types" : [
            "facility"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2023-12-07",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2025-09-22",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2017,
         "external_ids" : [
            {
               "all" : [
                  "Q123695101"
               ],
               "preferred" : "Q123695101",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01w108f05",
         "links" : [
            {
               "type" : "website",
               "value" : "https://2b-fuel.cnrs.fr"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "AS",
                  "continent_name" : "Asia",
                  "country_code" : "KR",
                  "country_name" : "South Korea",
                  "country_subdivision_code" : "11",
                  "country_subdivision_name" : "Seoul",
                  "lat" : 37.566,
                  "lng" : 126.9784,
                  "name" : "Seoul"
               },
               "geonames_id" : 1835848
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "2B-FUEL"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "2B-FUEL (Building Blocks for FUture Electronics Laboratory)"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Building Blocks for Future Electronics"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Building Blocks for Future Electronics Laboratory"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02feahw73",
               "label" : "Centre National de la Recherche Scientifique",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/02en5vm52",
               "label" : "Sorbonne Université",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/01wjejq96",
               "label" : "Yonsei University",
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
            "count" : 23,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 1,
            "id" : "as",
            "title" : "Asia"
         }
      ],
      "countries" : [
         {
            "count" : 23,
            "id" : "fr",
            "title" : "France"
         },
         {
            "count" : 1,
            "id" : "kr",
            "title" : "South Korea"
         }
      ],
      "statuses" : [
         {
            "count" : 24,
            "id" : "inactive",
            "title" : "inactive"
         }
      ],
      "types" : [
         {
            "count" : 23,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 1,
            "id" : "other",
            "title" : "other"
         }
      ]
   },
   "number_of_results" : 24,
   "time_taken" : 3
}
```

## Search strings with spaces

[Elasticsearch query string syntax](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax) will treat words separated by a space as separate parts of a query. It is therefore advisable to surround multi-word search terms of the ROR API with URL-encoded quotation marks.

### Example

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=locations.geonames_details.name:San%20Francisco' | json_pp
```

The ROR API returns no results because the sample advanced query is looking for records whose city is exactly "San", not "San Francisco". The request terminates at the space between "San" and "Francisco".

```json
{
   "items" : [],
   "meta" : {
      "continents" : [],
      "countries" : [],
      "statuses" : [],
      "types" : []
   },
   "number_of_results" : 0,
   "time_taken" : 1
}
```

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=locations.geonames_details.name:%22San%20Francisco%22' | json_pp
```

When the term "San Francisco" is enclosed in URL-encoded quotation marks, the response is a list of research organizations whose city is "San Francisco".

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
         "established" : 1983,
         "external_ids" : [
            {
               "all" : [
                  "grid.446941.a"
               ],
               "preferred" : "grid.446941.a",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/0371hm317",
         "links" : [
            {
               "type" : "website",
               "value" : "http://humanitieswest.net/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Humanities West"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02vdm1p28",
               "label" : "National Endowment for the Humanities",
               "type" : "related"
            }
         ],
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
         "established" : 1975,
         "external_ids" : [
            {
               "all" : [
                  "grid.446428.8"
               ],
               "preferred" : "grid.446428.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 8306 5538"
               ],
               "preferred" : "0000 0004 8306 5538",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/0027wax95",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.calhum.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Cal Humanities"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "California Council for the Humanities"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/02vdm1p28",
               "label" : "National Endowment for the Humanities",
               "type" : "related"
            }
         ],
         "status" : "active",
         "types" : [
            "nonprofit"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2024-07-24",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "medicines360.org"
         ],
         "established" : 2009,
         "external_ids" : [
            {
               "all" : [
                  "0000 0004 6008 5544"
               ],
               "preferred" : "0000 0004 6008 5544",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/02hfps373",
         "links" : [
            {
               "type" : "website",
               "value" : "https://medicines360.org"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "acronym"
               ],
               "value" : "M360"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Medicines 360"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Medicines360"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Meds360"
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
         "established" : 1991,
         "external_ids" : [
            {
               "all" : [
                  "grid.475624.6"
               ],
               "preferred" : "grid.475624.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q17086739"
               ],
               "preferred" : "Q17086739",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/02arxvh93",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.readglobal.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/READ_Global"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "READ"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias",
                  "label",
                  "ror_display"
               ],
               "value" : "READ Global"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Rural Education and Development (READ) Global"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Rural Education and Development Global"
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
               "date" : "2024-08-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "pge.com"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "100022435"
               ],
               "preferred" : "100022435",
               "type" : "fundref"
            },
            {
               "all" : [
                  "0000 0001 1537 0782"
               ],
               "preferred" : "0000 0001 1537 0782",
               "type" : "isni"
            },
            {
               "all" : [
                  "Q1815011"
               ],
               "preferred" : "Q1815011",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05w630g93",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.pge.com"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Pacific_Gas_and_Electric_Company"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "PG&E Corporation"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "PG&E Corporation (United States)"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Pacific Gas and Electric Company"
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
               "date" : "2024-08-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "pinterest.com"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "Q96095585"
               ],
               "preferred" : "Q96095585",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04m6zg706",
         "links" : [
            {
               "type" : "website",
               "value" : "https://pinterest.com"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Cold Brew Labs Inc"
            },
            {
               "lang" : null,
               "types" : [
                  "label"
               ],
               "value" : "Pinterest"
            },
            {
               "lang" : null,
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Pinterest (United States)"
            },
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "Pinterest Inc."
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
               "date" : "2024-08-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "x.ai"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/05febkk55",
         "links" : [
            {
               "type" : "website",
               "value" : "https://x.ai"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/XAI_%28company%29"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "xAI"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "xAI (United States)"
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
               "date" : "2024-08-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "dropbox.com"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "Q15589464"
               ],
               "preferred" : "Q15589464",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03qq6zv19",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.dropbox.com"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Dropbox"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Dropbox (United States)"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Dropbox Inc"
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
               "date" : "2024-08-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "lyft.com"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/01nhtcw46",
         "links" : [
            {
               "type" : "website",
               "value" : "https://lyft.com"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Lyft"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Lyft (United States)"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Lyft Inc"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Lyft, Inc."
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
               "date" : "2024-08-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "airbnb.com"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/03xxf5v12",
         "links" : [
            {
               "type" : "website",
               "value" : "https://airbnb.com"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Airbed & Breakfast"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Airbnb"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Airbnb (United States)"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Airbnb Inc"
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
               "date" : "2024-08-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "gapinc.com"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/03q03zn57",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.gapinc.com"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Gap (United States)"
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
               "date" : "2024-08-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "cohere.com"
         ],
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "Q110363143"
               ],
               "preferred" : "Q110363143",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/050yr0479",
         "links" : [
            {
               "type" : "website",
               "value" : "https://cohere.com"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Cohere"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            },
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "CA",
                  "country_name" : "Canada",
                  "country_subdivision_code" : "ON",
                  "country_subdivision_name" : "Ontario",
                  "lat" : 43.70643,
                  "lng" : -79.39864,
                  "name" : "Toronto"
               },
               "geonames_id" : 6167865
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Cohere"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Cohere (Canada)"
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
               "date" : "2024-08-19",
               "schema_version" : "2.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [
            "databricks.com"
         ],
         "established" : null,
         "external_ids" : [],
         "id" : "https://ror.org/01ynzx943",
         "links" : [
            {
               "type" : "website",
               "value" : "https://databricks.com"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Databricks"
            },
            {
               "lang" : "en",
               "types" : [
                  "label",
                  "ror_display"
               ],
               "value" : "Databricks (United States)"
            },
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "Databricks Inc"
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
         "domains" : [],
         "established" : 2008,
         "external_ids" : [
            {
               "all" : [
                  "100013570"
               ],
               "preferred" : "100013570",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.490819.d"
               ],
               "preferred" : "grid.490819.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 7770 5818"
               ],
               "preferred" : "0000 0004 7770 5818",
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/051bjph87",
         "links" : [
            {
               "type" : "website",
               "value" : "https://strokeshieldfoundation.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Dr. Jeffrey Thomas Stroke Shield Foundation"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "JTSSF"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
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
         "established" : null,
         "external_ids" : [
            {
               "all" : [
                  "grid.428216.b"
               ],
               "preferred" : "grid.428216.b",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9215 0479"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30286910"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05tew9254",
         "links" : [],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Center for Policy Analysis"
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
               "date" : "2021-09-23",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2015,
         "external_ids" : [
            {
               "all" : [
                  "100015442"
               ],
               "preferred" : "100015442",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.512357.7"
               ],
               "preferred" : "grid.512357.7",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/058pagg05",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.gbhi.org/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "GBHI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Global Brain Health Institute"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
            "nonprofit"
         ]
      },
      {
         "admin" : {
            "created" : {
               "date" : "2023-03-16",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 2015,
         "external_ids" : [],
         "id" : "https://ror.org/04b8eax48",
         "links" : [
            {
               "type" : "website",
               "value" : "https://accelerobio.com"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Accelero Biostructures"
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
         "domains" : [],
         "established" : 2000,
         "external_ids" : [
            {
               "all" : [
                  "100011746"
               ],
               "preferred" : "100011746",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.497582.5"
               ],
               "preferred" : "grid.497582.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0393 4319"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5020625"
               ],
               "preferred" : "Q5020625",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04n1n3n22",
         "links" : [
            {
               "type" : "website",
               "value" : "https://qb3.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/California_Institute_for_Quantitative_Biosciences"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : "en",
               "types" : [
                  "alias"
               ],
               "value" : "California Institute for Quantitative Biosciences"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "QB3"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01an7q238",
               "label" : "University of California, Berkeley",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/043mz5j54",
               "label" : "University of California, San Francisco",
               "type" : "parent"
            },
            {
               "id" : "https://ror.org/03s65by71",
               "label" : "University of California, Santa Cruz",
               "type" : "parent"
            }
         ],
         "status" : "active",
         "types" : [
            "funder",
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
         "established" : 2006,
         "external_ids" : [
            {
               "all" : [
                  "grid.480429.3"
               ],
               "preferred" : "grid.480429.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q1390577"
               ],
               "preferred" : "Q1390577",
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04wt43v05",
         "links" : [
            {
               "type" : "website",
               "value" : "https://twitter.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Twitter"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Twitter (United States)"
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
         "domains" : [],
         "established" : 1911,
         "external_ids" : [
            {
               "all" : [
                  "100011879"
               ],
               "preferred" : "100011879",
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.484002.a"
               ],
               "preferred" : "grid.484002.a",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0679 5877"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q5020892"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/0474bqk45",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.cpuc.ca.gov/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/California_Public_Utilities_Commission"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "NA",
                  "continent_name" : "North America",
                  "country_code" : "US",
                  "country_name" : "United States",
                  "country_subdivision_code" : "CA",
                  "country_subdivision_name" : "California",
                  "lat" : 37.77493,
                  "lng" : -122.41942,
                  "name" : "San Francisco"
               },
               "geonames_id" : 5391959
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "CPUC"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "California Public Utilities Commission"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "PUC"
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
            "count" : 419,
            "id" : "na",
            "title" : "North America"
         }
      ],
      "countries" : [
         {
            "count" : 419,
            "id" : "us",
            "title" : "United States"
         },
         {
            "count" : 1,
            "id" : "ca",
            "title" : "Canada"
         }
      ],
      "statuses" : [
         {
            "count" : 419,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 158,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 145,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 95,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 34,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 30,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 17,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 12,
            "id" : "archive",
            "title" : "archive"
         },
         {
            "count" : 12,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 11,
            "id" : "government",
            "title" : "government"
         }
      ]
   },
   "number_of_results" : 419,
   "time_taken" : 3
}
```

## Uppercase and lowercase

Currently, the advanced query parameter is case-sensitive, meaning that the same search may return different results depending on the letter case of the input string. For best results, capitalize location names (_Panama_ not _panama_). We [may change this behavior in future](https://github.com/ror-community/ror-roadmap/issues/175) to support case insensitivity for all searches.

### Example

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=locations.geonames_details.name:berlin' | json_pp
```

The ROR API returns no results because the sample advanced query is looking for records whose city is exactly _berlin_.

```json
{
   "items" : [],
   "meta" : {
      "continents" : [],
      "countries" : [],
      "statuses" : [],
      "types" : []
   },
   "number_of_results" : 0,
   "time_taken" : 5
}
```

Capitalizing the name of the city returns better results.

```curl
curl 'https://api.ror.org/v2/organizations?query.advanced=locations.geonames_details.name:Berlin' | json_pp
```

The response includes all records whose city contains the capitalized term _Berlin_.

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
         "established" : 1994,
         "external_ids" : [
            {
               "all" : [
                  "grid.414814.c"
               ],
               "preferred" : "grid.414814.c",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0473 3103"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/01sm4s887",
         "links" : [],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BgVV"
            },
            {
               "lang" : "de",
               "types" : [
                  "label"
               ],
               "value" : "Bundesinstitut für gesundheitlichen Verbraucherschutz und Veterinärmedizin"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Federal Institute for Health Protection of Consumers and Veterinary Medicine"
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
         "established" : 1874,
         "external_ids" : [
            {
               "all" : [
                  "grid.415085.d"
               ],
               "preferred" : "grid.415085.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q1774663"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03zzvtn22",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.vivantes.de/klinikum-im-friedrichshain/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "KFH"
            },
            {
               "lang" : "de",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Klinikum im Friedrichshain"
            },
            {
               "lang" : "de",
               "types" : [
                  "alias"
               ],
               "value" : "Städtische Krankenhaus Am Friedrichshain"
            },
            {
               "lang" : "de",
               "types" : [
                  "alias"
               ],
               "value" : "Vivantes Klinikum im Friedrichshain"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "healthcare"
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
         "established" : 2003,
         "external_ids" : [
            {
               "all" : [
                  "grid.417855.a"
               ],
               "preferred" : "grid.417855.a",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/02z95d405",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.ikfeberlin.de/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IKFE"
            },
            {
               "lang" : "de",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Institut für Klinische Forschung und Entwicklung"
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
         "established" : 1929,
         "external_ids" : [
            {
               "all" : [
                  "grid.417953.d"
               ],
               "preferred" : "grid.417953.d",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0560 5172"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/03b2h3x98",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.pgdiakonie.de/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : "de",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Paul Gerhardt Diakonie"
            },
            {
               "lang" : "de",
               "types" : [
                  "alias"
               ],
               "value" : "Verein zur Errichtung Evangelischer Krankenhäuser"
            },
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "VzE"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/05q4r1796",
               "label" : "Evangelische Lungenklinik Berlin",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00xv9sn23",
               "label" : "Martin-Luther-Krankenhaus",
               "type" : "child"
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
               "date" : "2018-11-14",
               "schema_version" : "1.0"
            },
            "last_modified" : {
               "date" : "2024-12-11",
               "schema_version" : "2.1"
            }
         },
         "domains" : [],
         "established" : 1911,
         "external_ids" : [
            {
               "all" : [
                  "grid.418028.7"
               ],
               "preferred" : "grid.418028.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0565 1775"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q514590"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/03k9qs827",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.fhi-berlin.mpg.de/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Fritz_Haber_Institute_of_the_Max_Planck_Society"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "FHI"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Fritz Haber Institute of the Max Planck Society"
            },
            {
               "lang" : "de",
               "types" : [
                  "label"
               ],
               "value" : "Fritz-Haber-Institut der Max-Planck-Gesellschaft"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01hhn8329",
               "label" : "Max Planck Society",
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
         "established" : 1988,
         "external_ids" : [
            {
               "all" : [
                  "grid.418217.9"
               ],
               "preferred" : "grid.418217.9",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0000 9323 8675"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/00shv0x82",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.drfz.de/en/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "DRFZ"
            },
            {
               "lang" : "de",
               "types" : [
                  "label"
               ],
               "value" : "Deutsches Rheuma-Forschungszentrum Berlin"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "German Rheumatism Research Centre"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/01n6r0e97",
               "label" : "Leibniz Association",
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
         "established" : 1994,
         "external_ids" : [
            {
               "all" : [
                  "100010101"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.418468.7"
               ],
               "preferred" : "grid.418468.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 0549 9953"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/04fjkxc67",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.helios-kliniken.de/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : "de",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Helios Kliniken"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/0010c1z81",
               "label" : "Deutsche Klinik für Diagnostik",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04ncnj403",
               "label" : "HELIOS Albert-Schweitzer-Klinik Northeim",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02av38n71",
               "label" : "HELIOS St. Elisabeth Klinik Oberhausen",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00dvqrz49",
               "label" : "Helios Amper-Klinikum Dachau",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/03kxagd85",
               "label" : "Helios Dr. Horst Schmidt Kliniken Wiesbaden",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/040gtvq30",
               "label" : "Helios Endo-Klinik Hamburg",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/028v8ft65",
               "label" : "Helios Hospital Bad Saarow",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05hgh1g19",
               "label" : "Helios Hospital Berlin-Buch",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/018gc9r78",
               "label" : "Helios Hospital Schwerin",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00jp3t114",
               "label" : "Helios Hospital Siegburg",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/019wxyf39",
               "label" : "Helios Klinik Hagen Ambrock",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00g63xj11",
               "label" : "Helios Klinik Kipfenberg",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04sr0hc76",
               "label" : "Helios Kliniken Mittelweser",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/00td6v066",
               "label" : "Helios Klinikum Emil von Behring",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/04y18m106",
               "label" : "Helios Klinikum Erfurt",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/01be19w37",
               "label" : "Helios Klinikum Krefeld",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02q7ym472",
               "label" : "Helios Park-Klinikum Leipzig",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/05ktfmr66",
               "label" : "Helios Vogtland Klinikum Plauen",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/029cqm867",
               "label" : "Hospital Krefeld-Düsseldorf",
               "type" : "child"
            },
            {
               "id" : "https://ror.org/02kj91m96",
               "label" : "Leipzig Heart Institute",
               "type" : "child"
            }
         ],
         "status" : "active",
         "types" : [
            "funder",
            "healthcare"
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
         "established" : 1920,
         "external_ids" : [
            {
               "all" : [
                  "501100001657"
               ],
               "preferred" : null,
               "type" : "fundref"
            },
            {
               "all" : [
                  "grid.428580.3"
               ],
               "preferred" : "grid.428580.3",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 5907 2797"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/05rg3k287",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.mathunion.org/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/International_Mathematical_Union"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "IMU"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "International Mathematical Union"
            }
         ],
         "relationships" : [],
         "status" : "active",
         "types" : [
            "funder",
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
         "established" : 1997,
         "external_ids" : [
            {
               "all" : [
                  "grid.429831.5"
               ],
               "preferred" : "grid.429831.5",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q13824461"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04g007714",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.noxxon.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Noxxon_Pharma"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Noxxon Pharma (Germany)"
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
         "domains" : [],
         "established" : 1749,
         "external_ids" : [
            {
               "all" : [
                  "grid.431130.4"
               ],
               "preferred" : "grid.431130.4",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0552 5607"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q98818"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/026xdv168",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.degruyter.com/"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Walter_de_Gruyter"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "alias"
               ],
               "value" : "Verlag Walter de Gruyter"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Walter de Gruyter (Germany)"
            },
            {
               "lang" : "de",
               "types" : [
                  "label"
               ],
               "value" : "ˈɡʁɔʏ̯tɐ"
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
         "domains" : [],
         "established" : 2003,
         "external_ids" : [
            {
               "all" : [
                  "grid.431881.7"
               ],
               "preferred" : "grid.431881.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30289740"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/04b8jke36",
         "links" : [
            {
               "type" : "website",
               "value" : "http://enablingmnt.com/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "enablingMNT (Germany)"
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
         "domains" : [],
         "established" : 2007,
         "external_ids" : [
            {
               "all" : [
                  "grid.431916.8"
               ],
               "preferred" : "grid.431916.8",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0533 937X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30289768"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/015rere91",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.atlas-biolabs.com/home"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Atlas Biolabs (Germany)"
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
         "domains" : [],
         "established" : 2004,
         "external_ids" : [
            {
               "all" : [
                  "grid.432214.2"
               ],
               "preferred" : "grid.432214.2",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30255031"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/007422545",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.activespacetech.com"
            },
            {
               "type" : "wikipedia",
               "value" : "https://en.wikipedia.org/wiki/Active_Space_Technologies"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Active Space Technologies (Germany)"
            }
         ],
         "relationships" : [
            {
               "id" : "https://ror.org/052ay3140",
               "label" : "Active Space Technologies (Portugal)",
               "type" : "parent"
            }
         ],
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
         "domains" : [],
         "established" : 2012,
         "external_ids" : [
            {
               "all" : [
                  "grid.432220.5"
               ],
               "preferred" : "grid.432220.5",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/04hypbt36",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.bisdn.de/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "BISDN (Germany)"
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
         "domains" : [],
         "established" : 1949,
         "external_ids" : [
            {
               "all" : [
                  "grid.432269.8"
               ],
               "preferred" : "grid.432269.8",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/026a1h106",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.vku.de/datenschutz.html"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "VKU"
            },
            {
               "lang" : "de",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Verband kommunaler Unternehmen"
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
         "established" : 1996,
         "external_ids" : [
            {
               "all" : [
                  "grid.432428.b"
               ],
               "preferred" : "grid.432428.b",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/05xr55z04",
         "links" : [
            {
               "type" : "website",
               "value" : "https://www.amic-berlin.de/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Angewandte Micro Messtechnik (Germany)"
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
         "domains" : [],
         "established" : 1992,
         "external_ids" : [
            {
               "all" : [
                  "grid.432725.5"
               ],
               "preferred" : "grid.432725.5",
               "type" : "grid"
            }
         ],
         "id" : "https://ror.org/05ey8y327",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.berliner-e-agentur.de/en"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BEA"
            },
            {
               "lang" : "en",
               "types" : [
                  "label"
               ],
               "value" : "Berlin Energy Agency"
            },
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Berliner Energieagentur (Germany)"
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
         "domains" : [],
         "established" : 1983,
         "external_ids" : [
            {
               "all" : [
                  "grid.432726.6"
               ],
               "preferred" : "grid.432726.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0004 0561 6760"
               ],
               "preferred" : null,
               "type" : "isni"
            }
         ],
         "id" : "https://ror.org/0043ncq27",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.bis-berlin.de/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "BIS"
            },
            {
               "lang" : "de",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Berliner Institut für Sozialforschung"
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
         "established" : 1978,
         "external_ids" : [
            {
               "all" : [
                  "grid.432740.6"
               ],
               "preferred" : "grid.432740.6",
               "type" : "grid"
            },
            {
               "all" : [
                  "0000 0001 2230 533X"
               ],
               "preferred" : null,
               "type" : "isni"
            },
            {
               "all" : [
                  "Q30255419"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/01dxs6p42",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.emz-berlin.de"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "acronym"
               ],
               "value" : "EMZ"
            },
            {
               "lang" : "en",
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "European Migration Centre"
            },
            {
               "lang" : "de",
               "types" : [
                  "label"
               ],
               "value" : "Europäisches Migrationszentrum"
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
                  "grid.432750.7"
               ],
               "preferred" : "grid.432750.7",
               "type" : "grid"
            },
            {
               "all" : [
                  "Q30255427"
               ],
               "preferred" : null,
               "type" : "wikidata"
            }
         ],
         "id" : "https://ror.org/05qzdw571",
         "links" : [
            {
               "type" : "website",
               "value" : "http://www.agentscape.de/"
            }
         ],
         "locations" : [
            {
               "geonames_details" : {
                  "continent_code" : "EU",
                  "continent_name" : "Europe",
                  "country_code" : "DE",
                  "country_name" : "Germany",
                  "country_subdivision_code" : "BE",
                  "country_subdivision_name" : "Berlin",
                  "lat" : 52.52437,
                  "lng" : 13.41053,
                  "name" : "Berlin"
               },
               "geonames_id" : 2950159
            }
         ],
         "names" : [
            {
               "lang" : null,
               "types" : [
                  "ror_display",
                  "label"
               ],
               "value" : "Agentscape (Germany)"
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
            "count" : 588,
            "id" : "eu",
            "title" : "Europe"
         },
         {
            "count" : 4,
            "id" : "na",
            "title" : "North America"
         }
      ],
      "countries" : [
         {
            "count" : 588,
            "id" : "de",
            "title" : "Germany"
         },
         {
            "count" : 4,
            "id" : "us",
            "title" : "United States"
         }
      ],
      "statuses" : [
         {
            "count" : 592,
            "id" : "active",
            "title" : "active"
         }
      ],
      "types" : [
         {
            "count" : 157,
            "id" : "company",
            "title" : "company"
         },
         {
            "count" : 107,
            "id" : "other",
            "title" : "other"
         },
         {
            "count" : 95,
            "id" : "facility",
            "title" : "facility"
         },
         {
            "count" : 93,
            "id" : "nonprofit",
            "title" : "nonprofit"
         },
         {
            "count" : 66,
            "id" : "funder",
            "title" : "funder"
         },
         {
            "count" : 49,
            "id" : "education",
            "title" : "education"
         },
         {
            "count" : 40,
            "id" : "healthcare",
            "title" : "healthcare"
         },
         {
            "count" : 34,
            "id" : "government",
            "title" : "government"
         },
         {
            "count" : 17,
            "id" : "archive",
            "title" : "archive"
         }
      ]
   },
   "number_of_results" : 592,
   "time_taken" : 21
}
```

<br />
