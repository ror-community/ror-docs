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


<Image src="https://files.readme.io/498d9274cae8a1c44e6da0a3ee6c09ee2a53d73d07a1444b2a2881553d6a92ad-ror-search-selector.png" alt="Select Search or Advanced." align="center" border={true} />

Note that in both modes, only records with an [Active status](https://ror.readme.io/docs/ror-data-structure#status) are returned by default, but Inactive and Withdrawn records can be returned by selecting the desired options.

## Wildcards and quotation marks

Both Search and Advanced modes support searching for partial words with the wildcard character `*` as well as searching for exact strings by surrounding a multi-word phrase with quotation marks `" "`.

## View details

The ROR search home page and results pages show only some information about ROR records. Clicking the **View details** button will take you to a single ROR record with extended information about that organization.


<Image src="https://files.readme.io/44b41bf0fb1aa33338afd155ade77002ae1110d035c1d6290162479a471d8a31-ror-search-results-view-details.png" alt="ROR search results list." align="center" border={true} />

<br />

On the single ROR record, you can see additional information such as other names by type and links to the ROR records of related organizations. At the bottom of the record are buttons that allow you to view the underlying record data in JSON format or to request a change to the record. The date the record was last modified is available at the bottom of the record on the right.&#x20;


<Image src="https://files.readme.io/cae5e60558873c0b4b90d43e71973e4e899a4fe2aa7c400bec48d3e792e5e088-ror-single-record.png" alt="ROR record with features circled." align="center" border={true} />

# Search

The ROR search default is designed to help you find organizations by name or by identifier. You can type or paste an organization name or identifier to see suggestions, then click a suggestion to jump straight to that record. To see all results for a search, click the magnifying glass icon or hit Enter.

## Jump straight to a suggestion

When you've typed or pasted at least three characters, the ROR search bar will offer a list of suggested organizations with their name, city, and country. Click a suggestion to go directly to that record.&#x20;


<Image src="https://files.readme.io/89f335a0279b5a860e108af4d9db24eaa6b283ab686cd1538d7db59fb7ad3de8-ror-search-suggestions.gif" alt="Suggestions for the word Lincoln." align="center" border={true} />

## Go to the results list

If you'd like to see the whole results list, you can click the magnifying glass icon or hit Enter after typing or pasting your search terms to bypass the suggestions.&#x20;


<Image src="https://files.readme.io/6edf5f06da4430289cb6bfcbee856f9161e65f586a522ab1da06c156e0505d9c-ror-search-results-list.gif" alt="Search for Lincoln and go to results list." align="center" border={true} />

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


<Image src="https://files.readme.io/a0082843c0d52f3c80187db16cd934daeb1765bc4e316b0b9c58772e04088550-ror-search-filters.png" alt="Results using filters." align="center" border={true} />


## Example - quotation marks

Search for an active organization with the exact phrase "solar energy" in the name by surrounding it with quotation marks.


<Image src="https://files.readme.io/f57904a052b26888d15b917cd185aff8e334bac67a7f18bd19790b03f2f8e5a0-ror-search-quotation-marks.png" alt="Results of an exact phrase search." align="center" border={true} />

<br />

Note that searching for the term "solar energy" _without_ using quotation marks produces many more results, since the ROR web search is looking for records with _either_ the term "solar" _or_ the term "energy" in the organization name.


<Image src="https://files.readme.io/2ec30c431d4f3bb626e41ddd5a8d461792875c0458d721030b081ab4a3ca9d3e-ror-search-no-quotation-marks.png" alt="Results of a keyword search." align="center" border={true} />

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
- **Related organization(s)** - Search for an organization and select it to find organizations [related](https://ror.readme.io/docs/relationships) to it in any way. This condition applies to all entities in the selected organization's `relationships` field, including Related, Child, Parent, Predecessor, and Successor organizations.
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

Conditions can be combined with the Boolean operators AND, OR, or NOT.&#x20;

* AND - All chosen conditions are present in each of the resulting records. Produces fewer results.&#x20;
* OR - Any chosen condition is present in the resulting records. Produces more results.&#x20;
* NOT - Records with the condition are excluded. Produces fewer results. **Note that if you have multiple conditions connected with OR before adding a NOT, the NOT condition will apply only to the immediately previous condition.&#x20;**

## Example - city

Find all organizations whose city is Raleigh.&#x20;


<Image src="https://files.readme.io/c755cc4bd1e00c573ebaf1caf11f3b5c8dc0fe31ffdad1d135392d30877684f4-ror-search-city.png" align="center" border={true} />

<br />

The search returns 138 organizations whose city is "Raleigh." Note that neither country nor continent is specified in the search.&#x20;


<Image src="https://files.readme.io/9fb4173e8ae7dadad3c35bc02f9f0e3ad56f8c6c92ee8b69a4f055ed12fa2edc-ror-search-city-results.png" border={true} />

<br />

## Example - AND

Find all organizations whose city is Raleigh AND where the record Creation date is on or after January 1, 2026.&#x20;


<Image src="https://files.readme.io/2bd3b0e813f09f2d3ffd1c8a371426160df93d664a3a8207897df0e8058159ca-ror-search-city-and-created-date.png" border={true} />


<br />

The search returns 10 organizations where the city is Raleigh and the record was created on or after January 1, 2026.&#x20;


<Image src="https://files.readme.io/30644780bfcc1d4243a0ff170262d436fae017eda8673c0c61d4eba2e0d29bac-ror-search-city-and-created-date-results.png" border={true} />

<br />

Find all organizations whose city is Raleigh AND Chapel Hill.&#x20;


<Image src="https://files.readme.io/6648372b1702e08f812c7b0f7d7c8e3df32a51491dbf6c2c4851905a6a50579c-ror-search-city-and-city.png" border={true} />


<br />

The search returns 0 results because there are no organizations located both in Raleigh and in Chapel Hill. Note that nearly all ROR records are located in only a single city.&#x20;


<Image src="https://files.readme.io/9a67c86aa85f8f719dc1306e5b24ce4856ce95560840db713416448a77810972-ror-search-city-and-city-results.png" border={true} />

<br />

Find all organizations whose record includes both the Facility and the Funder type.&#x20;


<Image src="https://files.readme.io/6b000d20c7cb1b5aa70ef2a5407b21b6d47919537e1ce7a342f8afa85abf1820-ror-search-type-and-type.png" alt="Two Type conditions." border={true} />

<br />

The search returns 2,431 records whose Organization type includes both Funder and Facility. Some results may include additional Organization types as well.&#x20;


<Image src="https://files.readme.io/24560322e635f20e8625d6850fba3d220aed7d06dc4a906af4d371bd5f1bd3f1-ror-search-type-and-type-results.png" alt="Results for combined Type search." border={true} />

<br />

## Example - OR

Find all organizations whose city is Raleigh OR Chapel Hill OR Durham.&#x20;


<Image src="https://files.readme.io/bbcb09481492ed8c3b14e882cf6738b163aa66225bd970c81c121c1df8fc74ca-ror-search-or.png" alt="Combine searches with OR." align="center" border={true} />

<br />

The search returns 415 organization records where the city is Raleigh, Chapel Hill, or Durham. Note again that neither the country nor the continent is specified in the search.&#x20;


<Image src="https://files.readme.io/f04059e74e5ca80c1ffa057598c407b787dfbe5a48cb36e51a73ebcccacc5702-ror-search-city-or-city-results.png" border={true} />

<br />

Find all organizations whose type is either Funder OR Facility.&#x20;


<Image src="https://files.readme.io/814e6ca92ce8f482ab916d00204dd802e2dda4fdde5a2584596c709a593a48ea-ror-search-type-or-type.png" alt="Combine type searches with OR." border={true} />

<br />

The search returns 36,501 results where the organization type includes either Funder or Facility. Some results may include both types or additional organization types.


<Image src="https://files.readme.io/73828e078b509d13ea875ac05871e035433a1df15688d0f8bd7535b49aaccb2c-ror-search-type-or-type-results.png" alt="Results for two type searches combines with OR." border={true} />

<br />

## Example - NOT&#x20;

Find all organizations whose record includes both the Facility and the Funder type but NOT the Government type.&#x20;


<Image src="https://files.readme.io/c65a0a784f6bd237081f77c17c7116ed4a0a9564d1f4886e8ea4bd75c31aa3a1-ror-search-types-and-not.png" alt="Two AND conditions and a NOT condition." border={true} />


<br />

The search returns 2,409 results for records that include both the Facility and the Funder type but do NOT include the Government type.


<Image src="https://files.readme.io/23cda2b05ed85012d87c340b065b93bf7dbe538290926f1ee31ee5d929b5e811-ror-search-types-and-not-results.png" alt="Search results for two AND conditions and a NOT." border={true} />


<br />

Find all organizations whose city is Raleigh OR Chapel Hill OR Durham that are NOT on the continent of North America.&#x20;


<Image src="https://files.readme.io/cb624da567dd30d96ebd35112ad1cd5a5ca2d0bb84549fed0e4b9657e7432892-ror-search-cities-not-continent.png" border={true} />


<br />

The search returns 21 organizations whose city is Durham that are not located in North America. **Note that if you have multiple conditions connected with OR before adding a NOT, the NOT condition will apply only to the immediately previous condition.** Therefore, this search is equivalent to a search for organizations whose city is Durham that are not located in North America: the first two conditions pertaining to Raleigh and Chapel Hill become irrelevant.&#x20;


<Image src="https://files.readme.io/afe7dad46b136fcfbba8a8f4187af9ac8a61414549ec5827425e30b86d44bfcf-ror-search-cities-not-continent.png" border={true} />


<br />

Find all organizations whose city is Raleigh OR Durham OR Chapel Hill that are NOT on the continent of North America.&#x20;


<Image src="https://files.readme.io/b306bc57e32e0d5ab89aa00cdbc6afd4b5d8005d601bf83be8b8e845b6edc50e-ror-search-cities-not-continent-failure.png" border={true} />


<br />

The search returns 0 results, since the only city named Chapel Hill is in North America. **Note that if you have multiple conditions connected with OR before adding a NOT, the NOT condition will apply only to the immediately previous condition.** Therefore, this search is equivalent to a search for organizations whose city is Chapel Hill that are not located in North America: the first two conditions pertaining to Raleigh and Durham become irrelevant.


<Image src="https://files.readme.io/7492b307b7ab1273019d271690ed249e86d5943f91945f0a39a292019d82373d-ror-search-cities-not-continent-chapel-hill-results.png" alt="0 organizations found." border={true} />

## Example - wildcard

Find all organizations whose city name begins with Chapel.&#x20;

<br />

<br />

<br />

<br />

<br />
