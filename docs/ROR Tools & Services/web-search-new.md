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

Both Search and Advanced modes support searching for partial words with the wildcard character `*` as well as searching for exact strings by surrounding a multi-word phrase with quotation marks `" "`.

## View details

The ROR search home page and results pages show only some information about ROR records. Clicking the **View details** button will take you to a single ROR record with extended information about that organization.


<Image src="https://files.readme.io/44b41bf0fb1aa33338afd155ade77002ae1110d035c1d6290162479a471d8a31-ror-search-results-view-details.png" alt="ROR search results list." align="center" caption="Search results list with View details button leading to single ROR record." border={true} />

On the single ROR record, you can see additional information such as other names by type and links to the ROR records of related organizations. At the bottom of the record are buttons that allow you to view the underlying record data in JSON format or to request a change to the record. The date the record was last modified is available at the bottom of the record on the right.&#x20;


<Image src="https://files.readme.io/cae5e60558873c0b4b90d43e71973e4e899a4fe2aa7c400bec48d3e792e5e088-ror-single-record.png" alt="ROR record with features circled." align="center" caption="Single ROR record with actions and information at the bottom of the record." border={true} />

# Search

The ROR search default is designed to help you find organizations by name or by identifier. You can type or paste an organization name or identifier to see suggestions, then click a suggestion to jump straight to that record. To see all results for a search, click the magnifying glass icon or hit Enter.

## Jump straight to a suggestion

When you've typed or pasted at least three characters, the ROR search bar will offer a list of suggested organizations with their name, city, and country. Click a suggestion to go directly to that record.&#x20;


<Image src="https://files.readme.io/89f335a0279b5a860e108af4d9db24eaa6b283ab686cd1538d7db59fb7ad3de8-ror-search-suggestions.gif" alt="Suggestions for the word Lincoln." align="center" caption="Begin searching and then click a suggestion to go quickly and directly to a single ROR record." border={true} />


## Go to the results list

If you'd like to see the whole results list, you can click the magnifying glass icon or hit Enter after typing or pasting your search terms to bypass the suggestions.&#x20;

<Image src="https://files.readme.io/6edf5f06da4430289cb6bfcbee856f9161e65f586a522ab1da06c156e0505d9c-ror-search-results-list.gif" alt="Search for Lincoln and go to results list." align="center" caption="Begin searching and then click the search icon to go to the results list." border={true} />

## Filtering

You can choose filters before or after you search to fine-tune your search results. These filters are available in the sidebar on the ROR search home page, on results lists, and on single ROR record pages.&#x20;

