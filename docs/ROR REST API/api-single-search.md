---
title: Single search parameter
deprecated: false
hidden: true
link:
  new_tab: false
metadata:
  robots: index
---
> 👍 ROR REST API v2
>
> This page documents v2 of the ROR REST API. For v1 documentation of the ROR REST API, see TKTKTK. You can also read more about ROR [API versions](doc:api-versions) and a summary of what's new in [Schema 2.0](doc:schema-v2) and [Schema 2.1](doc:schema-2-1).

> 🚧 Changes to the ROR API begin the week of July 28, 2025
>
> Beginning the week of July 28, 2025, **ROR API requests with no version in the path will default to responses that use version 2 of the ROR schema instead of version 1**. Read more in our [changelog](https://ror.readme.io/changelog/2025-07-01-sunset-of-version-1).

# About the single search parameter

The single search parameter is designed to match messy text to ROR records. It uses a newer and faster matching strategy designed as a replacement for the [affiliation parameter](/api-affiliation) of the ROR API. The single search parameter is designed to be used for for the following purposes:

* Matching ROR IDs to legacy author affiliations in large-scale knowledge graphs and scholarly publishing systems
* Matching ROR IDs to long and heavily-punctuated text strings that contain not just organization names, but also extraneous information such as addresses and academic departments

The single search parameter can match messy text strings to ROR IDs at about 85% efficacy depending on the text data and the affiliation implementation. Because of the high number of identical and similar names among research institutions globally, we recommend human review of suggested matches.

> 📘 Single search parameter format
>
> `https://api.ror.org/v2/organizations?TKTKTK`

# Formatting searches

All request strings must be [URL-encoded](https://www.w3schools.com/tags/ref_urlencode.asp). The single parameter is specifically designed to handle strings with punctuation, special characters, and spaces, so **it is not necessary to enclose multi-term search strings in quotation marks or to escape special characters**.

# Paging and filtering

The single search parameter **does not accept filters** and results **are not paginated**. When filter syntax is added to the end of an affiliation search, the terms will be treated as part of the affiliation search. All results will be returned, not just the first 20, since this approach typically returns a small number of results. Results are listed in descending order by matching confidence score.

> 🚧 Be aware of differences between the single search parameter and the query parameters
>
> Unlike the query and advanced query parameters, the single search parameter does not accept filters, and results are not paginated -- all results will be returned, not just the first 20. When filter syntax is added to the end of an affiliation search, the terms will be treated as part of the affiliation search.
>
> Also unlike the query and advanced query parameters, the single search parameter expects multi-word strings that include spaces, punctuation, and special characters. Surrounding terms in quotation marks or escaping special characters can produce worse results when using the single search parameter.

# Matching long, messy text strings to ROR records

If you have messy, unstructured text data that includes organization names, you can use the single search parameter to look for ROR records that match the organization names buried within those text strings. One such messy text string might be TKTKTK.

## Example

```curl
curl 'https://api.ror.org/v2/organizations?TKTKTK' | json_pp
```


# Other ways to match affiliations to ROR records

See our guide on [Matching organization names to ROR IDs](doc:matching) for more details on using the single search parameter and other methods, including third-party machine learning models, to match long, messy affiliation strings to ROR records.
