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

The `matching_type` is given as _"PHRASE"_, indicating the method by which the affiliation parameter chose the matching record. The confidence `score` is .95, with 1 being the highest possible level of confidence in the match. Results are listed in descending order by matching confidence score.

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

# Single search strategy

As of November 2025, the affiliation parameter also supports a single search strategy that outperforms the multisearch strategy in terms of precision and recall while also being faster and more computationally efficient.

> 📘 Affiliation parameter single search format
>
> `https://api.ror.org/v2/organizations?affiliation=[URL-encoded-string]&single_search`

The single search matching strategy uses only a single query to find potential matches between affiliation strings and ROR records. Unlike the multisearch strategy, the single search strategy does not break up the search string into substrings, but instead always uses the entirety of the text string as the search term.

## Example

The single search strategy allows you to find a matching ROR record for a long, complex affiliation text string such as "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France".

```curl
curl 'https://api.ror.org/v2/organizations?affiliation=Department%20of%20Urology,%20Grenoble%20Alpes%20University%20Hospital,%20Universit%C3%A9%20Grenoble%20Alpes,%20CNRS,%20Grenoble%20INP,%20TIMC-IMAG,%20Grenoble,%20France&single_search' | json_pp 
```

The first item in the results list, the ROR record for Université Grenoble Alpes, has a `chosen` value of _true_, indicating that the affiliation service considers this record a sufficiently likely match to the text string. Not all affiliation searches will produce a "chosen" result.

The `matching_type` is given as "SINGLE SEARCH", which will always be the case for queries that use the `&single_search` parameter. The confidence `score` for the match is 1, the highest possible level of confidence in the match. Results are listed in descending order by matching confidence score.

The substring used to find the match in this case is "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France", which is the entirety of the text string including punctuation.