- **Record status** - Each ROR record has a [status](https://ror.readme.io/docs/ror-data-structure#status) of active, inactive, or withdrawn. By default, all searches return only active records. This filter allows you to include inactive or withdrawn records as well or instead of active records in your search results.&#x20;
- **Organization type&#x20;**- Each ROR record has at least one [organization type](https://ror.readme.io/docs/ror-data-structure#types). This filter allows you to choose the organization type(s) in your search results.&#x20;
- **Country** - Each ROR record includes the country of the organization's [location](https://ror.readme.io/docs/ror-data-structure#locations). Country names are derived from <Anchor target="_blank" href="https://geonames.org">GeoNames</Anchor>. This filter allows you to include organizations located in specific countries in your results. If you do not see a country among the available options, use the "Search countries" field in the filter to find and select a country.&#x20;
- **Continent** - Each ROR record includes the continent of the organization's [location](https://ror.readme.io/docs/ror-data-structure#locations). Continent names are derived from <Anchor target="_blank" href="https://geonames.org">GeoNames</Anchor>. This filter allows you to include organizations located on one or more specific continents in your results.&#x20;

<Callout icon="far fa-circle-info" theme="info">
  ## Remember that the default search does not include all elements of a ROR record

  In Search mode, the ROR web search searches only three elements: `names`, `external_ids`, and `id`, which means that only organization names and identifiers should be used as search terms. To find organizations by location, type, or other criteria, use the sidebar filters or Advanced mode.
</Callout>

## Example - filters

Search for inactive and withdrawn records for organizations in Finland with the word "energy" in the name.


<Image src="https://files.readme.io/a0082843c0d52f3c80187db16cd934daeb1765bc4e316b0b9c58772e04088550-ror-search-filters.png" alt="Results using filters." align="center" caption="Example result from a ROR search using filters for Record status and Country." border={true} />


## Example - quotation marks

Search for an active organization with the exact phrase "solar energy" in the name by surrounding it with quotation marks.


<Image src="https://files.readme.io/f57904a052b26888d15b917cd185aff8e334bac67a7f18bd19790b03f2f8e5a0-ror-search-quotation-marks.png" alt="Results of an exact phrase search." align="center" caption="Example results from a ROR search using quotation marks to search for an exact text string." border={true} />


Note that searching for the term "solar energy" _without_ using quotation marks produces many more results, since the ROR web search is looking for records with _either_ the term "solar" _or_ the term "energy" in the organization name.


<Image src="https://files.readme.io/2ec30c431d4f3bb626e41ddd5a8d461792875c0458d721030b081ab4a3ca9d3e-ror-search-no-quotation-marks.png" alt="Results of a keyword search." align="center" caption="Example results from a multi-term ROR search without quotation marks.&#x20;" border={true} />

# Advanced

Advanced search mode is designed to help you construct precise, complex, multi-field searches of the ROR registry. Add conditions such as City, Website, and Modified Date and combine them with AND, OR, or NOT.

The Country, Continent, Organization type, and Record status conditions in Advanced search mode operate on the same fields and in similar ways as the sidebar filters, but they function separately, so choosing (for example) a specific Country in Advanced mode will not affect the Country sidebar options. Results from Advanced searches can be further limited using the sidebar filters.&#x20;

## Conditions

You can choose up to 10 of the following conditions in Advanced search mode. Conditions are based on fields from the ROR [data structure.](https://ror.readme.io/docs/ror-data-structure)

- **Name** - Enter an organization's name. Note that the Name condition in Advanced search mode does not offer suggestions. &#x20;
- **Country** - Select a country.
- **Continent** - Select a continent.
- **City&#x20;**- Enter a city.&#x20;
- **Organization type** - Select an organization type.&#x20;
- **Record status** - Select a record status.
- **Related organization(s)** - Search for an organization and select it to find any organizations [related](https://ror.readme.io/docs/relationships) to it in any way. This condition will return all values listed in the selected organization's `relationships` field, including Related, Child, Parent, Predecessor, and Successor organizations.
- **Relationship type** - Select a relationship type to include or exclude records that include a [relationship](https://ror.readme.io/docs/relationships) of that type in the `relationships` field. For instance, searching for records where the "Parent" relationship type is included will return organizations that _have_ a parent as distinct from organizations that _are_ a parent. Note, however, that any one organization can both have a parent and be a parent.&#x20;
- **Funder ID** - Enter a Crossref Funder ID in the format 100009717 or 10.13039/100009717.&#x20;
- **ISNI ID** - Enter an ISNI ID in the format 0000000119575136 or 0000 0001 1957 5136.&#x20;
- **Wikidata ID** - Enter a Wikidata ID in the format Q643674 or 643674.&#x20;
- **GRID ID** - Enter a GRID ID in the format grid.452594.b.&#x20;
- **Website** - Enter a website such as `https://israbat.ac.ma/`.&#x20;
- **Domain** - Enter a web domain such as `ucla.edu`.
- **Year established** - Enter the year an organization was established. This must match the established year exactly.
- **Created date** - Search for records created on a specific date, on or before a date, on or after a date, or between two dates. Enter dates in ISO format: YYYY-MM-DD.&#x20;
- **Modified date** - Search for records modified on a specific date, on or before a date, on or after a date, or between two dates. Enter dates in ISO format: YYYY-MM-DD.
- **GeoNames ID** - Search for records in a location using a <Anchor target="_blank" href="https://www.geonames.org/">GeoNames</Anchor> ID in the format 2988507.&#x20;

## Combining conditions

The first condition you choose in Advanced mode will find all organizations where that condition is true. You can add up to 9 additional conditions by selecting them from the list on the left of the Advanced window.&#x20;

Conditions can be combined with the Boolean operators AND, OR, or NOT. **Note that the Boolean operator applies only to the most recent previous condition added, not to any earlier conditions in the same query.**&#x20;

* AND - Both the first and the second chosen conditions are present in each of the resulting records. Produces fewer results.&#x20;
* OR - Either the first or the second chosen conditions are present in the resulting records. Produces more results.&#x20;
* NOT - The first chosen condition is present in the resulting records, but records with the second condition are excluded. Produces fewer results.&#x20;

## Example - city

Find all organizations whose city is Raleigh.&#x20;

## Example - AND

<br />

## Example - OR

Find all organizations whose city is Raleigh OR Durham OR Chapel Hill.&#x20;


<Image src="https://files.readme.io/bbcb09481492ed8c3b14e882cf6738b163aa66225bd970c81c121c1df8fc74ca-ror-search-or.png" alt="Combine searches with OR." align="center" caption="Example search with the OR operator." border={true} />


## Example - NOT&#x20;

<br />

## Example - wildcard

<br />

<br />

<br />

<br />
