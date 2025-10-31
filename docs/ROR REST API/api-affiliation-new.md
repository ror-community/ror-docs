---
title: Affiliation parameter (new)
deprecated: false
hidden: true
link:
  new_tab: false
metadata:
  robots: index
---
Affiliation parameter

> 👍 ROR REST API v2
>
> This page documents v2 of the ROR REST API. For v1 documentation of the ROR REST API, see [https://ror.readme.io/v1/docs/api-affiliation](https://ror.readme.io/v1/docs/api-affiliation). You can also read more about ROR [API versions](doc:api-versions) and a summary of what's new in [Schema 2.0](doc:schema-v2) and [Schema 2.1](doc:schema-2-1).

> ❗️ Version 1 of the ROR schema and API will be sunset in December 2025
>
> In December 2025, version 1 of the ROR schema and API will be sunset, meaning that ROR API requests with v1 in the path will no longer return a response, v1 files will no longer be included in the ROR data dump, and v1 documentation will no longer be available. Read more in our [changelog](https://ror.readme.io/changelog/2025-07-01-sunset-of-version-1#/).

# About the affiliation parameter

For many years, publishers of scholarly information often captured author and contributor affiliation information as unstructured data, sometimes storing an organization's name, a sub-unit or department's name, a street address, and a geographical location along with copious irregular punctuation as a text string in a single field. Some affiliation strings even include multiple affiliations.

The affiliation parameter of the ROR API is designed to help match these messy text strings to ROR records to produce cleaner affiliation data. The affiliation service attempts to find the ROR record that is the most probable match for the given affiliation string; if it finds a likely candidate, it returns that result with a chosen:true value. Additional possibilities that might match the string are also included in results, listed in descending order by confidence score.

An API-based approach to matching affiliation strings to ROR IDs can work well for large-scale systems where human review of every proposed match is impractical, but no large-scale programmatic approach to matching is perfect, especially since there are many similar and even identical names and acronyms among research organizations globally. Often, the matching service will not be able to suggest a match for a particular string, and in some cases, the matching service might suggest an incorrect match. Human review is always the best fallback.

> 📘 Consider a different method of matching if you have structured organization data
>
> You can also match organization data to ROR IDs using the ?query and ?query.advanced parameter with filters and field-specific queries. If your data is structured such that it separately stores an organization's name, city, country, website, and organizational identifiers such as GRID, Wikidata, or Funder IDs, we recommend that you use the ?query parameter or ?query.advanced parameter of the ROR API to match your data to ROR.

# Formatting searches

All request strings must be [URL-encoded](https://www.w3schools.com/tags/ref_urlencode.asp). The affiliation parameter is specifically designed to handle strings with punctuation, special characters, and spaces, so **it is not necessary to enclose multi-term search strings in quotation marks or to escape special characters**.

# Paging and filtering

The affiliation parameter **does not accept filters** and results **are not paginated**. If filter syntax is added to the end of an affiliation search, the terms will be treated as part of the affiliation search. All results will be returned, not just the first 20, since matching strategies typically return a small number of results. Results are listed in descending order by matching confidence score.

> 🚧 Be aware of differences between the affiliation parameter and the query parameters
>
> Unlike the query and advanced query parameters, the affiliation parameter does not accept filters, and results are not paginated -- all results will be returned, not just the first 20. If filter syntax is added to the end of an affiliation search, the terms will be treated as part of the affiliation search.
>
> Also unlike the query and advanced query parameters, the affiliation parameter expects multi-word strings that include spaces, punctuation, and special characters. Surrounding terms in quotation marks or escaping special characters can produce worse results when using the affiliation parameter.

# Multisearch strategy

The default matching strategy for the ROR API affiliation parameter, in place [since November 2019](https://doi.org/10.71938/36jw-rs79), breaks long search strings into separate substrings, performs multiple searches of only the `names` field in ROR using several different search algorithms, limits results to records matching any country names or ISO codes in the text, and finally returns (if possible!) its best guess about the most likely match to a ROR record with the chosen:true indicator.

Additional possibilities also appear in the results list and are ranked in descending order by confidence score. Only results with a score of at least .5 are returned. No more than one record in the results list receives a chosen:true indicator, and that record (if present) will always be listed first.

> 📘 Affiliation parameter multisearch format
>
> `https://api.ror.org/v2/organizations?affiliation=[URL-encoded-string]`

Search algorithms used in the multisearch matching strategy include the following:

`PHRASE`: the entire phrase was matched to a variant of the organization's name
`COMMON TERMS`: the matching was done by comparing the words separately
`FUZZY`: the matching was done by fuzzy-comparing the words separately
`HEURISTICS`: "University of X" was matched to "X University"
`ACRONYM`: matched by acronym
`EXACT`: exact match of the entered string in name values in the `names` field excluding acronyms

## Example

The default multisearch strategy allows you to find a matching ROR record for a long, complex affiliation text string such as "Department of Civil and Industrial Engineering, University of Pisa, Largo Lucio Lazzarino 2, Pisa 56126, Italy".

```curl
curl 'https://api.ror.org/v2/organizations?affiliation=Department%20of%20Civil%20and%20Industrial%20Engineering%2C%20University%20of%20Pisa%2C%20Largo%20Lucio%20Lazzarino%202%2C%20Pisa%2056126%2C%20Italy' | json_pp
```

The first item in the results list, the ROR record for the University of Pisa, has a `chosen` value of _true_, indicating that the affiliation service considers this record a sufficiently likely match to the text string. Not all affiliation searches will produce a "chosen" result.

The `matching_type` is given as _"COMMON TERMS"_, indicating the method by which the affiliation parameter chose the matching record. The confidence `score` is 1, the highest possible level of confidence in the match. Results are listed in descending order by matching confidence score.

The substring used to find the match in this case is "Department of Civil and Industrial Engineering University of Pisa Largo Lucio Lazzarino 2 Pisa Italy", or the entire text content of the entered string excluding punctuation and the numeric postcode.

TKTKTK JSON of results

## Example

The default multisearch strategy uses multiple search algorithms to find matching ROR records for complex affiliation text strings such as "International Centre for Theoretical Physics (ICTP), Trieste, Italy".

```curl
curl 'https://api.ror.org/v1/organizations?affiliation=International%20Centre%20for%20Theoretical%20Physics%20(ICTP),%20Trieste,%20Italy' | json_pp
```

The first item in the results list, the ROR record for The Abdus Salam International Centre for Theoretical Physics (ICTP),  has a `chosen` value of _true_, indicating that the affiliation service considers this record a sufficiently likely match to the text string. Not all affiliation searches will produce a "chosen" result.

The `matching_type` is given as _"PHRASE"_, indicating the method by which the affiliation parameter chose the matching record. The confidence `score` is 1, the highest possible level of confidence in the match. Results are listed in descending order by matching confidence score.

The substring used to find the match in this case is "International Centre for Theoretical Physics ICTP", which is the text of the organization name and its acronym excluding punctuation and the organization's location in Trieste, Italy.

TKTKTK JSON of results

# Single search strategy

As of October 2025, the affiliation parameter also supports a single search strategy that performs comparably to the multiple search strategy in terms of precision and recall but is much faster to execute, returning matching results in a fraction of the time as the multiple search strategy while also using far fewer computing resources. The single search strategy is therefore particularly well suited for matching ROR IDs to affiliation strings in very large datasets comprising millions of items.

> 📘 Affiliation parameter single search format
>
> `https://api.ror.org/v2/organizations?affiliation=[URL-encoded-string]&single_search`

The single search matching strategy uses only a single search algorithm to find probable matches of affiliation strings in ROR records. Unlike the multisearch strategy, the single search strategy does not break up the search string into substrings, but instead always uses the entirety of the text string as the search term. TKTKTK

## Example

The single search strategy allows you to find a matching ROR record for a long, complex affiliation text string such as "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France".

```curl
curl 'https://api.ror.org/organizations?affiliation=Department%20of%20Urology,%20Grenoble%20Alpes%20University%20Hospital,%20Universit%C3%A9%20Grenoble%20Alpes,%20CNRS,%20Grenoble%20INP,%20TIMC-IMAG,%20Grenoble,%20France&single_search' | json_pp
```

The first item in the results list, the ROR record for Université Grenoble Alpes, has a `chosen` value of _true_, indicating that the affiliation service considers this record a sufficiently likely match to the text string. Not all affiliation searches will produce a "chosen" result.

The `matching_type` is given as "SINGLE SEARCH", which will always be the case for queries that use the &single_search parameter. The confidence `score` for the match is 1, the highest possible level of confidence in the match. Results are listed in descending order by matching confidence score.

The substring used to find the match in this case is "Department of Urology, Grenoble Alpes University Hospital, Université Grenoble Alpes, CNRS, Grenoble INP, TIMC-IMAG, Grenoble, France", which is the entirety of the text string including punctuation.

TKTKTK JSON of results

# No matches found

When there's no result with the chosen:true indicator (meaning that all results of a query are chosen:false) it can mean that the string does not include enough information for the algorithm to find a match, or that there are several good matches but no clear winner, or that the organization is not in ROR. If there is no result with chosen:true, some additional review by humans or machines is almost always needed.

> 🚧 Don't automatically select the first result of an ?affiliation query
>
> When no result has the `chosen: true` indicator, there might be no match with a high score, or several results might have the exact same score. In these cases, it is best to respect the absence of chosen:true and leave the string unmatched or add an additional layer of human or machine matching, at your discretion.

## Example

An affiliation string such as "UCL School of Slavonic and East European Studies" does not contain enough identifying information for the ROR API affiliation matching service to choose a matching ROR record.

```curl
curl 'https://api.ror.org/v2/organizations?affiliation=UCL%20School%20of%20Slavonic%20and%20East%20European%20Studies' | json_pp
```

The response returns 11 results with confidence scores ranging from .7 to .54 listed in descending order, but the affiliation matching service does not rate any of these results as a recommended match.

TKTKTK JSON results

## Example

The affiliation string "CNRS-CRHEA, rue Bernard Grégory, 06560 Valbonne, France" represents an organization that is not in ROR, and therefore the ROR API affiliation matching service cannot find a matching ROR record.

```curl
curl 'https://api.ror.org/organizations?affiliation=CNRS-CRHEA,%20rue%20Bernard%20Gr%C3%A9gory,%2006560%20Valbonne,%20France&single_search' | json_pp
```

The response returns 10 results with confidence scores ranging from .82 to .74 listed in descending order, but the affiliation matching service does not rate any of these results as a recommended match.

TKTKTK JSON results

# Other ways to match affiliations to ROR records

See our guide on [Matching organization names to ROR IDs](doc:matching) for more details on using the affiliation parameter and other methods, including third-party machine learning models, to match long, messy affiliation strings to ROR records.

Other resources
Match organization names to ROR IDs
ROR blog posts on matching
ror-utilities matching scripts
