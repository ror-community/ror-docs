---
title: Affiliation parameter
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR API affiliation parameter
  description: >-
    Instructions for using the affiliation parameter of the ROR API to match
    text strings to ROR IDs.
  robots: index
next:
  pages:
    - title: ROR blog posts on matching
      type: link
      url: http://ror.org/categories/matching
    - title: ror-utilities matching scripts
      type: link
      url: https://github.com/ror-community/ror-utilities
---
> 👍 ROR REST API v2
>
> This page documents v2 of the ROR REST API. For v1 documentation of the ROR REST API, see [https://ror.readme.io/v1/docs/api-affiliation](https://ror.readme.io/v1/docs/api-affiliation). You can also read more about ROR [API versions](doc:api-versions) and a summary of what's new in [Schema 2.0](doc:schema-v2) and [Schema 2.1](doc:schema-2-1).

> ❗️ Version 1 of the ROR schema and API will be sunset in December 2025
>
> In December 2025, version 1 of the ROR schema and API will be sunset, meaning that ROR API requests with v1 in the path will no longer return a response, v1 files will no longer be included in the ROR data dump, and v1 documentation will no longer be available. Read more in our [changelog](https://ror.readme.io/changelog/2025-07-01-sunset-of-version-1#/).

# About the affiliation parameter

For many years, publishers of scholarly information often captured author and contributor affiliation information as unstructured data, sometimes storing an organization's name, a sub-unit or department's name, a street address, and a geographical location along with copious irregular punctuation as a text string in a single field. Some affiliation strings even include multiple affiliations.

The affiliation parameter of the ROR API is designed to help match these messy text strings to ROR records to produce cleaner affiliation data. The affiliation service attempts to find the ROR record that is the most probable match for the given affiliation string; if it finds a likely candidate, it returns that result with a `chosen:true` value. Additional possibilities that might match the string are also included in results, listed in descending order by confidence `score`.

An API-based approach to matching affiliation strings to ROR IDs can work well for large-scale systems where human review of every proposed match is impractical, but no large-scale programmatic approach to matching is perfect, especially since there are many similar and even identical names and acronyms among research organizations globally. Often, the matching service will not be able to suggest a match for a particular string, and in some cases, the matching service might suggest an incorrect match. Human review is always the best fallback.

> 📘 Consider a different method of matching if you have structured organization data
>
> You can also match organization data to ROR IDs using the ?query and ?query.advanced parameter with filters and field-specific queries. If your data is structured such that it separately stores an organization's name, city, country, website, and organizational identifiers such as GRID, Wikidata, or Funder IDs, we recommend that you use the [?query parameter](https://ror.readme.io/docs/api-query) or [?query.advanced parameter](https://ror.readme.io/docs/api-advanced-query) of the ROR API to match your data to ROR.

<br />

<br />

> 📘 Affiliation parameter format
>
> `https://api.ror.org/v2/organizations?affiliation=[URL-encoded-string]`

# Formatting searches

All request strings must be [URL-encoded](https://www.w3schools.com/tags/ref_urlencode.asp). The affiliation parameter is specifically designed to handle strings with punctuation, special characters, and spaces, so **it is not necessary to enclose multi-term search strings in quotation marks or to escape special characters**.

# Paging and filtering

The affiliation parameter **does not accept filters** and results **are not paginated**. When filter syntax is added to the end of an affiliation search, the terms will be treated as part of the affiliation search. All results will be returned, not just the first 20, since this approach typically returns a small number of results. Results are listed in descending order by matching confidence score.

> 🚧 Be aware of differences between the affiliation parameter and the query parameters
>
> Unlike the query and advanced query parameters, the affiliation parameter does not accept filters, and results are not paginated -- all results will be returned, not just the first 20. When filter syntax is added to the end of an affiliation search, the terms will be treated as part of the affiliation search.
>
> Also unlike the query and advanced query parameters, the affiliation parameter expects multi-word strings that include spaces, punctuation, and special characters. Surrounding terms in quotation marks or escaping special characters can produce worse results when using the affiliation parameter.

# Matching long, messy text strings to ROR records

If you have messy, unstructured text data that includes organization names, you can use the affiliation parameter to look for ROR records that match the organization names buried within those text strings. One such messy text string might be "Department of Civil and Industrial Engineering, University of Pisa, Largo Lucio Lazzarino 2, Pisa 56126, Italy".

## Example

```curl
curl 'https://api.ror.org/v2/organizations?affiliation=Department%20of%20Civil%20and%20Industrial%20Engineering%2C%20University%20of%20Pisa%2C%20Largo%20Lucio%20Lazzarino%202%2C%20Pisa%2056126%2C%20Italy' | json_pp
```

The response is a JSON object that contains an array of items with the following fields:

* `organization`: ROR record for matched organization

* `substring`: substring of the supplied string that generated this match

* `score`: matching confidence score, with values between .5 and 1 (inclusive)

* `chosen`: binary indicator of whether the score is high enough to consider the organization correctly matched

* `matching_type`: type of matching algorithm applied in this case, possible values:
  * `PHRASE`: the entire phrase matched to a variant of the organization's name
  * `COMMON TERMS`: the matching was done by comparing the words separately
  * `FUZZY`: the matching was done by fuzzy-comparing the words separately
  * `HEURISTICS`: "University of X" was matched to "X University"
  * `ACRONYM`: matched by acronym
  * `EXACT`: exact match of the entered string in name values in the `names` field excluding acronyms

The first item in the array, the ROR record for the University of Pisa, has a `chosen` value of _true_, indicating that the affiliation service considers this record a sufficiently likely match to the text string. **Not all affiliation searches will produce a "chosen" result.**

The `matching_type` is given as _"COMMON TERMS"_, indicating the method by which the affiliation parameter chose the matching record. The confidence `score` is 1, the highest possible level of confidence in the match. **Only results with a score of at least .5 are returned.** Results are listed in descending order by matching confidence score.

The substring used to find the match in this case is "Department of Civil and Industrial Engineering University of Pisa Largo Lucio Lazzarino 2 Pisa Italy", or the entire text content of the entered string excluding the numeric postcode.

```json
{
   "items" : [
      {
         "chosen" : true,
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
               "unipi.it"
            ],
            "established" : 1343,
            "external_ids" : [
               {
                  "all" : [
                     "501100007514"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.5395.a"
                  ],
                  "preferred" : "grid.5395.a",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 1757 3729"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q645663"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/03ad39j10",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.unipi.it"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/University_of_Pisa"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "IT",
                     "country_name" : "Italy",
                     "country_subdivision_code" : "52",
                     "country_subdivision_name" : "Tuscany",
                     "lat" : 43.70853,
                     "lng" : 10.4036,
                     "name" : "Pisa"
                  },
                  "geonames_id" : 3170647
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "UniPi"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "University of Pisa"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "label"
                  ],
                  "value" : "Università di Pisa"
               },
               {
                  "lang" : "de",
                  "types" : [
                     "label"
                  ],
                  "value" : "Universität Pisa"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "label"
                  ],
                  "value" : "Université de Pise"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/00mc91w09",
                  "label" : "Ospedale Cisanello",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 1,
         "substring" : "Department of Civil and Industrial Engineering University of Pisa Largo Lucio Lazzarino 2 Pisa  Italy"
      },
      {
         "chosen" : false,
         "matching_type" : "FUZZY",
         "organization" : {
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
               "unisalento.it"
            ],
            "established" : 1955,
            "external_ids" : [
               {
                  "all" : [
                     "501100005728",
                     "501100005729",
                     "501100006195"
                  ],
                  "preferred" : "501100005728",
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.9906.6"
                  ],
                  "preferred" : "grid.9906.6",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 2289 7785"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q1230902"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/03fc1k060",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://unisalento.it"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/University_of_Salento"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "IT",
                     "country_name" : "Italy",
                     "country_subdivision_code" : "75",
                     "country_subdivision_name" : "Apulia",
                     "lat" : 40.35481,
                     "lng" : 18.17244,
                     "name" : "Lecce"
                  },
                  "geonames_id" : 3174953
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "University of Salento"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "label"
                  ],
                  "value" : "Università degli Studi di Lecce"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Università del Salento"
               },
               {
                  "lang" : "de",
                  "types" : [
                     "label"
                  ],
                  "value" : "Universität Salento"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "label"
                  ],
                  "value" : "Université du salento"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/00qrf6g60",
                  "label" : "Istituto Nazionale di Fisica Nucleare, Sezione di Lecce",
                  "type" : "child"
               }
            ],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 0.82,
         "substring" : "University of Pisa"
      },
      {
         "chosen" : false,
         "matching_type" : "HEURISTICS",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2024-09-14",
                  "schema_version" : "2.0"
               },
               "last_modified" : {
                  "date" : "2024-12-11",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [
               "liceodini.it"
            ],
            "established" : 1924,
            "external_ids" : [
               {
                  "all" : [
                     "Q30889474"
                  ],
                  "preferred" : "Q30889474",
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/006xg2x43",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.liceodini.it"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://it.wikipedia.org/wiki/Liceo_scientifico_statale_Ulisse_Dini"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "IT",
                     "country_name" : "Italy",
                     "country_subdivision_code" : "52",
                     "country_subdivision_name" : "Tuscany",
                     "lat" : 43.70853,
                     "lng" : 10.4036,
                     "name" : "Pisa"
                  },
                  "geonames_id" : 3170647
               }
            ],
            "names" : [
               {
                  "lang" : "it",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Liceo Dini"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Liceo Scientifico \"Ulisse Dini\""
               },
               {
                  "lang" : "it",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Liceo Scientifico 'Ulisse Dini' - Pisa"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Liceo Scientifico Ulisse Dini"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Liceo scientifico statale Ulisse Dini"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "alias"
                  ],
                  "value" : "U. Dini"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Ulisse Dini Scientific High School"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.8,
         "substring" : "Pisa University"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
                     "grid.144189.1"
                  ],
                  "preferred" : "grid.144189.1",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 1756 8209"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/05xrcj819",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.ao-pisa.toscana.it/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "IT",
                     "country_name" : "Italy",
                     "country_subdivision_code" : "52",
                     "country_subdivision_name" : "Tuscany",
                     "lat" : 43.70853,
                     "lng" : 10.4036,
                     "name" : "Pisa"
                  },
                  "geonames_id" : 3170647
               }
            ],
            "names" : [
               {
                  "lang" : "it",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Azienda Ospedaliera Universitaria Pisana"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "University Hospital of Pisa"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/00mc91w09",
                  "label" : "Ospedale Cisanello",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04069k268",
                  "label" : "ERN ReCONNET",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "healthcare"
            ]
         },
         "score" : 0.8,
         "substring" : "University of Pisa"
      },
      {
         "chosen" : false,
         "matching_type" : "FUZZY",
         "organization" : {
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
               "unitn.it"
            ],
            "established" : 1962,
            "external_ids" : [
               {
                  "all" : [
                     "501100004004"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.11696.39"
                  ],
                  "preferred" : "grid.11696.39",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 1937 0351"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q930528"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/05trd4x28",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://unitn.it"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/University_of_Trento"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "IT",
                     "country_name" : "Italy",
                     "country_subdivision_code" : "32",
                     "country_subdivision_name" : "Trentino-Alto Adige",
                     "lat" : 46.06787,
                     "lng" : 11.12108,
                     "name" : "Trento"
                  },
                  "geonames_id" : 3165243
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "University of Trento"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "label"
                  ],
                  "value" : "Università degli Studi di Trento"
               },
               {
                  "lang" : "de",
                  "types" : [
                     "label"
                  ],
                  "value" : "Universität Trient"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "label"
                  ],
                  "value" : "Université de Trente"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/04kcgyj43",
                  "label" : "The Microsoft Research - University of Trento Centre for Computational and Systems Biology",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00nhs3j29",
                  "label" : "Istituto Nazionale di Fisica Nucleare, Trento Institute for Fundamental Physics And Applications",
                  "type" : "child"
               }
            ],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 0.74,
         "substring" : "University of Pisa"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
               "pi.infn.it"
            ],
            "established" : null,
            "external_ids" : [
               {
                  "all" : [
                     "grid.470216.6"
                  ],
                  "preferred" : "grid.470216.6",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "Q30265297"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/05symbg58",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.pi.infn.it"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "IT",
                     "country_name" : "Italy",
                     "country_subdivision_code" : "52",
                     "country_subdivision_name" : "Tuscany",
                     "lat" : 43.70853,
                     "lng" : 10.4036,
                     "name" : "Pisa"
                  },
                  "geonames_id" : 3170647
               }
            ],
            "names" : [
               {
                  "lang" : "it",
                  "types" : [
                     "alias"
                  ],
                  "value" : "INFN Pisa"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "INFN Pisa Division"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "INFN Pisa Unit"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "alias"
                  ],
                  "value" : "INFN Sezione di Pisa"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "INFN-PI"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Istituto Nazionale di Fisica Nucleare, Sezione di Pisa"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "National Institute for Nuclear Physics, Pisa Division"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/005ta0471",
                  "label" : "Istituto Nazionale di Fisica Nucleare",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/00x27da85",
                  "label" : "University of Perugia",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/02w0r2764",
                  "label" : "MAGIC Telescopes",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "facility"
            ]
         },
         "score" : 0.71,
         "substring" : "Department of Civil and Industrial Engineering University of Pisa Largo Lucio Lazzarino 2 Pisa  Italy"
      },
      {
         "chosen" : false,
         "matching_type" : "FUZZY",
         "organization" : {
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
               "univr.it"
            ],
            "established" : 1982,
            "external_ids" : [
               {
                  "all" : [
                     "501100007052"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.5611.3"
                  ],
                  "preferred" : "grid.5611.3",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 1763 1124"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q2531629"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/039bp8j42",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.univr.it"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/University_of_Verona"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "IT",
                     "country_name" : "Italy",
                     "country_subdivision_code" : "34",
                     "country_subdivision_name" : "Veneto",
                     "lat" : 45.43854,
                     "lng" : 10.9938,
                     "name" : "Verona"
                  },
                  "geonames_id" : 3164527
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "University of Verona"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "label"
                  ],
                  "value" : "Università degli Studi di Verona"
               },
               {
                  "lang" : "de",
                  "types" : [
                     "label"
                  ],
                  "value" : "Universität Verona"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "label"
                  ],
                  "value" : "Université de vérone"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 0.68,
         "substring" : "University of Pisa"
      },
      {
         "chosen" : false,
         "matching_type" : "FUZZY",
         "organization" : {
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
            "established" : 1982,
            "external_ids" : [
               {
                  "all" : [
                     "100012783"
                  ],
                  "preferred" : "100012783",
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.425554.7"
                  ],
                  "preferred" : "grid.425554.7",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 1773 7551"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/050xp5d36",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.protezionecivile.gov.it/jcms/en/homepage.wp"
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
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Civil Protection Department"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Dipartimento della Protezione Civile"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "funder",
               "government"
            ]
         },
         "score" : 0.58,
         "substring" : "Department of Civil and Industrial Engineering"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
            "external_ids" : [
               {
                  "all" : [
                     "0000 0004 1758 7813"
                  ],
                  "preferred" : "0000 0004 1758 7813",
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/00vfm5970",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.pi.ingv.it"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "IT",
                     "country_name" : "Italy",
                     "country_subdivision_code" : "52",
                     "country_subdivision_name" : "Tuscany",
                     "lat" : 43.70853,
                     "lng" : 10.4036,
                     "name" : "Pisa"
                  },
                  "geonames_id" : 3170647
               }
            ],
            "names" : [
               {
                  "lang" : "it",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "INGV Sezione di Pisa"
               },
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "INGV-PI"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Istituto Nazionale di Geofisica e Vulcanologia Sezione di Pisa"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "National Institute of Geophysics and Volcanology, Pisa Section"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/00qps9a02",
                  "label" : "Istituto Nazionale di Geofisica e Vulcanologia",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "facility"
            ]
         },
         "score" : 0.55,
         "substring" : "Department of Civil and Industrial Engineering University of Pisa Largo Lucio Lazzarino 2 Pisa  Italy"
      }
   ],
   "number_of_results" : 9
}
```

> 📘 Is it a match?
>
> When using affiliation matching, look for results with `"chosen": true` if you want a simple approach to choosing the best match. When there's no result with `"chosen": true` it can mean either that there are no particularly good matches or that there are several good matches but no clear winner. If there is no result with `"chosen": true`, some additional review (either by humans or machines) is almost always needed.

## Example

An affiliation string such as "UCL School of Slavonic and East European Studies" does not contain enough identifying information for the ROR API affiliation matching service to choose a matching ROR record.

```curl
curl 'https://api.ror.org/v2/organizations?affiliation=UCL%20School%20of%20Slavonic%20and%20East%20European%20Studies' | json_pp
```

The response returns 11 results with confidence scores ranging from .7 to .54 listed in descending order, but the affiliation matching service does not rate any of these results as a recommended match.

```json
{
   "items" : [
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
                     "501100000687"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.470732.5"
                  ],
                  "preferred" : "grid.470732.5",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 1958 8607"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/02xxmjs88",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.basees.org/"
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
                     "lat" : 53.79648,
                     "lng" : -1.54785,
                     "name" : "Leeds"
                  },
                  "geonames_id" : 2644688
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "BASEES"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "British Association for Slavonic and East European Studies"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "funder",
               "other"
            ]
         },
         "score" : 0.7,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
            "established" : 1948,
            "external_ids" : [
               {
                  "all" : [
                     "100005346"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.446841.b"
                  ],
                  "preferred" : "grid.446841.b",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0000 8678 3376"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/04n3hjx90",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://aseees.org/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Association_for_Slavic,_East_European,_and_Eurasian_Studies"
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
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "ASEEES"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "American Association for the Advancement of Slavic Studies"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Association for Slavic East European and Eurasian Studies"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "funder",
               "nonprofit"
            ]
         },
         "score" : 0.67,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
                     "100009050"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.458378.5"
                  ],
                  "preferred" : "grid.458378.5",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 5900 8576"
                  ],
                  "preferred" : "0000 0004 5900 8576",
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/055mqk127",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://ostersjostiftelsen.se/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "SE",
                     "country_name" : "Sweden",
                     "country_subdivision_code" : "AB",
                     "country_subdivision_name" : "Stockholm",
                     "lat" : 59.23705,
                     "lng" : 17.98192,
                     "name" : "Huddinge"
                  },
                  "geonames_id" : 2704620
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Foundation for Baltic and East European Studies"
               },
               {
                  "lang" : "sv",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Östersjöstiftelsen"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "facility",
               "funder"
            ]
         },
         "score" : 0.67,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
            "established" : 2016,
            "external_ids" : [
               {
                  "all" : [
                     "grid.507643.3"
                  ],
                  "preferred" : "grid.507643.3",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 8023 4156"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q61781089"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/00a0w0523",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.zois-berlin.de/"
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
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Centre for East European and International Studies"
               },
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "ZOiS"
               },
               {
                  "lang" : "de",
                  "types" : [
                     "label"
                  ],
                  "value" : "Zentrum für Osteuropa- und internationale Studien"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.65,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
               "slu.cas.cz"
            ],
            "established" : 1922,
            "external_ids" : [
               {
                  "all" : [
                     "grid.448289.b"
                  ],
                  "preferred" : "grid.448289.b",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 0428 7272"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q12153738"
                  ],
                  "preferred" : "Q12153738",
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/03qq0n189",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.slu.cas.cz"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "CZ",
                     "country_name" : "Czechia",
                     "country_subdivision_code" : "10",
                     "country_subdivision_name" : "Prague",
                     "lat" : 50.08804,
                     "lng" : 14.42076,
                     "name" : "Prague"
                  },
                  "geonames_id" : 3067696
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Czech Acad Sci, Inst Slav Studies"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Czech Academy of Sciences, Institute of Slavonic Studies"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Institute of Slavonic Studies CAS"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Institute of Slavonic Studies of the Czech Academy of Sciences"
               },
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "SLÚ AV ČR"
               },
               {
                  "lang" : "cs",
                  "types" : [
                     "label"
                  ],
                  "value" : "Slovanský ústav AV ČR"
               },
               {
                  "lang" : "cs",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Slovanský ústav AV ČR, v. v. i."
               },
               {
                  "lang" : "cs",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Slovanský ústav AV ČR, veřejná výzkumná instituce"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/053avzc18",
                  "label" : "Czech Academy of Sciences",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "facility"
            ]
         },
         "score" : 0.64,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "FUZZY",
         "organization" : {
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
               "vsers.cz"
            ],
            "established" : 2003,
            "external_ids" : [
               {
                  "all" : [
                     "grid.486536.a"
                  ],
                  "preferred" : "grid.486536.a",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0000 9364 6708"
                  ],
                  "preferred" : "0000 0000 9364 6708",
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q8310981"
                  ],
                  "preferred" : "Q8310981",
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/040qrz805",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://vsers.cz/"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "CZ",
                     "country_name" : "Czechia",
                     "country_subdivision_code" : "31",
                     "country_subdivision_name" : "Jihočeský kraj",
                     "lat" : 48.97447,
                     "lng" : 14.47434,
                     "name" : "České Budějovice"
                  },
                  "geonames_id" : 3077916
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "College of European and Regional Studies"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "The College of European and Regional Studies"
               },
               {
                  "lang" : "cs",
                  "types" : [
                     "label"
                  ],
                  "value" : "Vysoká škola Evropských a Regionálních Studií"
               },
               {
                  "lang" : "cs",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Vysoká škola evropských a regionálních studií"
               },
               {
                  "lang" : "cs",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Vysoká škola evropských a regionálních studií, z. ú."
               },
               {
                  "lang" : "cs",
                  "types" : [
                     "acronym"
                  ],
                  "value" : "VŠERS"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.64,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "FUZZY",
         "organization" : {
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
            "established" : null,
            "external_ids" : [],
            "id" : "https://ror.org/007bytg49",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.acadsudest.ro"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "RO",
                     "country_name" : "Romania",
                     "country_subdivision_code" : "B",
                     "country_subdivision_name" : "București",
                     "lat" : 44.43225,
                     "lng" : 26.10626,
                     "name" : "Bucharest"
                  },
                  "geonames_id" : 683506
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Institute for South-East European Studies"
               },
               {
                  "lang" : "ro",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Institutul de Studii Sud-Est Europene"
               },
               {
                  "lang" : "ro",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Institutul de Studii Sud-Est Europene al Academiei Române"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "The Romanian Academy’s Institute for South-East European Studies"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/0561n6946",
                  "label" : "Romanian Academy",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "facility"
            ]
         },
         "score" : 0.63,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
                     "501100019860"
                  ],
                  "preferred" : "501100019860",
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.462368.d"
                  ],
                  "preferred" : "grid.462368.d",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "Q1664940"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/039s64n79",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.ios-regensburg.de/en"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "DE",
                     "country_name" : "Germany",
                     "country_subdivision_code" : "BY",
                     "country_subdivision_name" : "Bavaria",
                     "lat" : 49.01513,
                     "lng" : 12.10161,
                     "name" : "Regensburg"
                  },
                  "geonames_id" : 2849483
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "IOS"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Leibniz Institute for East and Southeast European Studies"
               },
               {
                  "lang" : "de",
                  "types" : [
                     "label"
                  ],
                  "value" : "Leibniz-Institut für Ost- und Südosteuropaforschung"
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
               "facility",
               "funder"
            ]
         },
         "score" : 0.63,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
                     "grid.445206.1"
                  ],
                  "preferred" : "grid.445206.1",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "Q12788728"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/04jj9d877",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://fds.nova-uni.si"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "SI",
                     "country_name" : "Slovenia",
                     "country_subdivision_code" : "052",
                     "country_subdivision_name" : "Kranj",
                     "lat" : 46.23887,
                     "lng" : 14.35561,
                     "name" : "Kranj"
                  },
                  "geonames_id" : 3197378
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "FSS"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Graduate School of Government and European Studies"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.61,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
               "esmt.org"
            ],
            "established" : 2002,
            "external_ids" : [
               {
                  "all" : [
                     "grid.434239.b"
                  ],
                  "preferred" : "grid.434239.b",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 2288 2583"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q569528"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/024xejm70",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://esmt.berlin"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/European_School_of_Management_and_Technology"
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
                  "value" : "ESMT"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "European School of Management and Technology"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.59,
         "substring" : "UCL School of Slavonic and East European Studies"
      },
      {
         "chosen" : false,
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
            "established" : 1978,
            "external_ids" : [
               {
                  "all" : [
                     "100012658"
                  ],
                  "preferred" : "100012658",
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "0000 0004 7744 075X"
                  ],
                  "preferred" : "0000 0004 7744 075X",
                  "type" : "isni"
               }
            ],
            "id" : "https://ror.org/04haakz20",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.nceeer.org"
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
                  "value" : "NCEEER"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "National Council For Eurasian & East European Research"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "National Council for Eurasian and East European Research"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "National Council/Eurasian & East European Research"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "funder",
               "nonprofit"
            ]
         },
         "score" : 0.54,
         "substring" : "UCL School of Slavonic and East European Studies"
      }
   ],
   "number_of_results" : 11
}
```

> 🚧 Don't automatically select the first "unchosen" result of an ?affiliation query with no `chosen: true` result
>
> When no result has `"chosen": true`, the first result is not necessarily the best match for a given affiliation string. In that case, several results may have the exact same score and/or there may be no match with a high score. Because the affiliation service breaks a given affiliation string into multiple substrings and performs searches of each substring on its own as well as in combination with other substrings, a high scoring match that is not chosen may be a match to only a small portion of the entire affiliation string. In these cases, it is best to respect the absence of `"chosen": true` and leave the string unmatched or add a layer of human or machine assignment, at your discretion.

# Other ways to match affiliations to ROR records

See our guide on [Matching organization names to ROR IDs](doc:matching) for more details on using the affiliation parameter and other methods, including third-party machine learning models, to match long, messy affiliation strings to ROR records.
