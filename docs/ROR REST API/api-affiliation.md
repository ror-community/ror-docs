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

<br />
