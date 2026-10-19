---
title: Web searcn (new)
deprecated: false
hidden: false
metadata:
  robots: index
---
Search the ROR registry in your web browser at <https://ror.org/search>. Find organizations by name keywords in any language, filter results, or construct complex searches. 

<Callout icon="🚧" theme="warn">
  ## Don't send requests from an application to the ROR web search!

  If you're integrating ROR search into a web application, use the free and open [REST API](https://ror.readme.io/docs/rest-api). Please don't send search requests to [https://ror.org/search](https://ror.org/search) from an application! The web search is for humans.
</Callout>

# About the web search

The ROR web search has two modes: *Search* and *Advanced Search.* The default Search uses the [query parameter of the ROR API](https://ror.readme.io/docs/api-query), which is designed to help you find one or more organizations by name or identifier. From Search, you can go directly to a suggested single ROR record or you can see all results. Results can be filtered using sidebar options for *Record status*, *Organization type*, *Country*, or *Continent.*

Selecting Advanced Search allows you to construct complex searches with multiple conditions that can be combined with Boolean operators *AND*, *OR*, and *NOT.* 




Note that in both modes, only records with an [Active status](https://ror.readme.io/docs/ror-data-structure#status) are returned by default, but Inactive and Withdrawn records can be returned by selecting the desired record status options. 

<Callout icon="🚧" theme="warn">
## Remember that the default search does not include all elements of a ROR record

In Search mode, the ROR web search searches only three elements: `names`, `external_ids`, and `id`, which means that only organization names and identifiers should be used as search terms. To find organizations by website, location, type, or other criteria, use the sidebar filters or Advanced Search mode. 
</Callout>

## Wildcards and quotation marks

Both modes support wildcards * and ? as well as searching for exact strings by surrounding a term with quotation marks. 

# Search mode


## Filtering

### Record status

### Organization type

### Country

### Continent

## Examples 


# Advanced Search mode


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


## Examples
