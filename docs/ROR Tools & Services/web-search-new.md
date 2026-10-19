---
title: Web search (new)
deprecated: false
hidden: true
metadata:
  robots: index
---
Search the ROR registry in your web browser at [https://ror.org/search](https://ror.org/search). Find organizations by name or ID using the default Search or construct complex queries using Advanced Search.

<Callout icon="🚧" theme="warn">
  ## Don't send requests from an application to the ROR web search!

  If you're integrating ROR search into a web application, use the free and open [REST API](https://ror.readme.io/docs/rest-api). Please don't send search requests to [https://ror.org/search](https://ror.org/search) from an application! The web search is for humans.
</Callout>

# About the web search

The ROR web search has two modes: **Search** and **Advanced Search**_._ The default Search uses the [query parameter](https://ror.readme.io/docs/api-query) of the ROR API, which is designed to help you find one or more organizations by name or identifier. From Search, you can go directly to a suggested single ROR record or you can see all results. Results can be filtered using sidebar options for **Record status, Organization type, Country, or Continent.**

Selecting Advanced Search allows you to construct complex searches with multiple conditions that can be combined with Boolean operators **AND, OR, and NOT.&#x20;**&#x54;he Advanced Search uses the [Advanced query parameter](https://ror.readme.io/docs/api-advanced-query) of the ROR API.&#x20;

<Image src="https://files.readme.io/498d9274cae8a1c44e6da0a3ee6c09ee2a53d73d07a1444b2a2881553d6a92ad-ror-search-selector.png" align="center" border={true} />


Note that in both modes, only records with an [Active status](https://ror.readme.io/docs/ror-data-structure#status) are returned by default, but Inactive and Withdrawn records can be returned by selecting the desired options.

## Wildcards and quotation marks

Both modes support the wildcard character `*` as well as searching for exact strings by surrounding a term with quotation marks `" "`.

## View details

The ROR search home page and results pages show only some information about ROR records. Clicking the **View details** button will take you to a single ROR record with extended information about that organization.


<Image src="https://files.readme.io/d3d2c20dac5d9e1caa47a663bd1aabc61d963370688f8d0c5e641501ad1adf8f-ror-search-results-view-details.png" align="center" border={true} />

<br />

On the single ROR record, you can see all&#x20;

<br />

# Search

<br />

## Filtering

### Record status

### Organization type

### Country

### Continent

<br />

<Callout icon="far fa-circle-info" theme="info">
  ## Remember that the default search does not include all elements of a ROR record

  In Search mode, the ROR web search searches only three elements: `names`, `external_ids`, and `id`, which means that only organization names and identifiers should be used as search terms. To find organizations by website, location, type, or other criteria, use the sidebar filters or Advanced Search mode.
</Callout>

## Examples 

<br />

# Advanced Search

## Boolean 

## Conditions

### Name

### Country

### Continent

### City

### Organization type

### Record status

### Related organization

### Relationship type

### Funder ID

### ISNI ID

### Wikidata ID

### Funder ID

### GRID ID

### Website

### Domain

### Year established

### Created date

### Modified date

### GeoNames ID

<br />

## Examples