```json
{
   "items" : [
      {
         "chosen" : true,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2025-07-15",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2016,
            "external_ids" : [
               {
                  "all" : [
                     "100012952",
                     "100013349"
                  ],
                  "preferred" : "100012952",
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.450307.5"
                  ],
                  "preferred" : "grid.450307.5",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "Q945876"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/02rx3b187",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.univ-grenoble-alpes.fr"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Grenoble_Alpes_University"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "FR",
                     "country_name" : "France",
                     "country_subdivision_code" : "ARA",
                     "country_subdivision_name" : "Auvergne-Rhône-Alpes",
                     "lat" : 45.1787,
                     "lng" : 5.76281,
                     "name" : "Saint-Martin-d'Hères"
                  },
                  "geonames_id" : 2978317
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Grenoble Alpes University"
               },
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "UGA"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Université Grenoble Alpes"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/000tdrn36",
                  "label" : "Centre Interuniversitaire de MicroElectronique et Nanotechnologies",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00fwjkb59",
                  "label" : "Laboratoire d'Economie Appliquée de Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05sbt2524",
                  "label" : "Institut polytechnique de Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04dbzz632",
                  "label" : "Institut Néel",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01cf2sz15",
                  "label" : "Institut des Sciences de la Terre",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01wwcfa26",
                  "label" : "Institut des Géosciences de l'Environnement",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03eqm6y13",
                  "label" : "LabEx PERSYVAL-Lab",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/023n9q531",
                  "label" : "Laboratoire Interdisciplinaire de Physique",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05588ks88",
                  "label" : "Laboratoire de Linguistique et Didactique des Langues Etrangères et Maternelles",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04qz4qy85",
                  "label" : "Laboratoire de Recherche Historique Rhône-Alpes",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03f0apy98",
                  "label" : "Laboratoire de Physique Subatomique et de Cosmologie",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02rmwrd87",
                  "label" : "Laboratoire Environnements, Dynamiques et Territoires de Montagne",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/047p7mf25",
                  "label" : "Institut Carnot PolyNat",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03vte9x46",
                  "label" : "Observatoire des Sciences de l'Univers de Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05c99vk74",
                  "label" : "Laboratoire de Recherche sur les Apprentissages en Contexte",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04fhvpc68",
                  "label" : "Département de Pharmacochimie Moléculaire",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03jrb0276",
                  "label" : "Laboratoire Inter-universitaire de Psychologie: Personnalité, Cognition, Changement Social",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01kbr1737",
                  "label" : "PHotonique ELectronique et Ingénierie QuantiqueS",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00ndvqf03",
                  "label" : "Laboratoire Modélisation et Exploration des Matériaux",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/0467x8h16",
                  "label" : "Maison des Sciences de l'Homme-Alpes",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02cmt9z73",
                  "label" : "Agence pour les Mathématiques en Interaction avec l'Entreprise et la Société",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/026m44z54",
                  "label" : "Laboratoire Nanotechnologies et Nanosystèmes",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03949e763",
                  "label" : "Integrated Structural Biology Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04ndt7n58",
                  "label" : "Centre d'Etudes et de Recherche sur la diplomatie, l’Administration Publique et le Politique",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/044cfnj78",
                  "label" : "GRICAD - Grenoble Alpes Recherche-Infrastructure de Calcul intensif et de Données",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/0509qp208",
                  "label" : "Centre d'Etudes et de Recherches Appliquées à la Gestion",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03e044190",
                  "label" : "Spintronique et Technologie des Composants",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01w1erp60",
                  "label" : "Couplage Multi-physiques et Multi-échelles en mécanique géo-environnemental",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01c8rcg82",
                  "label" : "Laboratoire d'Informatique de Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02wrme198",
                  "label" : "GIPSA-Lab",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04ett5b41",
                  "label" : "Laboratoire Jean Kuntzmann",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/026j45x50",
                  "label" : "Laboratoire Pacte",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04eg25g76",
                  "label" : "Laboratoire de Conception et d'Intégration des Systèmes",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/014p6mg26",
                  "label" : "Laboratoire de Psychologie et NeuroCognition",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/055q9jt53",
                  "label" : "Laboratoire Arts et pratiques du texte, de l’image, de l’écran et de la scène",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05hz99a17",
                  "label" : "AAU - Ambiances, Architectures, Urbanités",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04e2ndp15",
                  "label" : "Centre de recherches juridiques de Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05bw7ad85",
                  "label" : "Centre de recherche en économie de Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01a0ez112",
                  "label" : "Laboratoire EcoSystèmes et Sociétés En Montagne",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00genbz89",
                  "label" : "Méthodes et Histoire de l'architecture",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/033d95m27",
                  "label" : "Centre d'études sur la sécurité internationale et les coopérations européennes",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00t9pwh21",
                  "label" : "Sport et environnement social",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02wrd4e19",
                  "label" : "Architecture, Environnement & Cultures Constructives",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00y523k32",
                  "label" : "Laboratoire Universitaire Histoire Cultures Italie Europe",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01554y636",
                  "label" : "Institut de philosophie de Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00awrf758",
                  "label" : "Groupe de recherche sur les enjeux de la communication",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00rv5x925",
                  "label" : "Laboratoire des Sciences pour la Conception, l'Optimisation et la Production",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05rwrfh97",
                  "label" : "Institut Fourier",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/000063q30",
                  "label" : "Techniques of Informatics and Microelectronics for Integrated Systems Architecture",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03985kf35",
                  "label" : "Translational Innovation in Medicine and Complexity",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04as3rk94",
                  "label" : "Grenoble Institute of Neurosciences",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01aj0bf86",
                  "label" : "Autonomie, Gérontologie, E-santé, Imagerie et Société",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/039j4x695",
                  "label" : "Laboratoire Biosciences et bioingénierie pour la Santé",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01027m165",
                  "label" : "Brain Tech Laboratory",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/0557vhy43",
                  "label" : "Laboratoire Biologie et Biotechnologie pour la Santé",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04szabx38",
                  "label" : "Institut de Biologie Structurale",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/0003ege03",
                  "label" : "Centre de Recherches sur les Macromolécules Végétales",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/010rs2a38",
                  "label" : "Département de Chimie Moléculaire",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/0459x4g44",
                  "label" : "Hypoxie et Physiopathologies cardiovasculaires et respiratoires",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03vyv3y87",
                  "label" : "Haute Technologie Animale Grenobloise",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05kwbf598",
                  "label" : "Institut pour l'avancée des biosciences",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02dd25k08",
                  "label" : "Laboratoire de Chimie et Biologie des Métaux",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03x1z2w73",
                  "label" : "Laboratoire d'Écologie Alpine",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00jxe7243",
                  "label" : "Laboratoire Physiologie Cellulaire & Végétale",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01273vs09",
                  "label" : "Laboratoire de Radiopharmaceutiques Biocliniques",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05514hp74",
                  "label" : "Laboratory of Fundamental and Applied Bioenergetics",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/0535cbn94",
                  "label" : "Systèmes Moléculaires et nanoMatériaux pour l'Énergie et la Santé",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03t9s7t87",
                  "label" : "Pathogénèse Bactérienne et Réponses Cellulaires",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/044ggyg50",
                  "label" : "Laboratoire Rhéologie et Procédés",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/045ktmd28",
                  "label" : "Laboratoire National des Champs Magnétiques Intenses",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02z8yps18",
                  "label" : "Laboratoire de génie des procédés pour la bioraffinerie, les matériaux bio-sourcés et l’impression fonctionnelle",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01kcrnc96",
                  "label" : "Institut de Planétologie et d'Astrophysique de Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/043pfpy19",
                  "label" : "Laboratoire des Écoulements Géophysiques et Industriels",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03bcdsr62",
                  "label" : "Sols, Solides, Structures, Risques",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04axb9j69",
                  "label" : "Laboratoire d’Electrochimie et de Physico-chimie des Matériaux et des Interfaces",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/014n97s28",
                  "label" : "Laboratoire des Matériaux et du Génie Physique",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02mc6qk71",
                  "label" : "Laboratoire de Physique et Modélisation des Milieux Condensés",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/036zswm25",
                  "label" : "Laboratoire des Technologies de la Microélectronique",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05hyx5a17",
                  "label" : "Laboratoire de Génie Électrique de Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00m7zca23",
                  "label" : "Science et Ingénierie des Matériaux et Procédés",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/050rs7291",
                  "label" : "Centre de Radiofréquences, Optique et Micro-nanoélectronique des Alpes",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/009ps2x93",
                  "label" : "Département des Systèmes Basses Températures",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01hrtf583",
                  "label" : "Synchrotron Radiation for Biomedicine",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00prvqp73",
                  "label" : "Institut Rhônalpin des Systèmes Complexes",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02f7wz369",
                  "label" : "Université Pierre Mendès France",
                  "type" : "predecessor"
               },
               {
                  "id" : "https://ror.org/041rhpw39",
                  "label" : "Centre Hospitalier Universitaire de Grenoble",
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
                  "id" : "https://ror.org/01x6z5t49",
                  "label" : "Fédération de Recherche Spectroscopies de Photoémission",
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
         "substring" : "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2025-07-15",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 1892,
            "external_ids" : [
               {
                  "all" : [
                     "grid.5676.2"
                  ],
                  "preferred" : "grid.5676.2",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 1765 4326"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q1665121"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/05sbt2524",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.grenoble-inp.fr/welcome/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Grenoble_Institute_of_Technology"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "FR",
                     "country_name" : "France",
                     "country_subdivision_code" : "ARA",
                     "country_subdivision_name" : "Auvergne-Rhône-Alpes",
                     "lat" : 45.17869,
                     "lng" : 5.71479,
                     "name" : "Grenoble"
                  },
                  "geonames_id" : 3014728
               }
            ],
            "names" : [
               {
                  "lang" : "fr",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Grenoble INP"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Grenoble Institute of Technology"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "label",
                     "ror_display"
                  ],
                  "value" : "Institut polytechnique de Grenoble"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/000tdrn36",
                  "label" : "Centre Interuniversitaire de MicroElectronique et Nanotechnologies",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00fwjkb59",
                  "label" : "Laboratoire d'Economie Appliquée de Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01c8rcg82",
                  "label" : "Laboratoire d'Informatique de Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02wrme198",
                  "label" : "GIPSA-Lab",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03taa9n66",
                  "label" : "Institut de Microélectronique, Electromagnétisme et Photonique",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/01wwcfa26",
                  "label" : "Institut des Géosciences de l'Environnement",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03eqm6y13",
                  "label" : "LabEx PERSYVAL-Lab",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04ett5b41",
                  "label" : "Laboratoire Jean Kuntzmann",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/044ggyg50",
                  "label" : "Laboratoire Rhéologie et Procédés",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04eg25g76",
                  "label" : "Laboratoire de Conception et d'Intégration des Systèmes",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05hyx5a17",
                  "label" : "Laboratoire de Génie Électrique de Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00rv5x925",
                  "label" : "Laboratoire des Sciences pour la Conception, l'Optimisation et la Production",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/043pfpy19",
                  "label" : "Laboratoire des Écoulements Géophysiques et Industriels",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04axb9j69",
                  "label" : "Laboratoire d’Electrochimie et de Physico-chimie des Matériaux et des Interfaces",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02z8yps18",
                  "label" : "Laboratoire de génie des procédés pour la bioraffinerie, les matériaux bio-sourcés et l’impression fonctionnelle",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03f0apy98",
                  "label" : "Laboratoire de Physique Subatomique et de Cosmologie",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/014n97s28",
                  "label" : "Laboratoire des Matériaux et du Génie Physique",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00m7zca23",
                  "label" : "Science et Ingénierie des Matériaux et Procédés",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03bcdsr62",
                  "label" : "Sols, Solides, Structures, Risques",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03e044190",
                  "label" : "Spintronique et Technologie des Composants",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03985kf35",
                  "label" : "Translational Innovation in Medicine and Complexity",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/000063q30",
                  "label" : "Techniques of Informatics and Microelectronics for Integrated Systems Architecture",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05afmzm11",
                  "label" : "Verimag",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/044cfnj78",
                  "label" : "GRICAD - Grenoble Alpes Recherche-Infrastructure de Calcul intensif et de Données",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/050rs7291",
                  "label" : "Centre de Radiofréquences, Optique et Micro-nanoélectronique des Alpes",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/011f68d87",
                  "label" : "Laboratoire franco-mexicain d'informatique et d'automatique",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02rx3b187",
                  "label" : "Université Grenoble Alpes",
                  "type" : "parent"
               },
               {
                  "id" : "https://ror.org/05be9p317",
                  "label" : "Micropesanteur Fondamentale et Appliquée",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/01sgwka45",
                  "label" : "Microscopie Fonctionnelle du Vivant",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/04fhg0w34",
                  "label" : "ALERT Geomaterials – Alliance of Laboratories in Europe for Education, Research and Technology",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 1,
         "substring" : "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
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
            "established" : 2005,
            "external_ids" : [
               {
                  "all" : [
                     "grid.492977.6"
                  ],
                  "preferred" : "grid.492977.6",
                  "type" : "grid"
               }
            ],
            "id" : "https://ror.org/020ay6p95",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://zurology.com/"
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
                     "lat" : 26.27119,
                     "lng" : -80.2706,
                     "name" : "Coral Springs"
                  },
                  "geonames_id" : 4151909
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Z Urology"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "healthcare"
            ]
         },
         "score" : 0.89,
         "substring" : "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
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
            "established" : 1951,
            "external_ids" : [
               {
                  "all" : [
                     "501100004113"
                  ],
                  "preferred" : null,
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.411324.1"
                  ],
                  "preferred" : "grid.411324.1",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 2324 3572"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q975461"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/05x6qnc69",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.ul.edu.lb/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/Lebanese_University"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "AS",
                     "continent_name" : "Asia",
                     "country_code" : "LB",
                     "country_name" : "Lebanon",
                     "country_subdivision_code" : "BA",
                     "country_subdivision_name" : "Beyrouth",
                     "lat" : 33.89332,
                     "lng" : 35.50157,
                     "name" : "Beirut"
                  },
                  "geonames_id" : 276781
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Lebanese University"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "label"
                  ],
                  "value" : "Université Libanaise"
               },
               {
                  "lang" : "ar",
                  "types" : [
                     "label"
                  ],
                  "value" : "الجامعة اللبنانية"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/036da3063",
                  "label" : "Lebanese Hospital Geitaoui-University Medical Center",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/000tqtb97",
                  "label" : "Rafik Hariri University Hospital",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 0.84,
         "substring" : "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2018-11-14",
                  "schema_version" : "1.0"
               },
               "last_modified" : {
                  "date" : "2025-10-06",
                  "schema_version" : "2.1"
               }
            },
            "domains" : [],
            "established" : 2007,
            "external_ids" : [
               {
                  "all" : [
                     "grid.457348.9"
                  ],
                  "preferred" : "grid.457348.9",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 0630 1517"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q2931224"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/02mg6n827",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.cea.fr/Pages/le-cea/les-centres-cea/grenoble.aspx"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "FR",
                     "country_name" : "France",
                     "country_subdivision_code" : "ARA",
                     "country_subdivision_name" : "Auvergne-Rhône-Alpes",
                     "lat" : 45.17869,
                     "lng" : 5.71479,
                     "name" : "Grenoble"
                  },
                  "geonames_id" : 3014728
               }
            ],
            "names" : [
               {
                  "lang" : "fr",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "CEA Grenoble"
               },
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "CENG"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Centre d'Études Nucléaires de Grenoble"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "alias"
                  ],
                  "value" : "Commissariat à l'Énergie Atomique et aux Énergies Alternatives centre de Grenoble"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/00fwjkb59",
                  "label" : "Laboratoire d'Economie Appliquée de Grenoble",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/036zswm25",
                  "label" : "Laboratoire des Technologies de la Microélectronique",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00jjx8s55",
                  "label" : "Commissariat à l'Énergie Atomique et aux Énergies Alternatives",
                  "type" : "parent"
               }
            ],
            "status" : "active",
            "types" : [
               "government"
            ]
         },
         "score" : 0.83,
         "substring" : "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
            "admin" : {
               "created" : {
                  "date" : "2023-07-27",
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
                     "0000 0001 2248 2918"
                  ],
                  "preferred" : "0000 0001 2248 2918",
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q3152445"
                  ],
                  "preferred" : "Q3152445",
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/03c2nxn64",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.inp.fr"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Institut_national_du_patrimoine"
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
                     "lat" : 48.8534,
                     "lng" : 2.3486,
                     "name" : "Paris"
                  },
                  "geonames_id" : 2968815
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "INP"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "alias"
                  ],
                  "value" : "INP France"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "alias"
                  ],
                  "value" : "INP Paris"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Institut national du patrimoine"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "National Heritage Institute"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "alias"
                  ],
                  "value" : "National Institute of Heritage"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "archive"
            ]
         },
         "score" : 0.82,
         "substring" : "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
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
            "established" : 1990,
            "external_ids" : [
               {
                  "all" : [
                     "grid.442434.1"
                  ],
                  "preferred" : "grid.442434.1",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 0641 7876"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q6429212"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/04j9dfg85",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.universitekongo.org/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Kongo_University"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "AF",
                     "continent_name" : "Africa",
                     "country_code" : "CD",
                     "country_name" : "DR Congo",
                     "country_subdivision_code" : "BC",
                     "country_subdivision_name" : "Bas-Congo",
                     "lat" : -5.25837,
                     "lng" : 14.85838,
                     "name" : "Mbanza-Ngungu"
                  },
                  "geonames_id" : 2312888
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Kongo University"
               },
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "UK"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "label"
                  ],
                  "value" : "Université Kongo"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.81,
         "substring" : "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
         "organization" : {
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
            "domains" : [
               "ulaval.ca"
            ],
            "established" : 1663,
            "external_ids" : [
               {
                  "all" : [
                     "100007867",
                     "100015115"
                  ],
                  "preferred" : "100007867",
                  "type" : "fundref"
               },
               {
                  "all" : [
                     "grid.23856.3a"
                  ],
                  "preferred" : "grid.23856.3a",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0004 1936 8390"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q1067935"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/04sjchr03",
            "links" : [
               {
                  "type" : "website",
                  "value" : "https://www.ulaval.ca"
               },
               {
                  "type" : "wikipedia",
                  "value" : "http://en.wikipedia.org/wiki/Laval_University"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "NA",
                     "continent_name" : "North America",
                     "country_code" : "CA",
                     "country_name" : "Canada",
                     "country_subdivision_code" : "QC",
                     "country_subdivision_name" : "Quebec",
                     "lat" : 46.81228,
                     "lng" : -71.21454,
                     "name" : "Québec"
                  },
                  "geonames_id" : 6325494
               }
            ],
            "names" : [
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Laval University"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Université Laval"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/01q8ytn75",
                  "label" : "Center for Northern Studies",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02s3xv195",
                  "label" : "Amundsen Science",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03tzsdk41",
                  "label" : "Sentinelle Nord",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03wx0vn75",
                  "label" : "CentrEau - Quebec Water Management Research Centre",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03x44hg04",
                  "label" : "Centre de recherche en aménagement et développement",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03re3pg25",
                  "label" : "Institut sur la nutrition et les aliments fonctionnels (INAF)",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04ya1kq07",
                  "label" : "Québec-Océan",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/04bzgtz06",
                  "label" : "Takuvik Joint International Laboratory",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/03gf7z214",
                  "label" : "Institut Universitaire de Cardiologie et de Pneumologie de Québec",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/03mt5nv96",
                  "label" : "Institut Universitaire en Santé Mentale de Québec",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/05xay3m79",
                  "label" : "Érudit",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/0155edt85",
                  "label" : "fabriqueREL",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/05fdt6816",
                  "label" : "Obvia",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/05qn5kv73",
                  "label" : "CHU de Québec-Université Laval",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/04rgqcd02",
                  "label" : "Centre de recherche du CHU de Québec-Université Laval",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/032hvvf98",
                  "label" : "Centre de recherche sur l'aluminium – REGAL",
                  "type" : "related"
               },
               {
                  "id" : "https://ror.org/03tg44r87",
                  "label" : "Centre interuniversitaire d'études et de recherches autochtones",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "education",
               "funder"
            ]
         },
         "score" : 0.81,
         "substring" : "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
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
            "established" : 1974,
            "external_ids" : [
               {
                  "all" : [
                     "grid.410529.b"
                  ],
                  "preferred" : "grid.410529.b",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "0000 0001 0792 4829"
                  ],
                  "preferred" : null,
                  "type" : "isni"
               },
               {
                  "all" : [
                     "Q2945764"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/041rhpw39",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.chu-grenoble.fr/"
               },
               {
                  "type" : "wikipedia",
                  "value" : "https://en.wikipedia.org/wiki/Centre_Hospitalier_Universitaire_de_Grenoble"
               }
            ],
            "locations" : [
               {
                  "geonames_details" : {
                     "continent_code" : "EU",
                     "continent_name" : "Europe",
                     "country_code" : "FR",
                     "country_name" : "France",
                     "country_subdivision_code" : "ARA",
                     "country_subdivision_name" : "Auvergne-Rhône-Alpes",
                     "lat" : 45.17869,
                     "lng" : 5.71479,
                     "name" : "Grenoble"
                  },
                  "geonames_id" : 3014728
               }
            ],
            "names" : [
               {
                  "lang" : null,
                  "types" : [
                     "acronym"
                  ],
                  "value" : "CHUG"
               },
               {
                  "lang" : "fr",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Centre Hospitalier Universitaire de Grenoble"
               },
               {
                  "lang" : "en",
                  "types" : [
                     "label"
                  ],
                  "value" : "Grenoble University Hospital Centre"
               }
            ],
            "relationships" : [
               {
                  "id" : "https://ror.org/05kfvx519",
                  "label" : "Hôpital Albert Michallon",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/00dxw4v18",
                  "label" : "Hôpital Couple Enfant",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/05kwbf598",
                  "label" : "Institut pour l'avancée des biosciences",
                  "type" : "child"
               },
               {
                  "id" : "https://ror.org/02rx3b187",
                  "label" : "Université Grenoble Alpes",
                  "type" : "related"
               }
            ],
            "status" : "active",
            "types" : [
               "healthcare"
            ]
         },
         "score" : 0.8,
         "substring" : "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France"
      },
      {
         "chosen" : false,
         "matching_type" : "SINGLE SEARCH",
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
            "established" : 2002,
            "external_ids" : [
               {
                  "all" : [
                     "grid.442547.4"
                  ],
                  "preferred" : "grid.442547.4",
                  "type" : "grid"
               },
               {
                  "all" : [
                     "Q30294265"
                  ],
                  "preferred" : null,
                  "type" : "wikidata"
               }
            ],
            "id" : "https://ror.org/03y2yf271",
            "links" : [
               {
                  "type" : "website",
                  "value" : "http://www.time.ens.tn"
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
                  "lang" : "fr",
                  "types" : [
                     "ror_display",
                     "label"
                  ],
                  "value" : "Time Université"
               }
            ],
            "relationships" : [],
            "status" : "active",
            "types" : [
               "education"
            ]
         },
         "score" : 0.8,
         "substring" : "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France"
      }
   ],
   "number_of_results" : 10
}
```

<br />
