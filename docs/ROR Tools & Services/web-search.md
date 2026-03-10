---
title: Web search
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR web search
  description: >-
    The document provides instructions for and technical details about the
    web-based Research Organization Registry search tool at
    https://ror.org/search with the option to filter results by record status. 
  robots: index
next:
  pages:
    - title: ROR web search
      type: link
      url: https://ror.org/search
---
Search the ROR registry in your web browser at [https://ror.org/search](https://ror.org/search). Find organizations by name-related keywords in any language, by acronyms, or by corresponding external identifiers.

> 🚧 Don't send requests from an application to the ROR web search!
>
> If you're integrating ROR search into a web application, use the free and open [REST API](doc:rest-api). Please don't send search requests to [https://ror.org/search](https://ror.org/search) from an application! The web search is for humans.

# About the web search

The ROR web search uses version 2 of the [ROR REST API](doc:rest-api) and performs searches using the [query parameter](doc:api-query), which is optimized for searching for an organization by keywords in its name. Advanced searches of other metadata fields such as an organization's location or website can be performed using the [Advanced query parameter](doc:api-advanced-query) of the ROR API. Only active organizations are returned by default: use the Record status filter widget to retrieve inactive and withdrawn organizations.

> 🚧 Remember that the web search does not search all fields
>
> The ROR web search searches only the `names` field, which includes acronyms, aliases, and names in various languages, plus the `external_ids` field.  Results from keyword searches using the query parameter **do not include** values from fields such as `links` and `locations`. To find organizations by website, location, or other criteria, use [Filtering](doc:api-filtering) or the [Advanced query parameter](doc:api-advanced-query).

# Keywords

Search for an organization by keywords in its name.

## Example

Search for active organizations with the keyword "Solar" in the name.

<Image align="center" alt="Results list in ROR web search from keyword search" border={true} caption="Beginning of results list from keyword search" src="https://files.readme.io/809eb8b-Screenshot_2024-04-03_at_8.22.12_PM.png" />

# Record status

Search results include only records with a status of `active` by default. Use the Record status filter widget in the left column to include records with a status of `inactive` and/or `withdrawn`.

Records with a status of `inactive` or `withdrawn` include a banner in the top right and link(s) to Successor organization(s), if applicable. [Learn more about record status in ROR](doc:ror-data-structure#status).

## Example

Search for inactive and withdrawn records with the word "energy" in the name.

<Image align="center" alt="List of inactive and withdrawn ROR records" border={true} caption="Beginning of ROR web search results list with inactive and withdrawn filters applied" src="https://files.readme.io/652ba17-Screenshot_2024-04-12_at_10.17.40_AM.png" />

# Exact strings

Search for an exact phrase in an organization name by surrounding it with quotation marks.

## Example

Search for an active organization with the exact phrase "solar energy" in the name by surrounding it with quotation marks. .

<Image align="center" alt="Results list from ROR web search for exact phrase search" border={true} caption="Beginning of results list from exact phrase search" src="https://files.readme.io/724fc38-Screenshot_2024-04-03_at_8.25.21_PM.png" />

Note that searching for the phrase "solar energy" **without** using quotation marks produces many more results, since the ROR web search is looking for records with _either_ the term "solar" _or_ the term "energy" in the organization name.

<Image align="center" alt="Results from multiple keyword search in ROR web search" border={true} caption="Beginning of results list from multiple keyword search" src="https://files.readme.io/eb65a74-Screenshot_2024-04-03_at_8.29.30_PM.png" />

# Identifiers

Search for the ROR record that corresponds to a given GRID ID, Wikidata ID, or Crossref Open Funder Registry ID by surrounding the ID with quotation marks.

## Example - GRID

Find the active ROR record that correponds to GRID ID grid.11780.3f.

<Image align="center" border={true} src="https://files.readme.io/3341162-Screenshot_2024-04-03_at_8.53.19_PM.png" className="border" />

## Example - Funder ID

Find the active ROR record that corresponds to Funder ID 501100003246.

<Image align="center" border={true} src="https://files.readme.io/0df8031-Screenshot_2024-04-03_at_8.34.34_PM.png" className="border" />

## Example - ISNI

Find the active ROR record that corresponds to ISNI 0000 0004 9155 2707.

<Image align="center" alt="ROR record for Universidade Federal de Rondonópolis." border={true} src="https://files.readme.io/ec7dedf0015052d99870fc6c41c5deb9d25db4ccb7ea3a13fec904a2504e60ef-Screenshot_2025-08-13_at_11.28.52_AM.png" className="border" />

# Detail view

Clicking on either the ROR ID or on "View details" will take you to the individual landing page for the record with additional details about the record. The detailed view will show the full list of other names for the organization by type and the full list of and links to related organizations in ROR. Note that the URL for the record landing page is exactly the same as the [ROR ID](doc:identifier).

On the ROR record landing page, you can choose to view the underlying JSON data by clicking "See JSON view for full record data".

<Image align="center" alt="ROR record for Baystate Medical Center" border={true} caption="Landing page for ROR ID [https://ror.org/04jq4p608](https://ror.org/04jq4p608)" src="https://files.readme.io/4d2876e-Screenshot_2024-04-03_at_3.09.03_PM.png" />
