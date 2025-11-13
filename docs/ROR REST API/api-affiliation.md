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
> You can also match organization data to ROR IDs using the [?query parameter](https://ror.readme.io/docs/api-query) or [?query.advanced parameter](https://ror.readme.io/docs/api-advanced-query) with filters and field-specific queries. If your data is structured such that it separately stores an organization's name, city, country, website, and organizational identifiers such as GRID, Wikidata, or Funder IDs, we recommend that you use the [?query parameter](https://ror.readme.io/docs/api-query) or [?query.advanced parameter](https://ror.readme.io/docs/api-advanced-query) of the ROR API to match your data to ROR.

# Formatting searches

All request strings must be [URL-encoded](https://www.w3schools.com/tags/ref_urlencode.asp). The affiliation parameter is specifically designed to handle strings with punctuation, special characters, and spaces, so **it is not necessary to enclose multi-term search strings in quotation marks or to escape special characters**.

# Paging and filtering

The affiliation parameter **does not accept filters** and results **are not paginated**. If filter syntax is added to the end of an affiliation search, the terms will be treated as part of the affiliation search. All results will be returned, not just the first 20, since matching strategies typically return a small number of results. Results are listed in descending order by matching confidence score.

> 🚧 Be aware of differences between the affiliation parameter and the query parameters
>
> Unlike the[?query parameter](https://ror.readme.io/docs/api-query) and the [?query.advanced parameter](https://ror.readme.io/docs/api-advanced-query), the affiliation parameter does not accept filters, and results are not paginated -- all results will be returned, not just the first 20. If filter syntax is added to the end of an affiliation search, the terms will be treated as part of the affiliation search.  
>
> Also unlike the query and advanced query parameters, the affiliation parameter expects multi-word strings that include spaces, punctuation, and special characters. Surrounding terms in quotation marks or escaping special characters can produce worse results when using the affiliation parameter.

# Multisearch strategy

The default matching strategy for the ROR API affiliation parameter, in place [since November 2019](https://doi.org/10.71938/36jw-rs79), breaks long search strings into separate substrings, performing multiple searches with these values and limiting results to records matching any countries that can be derived from the text.  It then returns (if possible) the most likely match to a ROR record, as identified by a `chosen:true` indicator.

Additional candidates also appear in the results list and are ranked in descending order by confidence `score`. Only results with a score of at least .5 are returned. No more than one record in the results list receives a `chosen:true` indicator, and that record (if present) will always be listed first.

> 📘 Affiliation parameter multisearch format
>
> `https://api.ror.org/v2/organizations?affiliation=[URL-encoded-string]`

The matching types used in the multisearch strategy include the following:

* `PHRASE`: the entire phrase was matched to a variant of the organization's name
* `COMMON TERMS`: the matching was done by comparing the words separately
* `FUZZY`: the matching was done by a fuzzy comparison of the words separately
* `HEURISTICS`: "University of X" was matched to "X University"
* `ACRONYM`: matched by acronym
* `EXACT`: exact match of the entered string in name values in the `names` field excluding acronyms

## Example

The default multisearch strategy allows you to find a matching ROR record for a long, complex affiliation text string such as "Department of Civil and Industrial Engineering, University of Pisa, Largo Lucio Lazzarino 2, Pisa 56126, Italy".

```curl

curl 'https://api.ror.org/v2/organizations?affiliation=Department%20of%20Civil%20and%20Industrial%20Engineering%2C%20University%20of%20Pisa%2C%20Largo%20Lucio%20Lazzarino%202%2C%20Pisa%2056126%2C%20Italy' | json_pp

```

The first item in the results list, the ROR record for the University of Pisa, has a `chosen` value of _true_, indicating that the affiliation service considers this record a sufficiently likely match to the text string. Not all affiliation searches will produce a "chosen" result.

The `matching_type` is given as _"COMMON TERMS"_, indicating the method by which the affiliation parameter chose the matching record. The confidence `score` is 1, the highest possible level of confidence in the match. Results are listed in descending order by matching confidence score.

The substring used to find the match in this case is "Department of Civil and Industrial Engineering University of Pisa Largo Lucio Lazzarino 2 Pisa Italy", or the entire text content of the entered string excluding punctuation and the numeric postcode.

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
                  "date" : "2025-01-22",
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
               },
               {
                  "id" : "https://ror.org/05symbg58",
                  "label" : "Istituto Nazionale di Fisica Nucleare, Sezione di Pisa",
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
                  "date" : "2025-01-22",
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
                  "type" : "related"
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
         "matching_type" : "COMMON TERMS",
         "organization" : {
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
                  "id" : "https://ror.org/02w0r2764",
                  "label" : "MAGIC Telescopes",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/03ad39j10",
                  "label" : "University of Pisa",
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
               "unibs.it"
            ],
            "established" : 1982,
            "external_ids" : [
               {
                  "all" : [
                     "501100007343"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.7637.5"
                  ],
                  "preferred" : "grid.7637.5",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 1757 1846"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q1781263"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/02q2d2610",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.unibs.it"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/University_of_Brescia"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "IT",
                     "country_name" : "Italy",
                     "country_subdivision_code" : "25",
                     "country_subdivision_name" : "Lombardy",
                     "lat" : 45.53558,
                     "lng" : 10.21472,
                     "name" : "Brescia"
                  },
                  "geonames_id" : 3181554
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "University of Brescia"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "label"
                  ],
                  "value" : "Università degli Studi di Brescia"
               },
               {
                  "lang" : "de",
                  "types" : [
                     "label"
                  ],
                  "value" : "Universität Brescia"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "label"
                  ],
                  "value" : "Université de brescia"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 0.67,
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
               "unical.it"
            ],
            "established" : 1972,
            "external_ids" : [
               {
                  "all" : [
                     "501100007069"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.7778.f"
                  ],
                  "preferred" : "grid.7778.f",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 1937 0319"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q1752540"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/02rc97e94",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.unical.it"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/University_of_Calabria"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "IT",
                     "country_name" : "Italy",
                     "country_subdivision_code" : "78",
                     "country_subdivision_name" : "Calabria",
                     "lat" : 39.33154,
                     "lng" : 16.18041,
                     "name" : "Rende"
                  },
                  "geonames_id" : 2523623
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "UNICAL"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "University of Calabria"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "label"
                  ],
                  "value" : "Università della Calabria"
               },
               {
                  "lang" : "de",
                  "types" : [
                     "label"
                  ],
                  "value" : "Universität Kalabrien"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "label"
                  ],
                  "value" : "Université de la calabre"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/039epzh36",
                  "label" : "Istituto Nazionale di Fisica Nucleare, Gruppo Collegato di Cosenza",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 0.65,
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

## Example

The default multisearch strategy uses multiple search algorithms to find matching ROR records for complex affiliation text strings such as "International Centre for Theoretical Physics (ICTP), Trieste, Italy".

```curl

curl 'https://api.ror.org/v2/organizations?affiliation=International%20Centre%20for%20Theoretical%20Physics%20(ICTP),%20Trieste,%20Italy' | json_pp

```

The ROR record for the Abdus Salam International Centre for Theoretical Physics (ICTP) has a `chosen` value of _true_, indicating that the affiliation service considers this record a sufficiently likely match to the text string. Not all affiliation searches will produce a "chosen" result.

The `matching_type` is given as _"PHRASE"_, indicating the method by which the affiliation parameter chose the matching record. The confidence `score` is .95, with 1.0 being the highest possible level of confidence in the match. Results are listed in descending order by matching confidence score.

The substring used to find the match in this case is "International Centre for Theoretical Physics ICTP", which is the text of the organization name and its acronym excluding punctuation and the organization's location in Trieste, Italy.

```json
{
   "items" : [
      {
         "chosen" : true,
         "matching_type" : "PHRASE",
         "organization" : {
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
            "domains" : [
               "ictp.it"
            ],
            "established" : 1964,
            "external_ids" : [
               {
                  "all" : [
                     "501100001681"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.419330.c"
                  ],
                  "preferred" : "grid.419330.c",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 2184 9917"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q1190606"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/009gyvm78",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.ictp.it"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/International_Centre_for_Theoretical_Physics"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "IT",
                     "country_name" : "Italy",
                     "country_subdivision_code" : "36",
                     "country_subdivision_name" : "Friuli Venezia Giulia",
                     "lat" : 45.64953,
                     "lng" : 13.77678,
                     "name" : "Trieste"
                  },
                  "geonames_id" : 3165185
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Abdus Salam International Centre for Theoretical Physics"
               },
               {
                  "lang" : "it",
                  "types" : [
                     "label"
                  ],
                  "value" : "Centro Internazionale di Fisica Teorica Abdus Salam"
               },
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "ICTP"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "International Centre for Theoretical Physics"
               },
               {
                  "lang" : "sl",
                  "types" : [
                     "label"
                  ],
                  "value" : "Mednarodno središče Abdusa Salama za teoretično fiziko"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "The Abdus Salam International Centre for Theoretical Physics (ICTP)"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/04ys00n93",
                  "label" : "Institute for Geometry and Physics",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01r4aq231",
                  "label" : "ICTP - East Africa Institute for Fundamental Research",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04h4z8k05",
                  "label" : "UNESCO",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "facility",
               "funder"
            ]
         },
         "score" : 0.95,
         "substring" : "International Centre for Theoretical Physics ICTP"
      }
   ],
   "number_of_results" : 1
}
```

<br />
