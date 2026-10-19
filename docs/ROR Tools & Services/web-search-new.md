---
title: Web search (new)
deprecated: false
hidden: true
metadata:
  robots: index
---
Search the ROR registry in your web browser at [https://ror.org/search](https://ror.org/search). Find organizations by name or ID using the default Search, or construct complex queries using Advanced Search.

<Callout icon="🚧" theme="warn">
  ## Don't send requests from an application to the ROR web search!

  If you're integrating ROR search into a web application, use the free and open [REST API](https://ror.readme.io/docs/rest-api). Please don't send search requests to [https://ror.org/search](https://ror.org/search) from an application! The web search is for humans.
</Callout>

# About the web search

The ROR web search has two modes: **Search** and **Advanced**_._ The default Search uses the [query parameter](https://ror.readme.io/docs/api-query) of the ROR API, which is designed to help you find one or more organizations by name or identifier. From Search, you can go directly to a suggested single ROR record, or you can see all results. Results can be filtered using sidebar options for **Record status, Organization type, Country,** or **Continent.**

Selecting Advanced allows you to construct complex searches with multiple conditions that can be combined with Boolean operators **AND, OR, and NOT.&#x20;**&#x41;dvanced mode uses the [Advanced query parameter](https://ror.readme.io/docs/api-advanced-query) of the ROR API.&#x20;


<Image src="https://files.readme.io/498d9274cae8a1c44e6da0a3ee6c09ee2a53d73d07a1444b2a2881553d6a92ad-ror-search-selector.png" alt="Select Search or Advanced." align="center" caption="ROR search mode selector." border={true} />

Note that in both modes, only records with an [Active status](https://ror.readme.io/docs/ror-data-structure#status) are returned by default, but Inactive and Withdrawn records can be returned by selecting the desired options.

## Wildcards and quotation marks

Both modes support the wildcard character `*` as well as searching for exact strings by surrounding a term with quotation marks `" "`.

## View details

The ROR search home page and results pages show only some information about ROR records. Clicking the **View details** button will take you to a single ROR record with extended information about that organization.


<Image src="https://files.readme.io/44b41bf0fb1aa33338afd155ade77002ae1110d035c1d6290162479a471d8a31-ror-search-results-view-details.png" alt="ROR search results list." align="center" caption="Search results list with View details button leading to single ROR record." border={true} />

On the single ROR record, you can see additional information such as other names by type and links to the ROR records of related organizations. At the bottom of the record are buttons that allow you to view the underlying record data in JSON format or to request a change to the record. The date the record was last modified is available at the bottom of the record on the right.&#x20;


<Image src="https://files.readme.io/cae5e60558873c0b4b90d43e71973e4e899a4fe2aa7c400bec48d3e792e5e088-ror-single-record.png" alt="ROR record with features circled." align="center" caption="Single ROR record with View JSON, Suggest a change, and Record last modified." border={true} />

# Search

The ROR search default is designed to help you find organizations by name or by identifier. You can type or paste an organization name or identifier to see suggestions, then click a suggestion to jump straight to that record. To see all results for a search, click the magnifying glass icon or hit Enter.

## Jump straight to a suggestion

When you've typed or pasted at least three characters, the ROR search bar will offer a list of suggested organizations with their name, city, and country. Click a suggestion to go directly to that record.&#x20;


<Image src="https://files.readme.io/89f335a0279b5a860e108af4d9db24eaa6b283ab686cd1538d7db59fb7ad3de8-ror-search-suggestions.gif" alt="Suggestions for the word Lincoln." align="center" caption="Begin searching and then click a suggestion to go quickly and directly to a single ROR record." border={true} />


## Go to the results list

If you'd like to see the whole results list, you can click the magnifying glass or hit Enter to bypass the suggestions.&#x20;


<Image src="https://files.readme.io/6edf5f06da4430289cb6bfcbee856f9161e65f586a522ab1da06c156e0505d9c-ror-search-results-list.gif" alt="Search for Lincoln and go to results list." align="center" caption="Begin searching and then click the search icon to go to the results list." border={true} />

## Filtering

<br />

- **Record status**
- **Organization type**
- **Country**
- **Continent**

<br />

<Callout icon="far fa-circle-info" theme="info">
  ## Remember that the default search does not include all elements of a ROR record

  In Search mode, the ROR web search searches only three elements: `names`, `external_ids`, and `id`, which means that only organization names and identifiers should be used as search terms. To find organizations by website, location, type, or other criteria, use the sidebar filters or Advanced Search mode.
</Callout>

## Examples 

<br />

# Advanced

<br />

## Boolean 

<br />

## Conditions

<br />

- **Name**
- **Country**
- **Continent**
- **City**
- **Organization type**
- **Record status**
- **Related organization**
- **Relationship type**
- **Funder ID**
- **ISNI ID**
- **Wikidata ID**
- **Funder ID**
- **GRID ID**
- **Website**
- **Domain**
- **Year established**
- **Created date**
- **Modified date**
- **GeoNames ID**

<br />

## Examples