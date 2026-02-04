---
title: Match organization names to ROR IDs
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    This document provides guidance on matching organization names to ROR IDs
    using the ROR API, including different approaches like the affiliation
    parameter and query parameter methods. It also mentions using scripts, the
    ROR data dump, OpenRefine, and third-party tools for this purpose.
  robots: index
next:
  pages:
    - title: 'Video: Strategies for Matching Affiliation Strings to ROR IDs'
      type: link
      url: https://youtu.be/Tx5y7lX030U
    - title: ror-utilities matching and mapping scripts
      type: link
      url: https://github.com/ror-community/ror-utilities
    - title: ROR blog posts on matching
      type: link
      url: https://ror.org/categories/matching
    - title: ROR single search affiliation matching strategy demo
      type: link
      url: https://youtu.be/DC7mZSnECsQ
---
If you have a list, a spreadsheet, or a database of organization names or affiliation strings, there are several approaches to matching those text strings to ROR IDs. The best method to use depends on the amount and type of data you have, whether you'd like to write your own code, and whether you want to do large-scale automatic matching or small-scale human review.

Here are some common types of organization data:

**Organization name**

University of Pisa

**Organization name and location as structured data**

|        Name        | City | Country |
| :----------------: | :--: | :-----: |
| University of Pisa | Pisa |  Italy  |

**Unstructured affiliation string including sub-affiliation and address information**

Department of Civil and Industrial Engineering, University of Pisa, Largo Lucio Lazzarino 2, Pisa 56126, Italy

# Match organization names to ROR using OpenRefine

The ROR OpenRefine Reconciler is a fairly labor-intensive way of matching organization names or unstructured affiliation strings to ROR IDs, but it works well for those who have no more than a few thousand items to match to ROR IDs, those who want to have a high degree of control and oversight over the matching process, and those who do not want to write code.

[OpenRefine](https://openrefine.org) (formerly Google Refine) is a free, open source desktop tool for cleaning up messy data stored in common formats like CSV, XLSX, JSON, and XML. You can even use it to connect to SQL-based databases and Google Sheets.

See [ROR OpenRefine Reconciler](doc:openrefine-reconciler) for written usage instructions, screenshots, and a tutorial video.

# Match organization names to ROR IDs using the ROR API

The [ROR REST API](doc:rest-api) offers several ways to search ROR that all work differently and return different results. Choose the best method for your data.

> 📘 What kind of data do you have?
>
> * Organization identifiers (Wikidata, ISNI, Funder IDs, GRID) - use the [Query parameter](doc:api-query) and see our guide to [mapping other organization IDs to ROR IDs](doc:mapping)
> * Organization names only - use the [Query parameter](doc:api-query)
> * Organization names and locations as structured data - use the [Query parameter](doc:api-query) and [Filtering](doc:api-filtering) by location
> * Organization websites as structured data - use the [Advanced query parameter](doc:api-advanced-query)
> * Unstructured affiliation strings that often include sub-affiliations and addresses - use the [Affiliation parameter](doc:api-affiliation)

## Query approach

In cases where you have Wikidata, ISNI, Funder IDs, or GRID identifiers or when you have organization names stored as structured data, use the [Query parameter](doc:api-query) of the ROR API to match organizations to ROR IDs. This approach searches only the `names` and `external_ids` fields in ROR records and returns all matching records. Will return the same results as the ROR [Web search](doc:web-search).

For best results, search for an identifier, for keywords from the organization's name, or for the exact name of the organization surrounded by double quotation marks and if possible [filter](doc:api-filtering) the results by organization type and/or location. See also our guide to [Mapping other organization IDs to ROR IDs](doc:mapping).

You should also use the [Query parameter](doc:api-query) if you are building a user-facing form in which users type in keywords from an organization's name and then select a result. See [Create ROR-powered forms](doc:forms) for more information.

## Advanced query approach

In cases where you do not have organization identifiers or locations, but do have organization websites or Wikipedia pages stored as structured data, use the [Advanced query parameter](doc:api-advanced-query) of the ROR API to match organizations to ROR IDs. This approach allows you to search fields not indexed by the [Query parameter](doc:api-query) such as `domains` and `links`.

## Affiliation approach

In cases where you have complex, unstructured affiliation strings, use the [Affiliation parameter](doc:api-affiliation) of the ROR API to match these strings to a ROR ID for the organization. ROR and Crossref have done extensive research to design the affiliation parameter of the ROR API to match messy strings to ROR IDs precisely and at scale.

The affiliation matching service attempts to find the ROR record that is the most probable match for the given affiliation string; if it finds a likely candidate, it returns that result with a `chosen:true` indicator. Additional possibilities that might match the string are also included in results, listed in descending order by confidence score. Note that we do not recommend selecting matches by confidence score: use the `chosen:true` indicator instead.

> 📘 Retrieving active and inactive organizations
>
> By default, the ROR API returns only records with an active [status](doc:data-structure#status). Consider whether you also want to retrieve records with an inactive status; inactive records generally represent organizations that no longer operate. See <Anchor label="API filtering" target="_blank" href="doc:api-filtering">API filtering</Anchor> for details.
>
> Be aware too that inactive organizations may be succeeded by a new organization under a different name with a different ROR ID. If you do retrieve inactive organizations, check the `relationships` field of an inactive record to see if it has a [Successor organization](doc:data-structure#relationships).

# Match organization names to ROR IDs using the data dump

Instead of using the ROR API, you can use your own scripts or processing tools on the [ROR data dump](doc:data-dump) to match organization names to ROR IDs. Advantages of this approach include:

* Fine-grained control over matching criteria
* Faster processing in cases where you have many IDs to map
* No chance of error responses due to network interruptions or API

Remember, too, that you can run the ROR API locally with a copy of the ROR data dump. See instructions for installing the ROR API locally with Docker in the [README file of the ROR API GitHub repository](https://github.com/ror-community/ror-api/blob/master/README.md).

# Match organization names to ROR IDs using third-party tools

Several projects and researchers have developed scripts and/or machine learning and artificial intelligence tools that match textual organization information to ROR IDs. These tools are not officially supported by ROR, but we list them here in case you find them useful.

* Selected Python scripts that match organization names to ROR IDs area available in the [ror-utilities Github repository](https://github.com/ror-community/ror-utilities).

* [OpenAlex Institution Parsing](https://github.com/ourresearch/openalex-institution-parsing) by OurResearch

* [S2AFF - Semantic Scholar Affiliations Linker](https://github.com/allenai/S2AFF/) by the Allen AI Institute

* [RORRetriever](https://github.com/Metadata-Game-Changers/RORRetriever) by Metadata Game Changers

* [EMBL-EBI ROR Predictor prototype](https://gitlab.ebi.ac.uk/literature-services/public-projects/ROR-proto-EMBL) by EMBL-EBI for [Project FREYA](https://www.project-freya.eu/Plone/en)

* [dataESR affiliation matcher](https://github.com/dataesr/affiliation-matcher) developed by Anne L'Hôte and Eric Jeangirard for the French Ministry of Higher Education

* [OpenAlex ROR Predictor (gpu-based)](https://github.com/adambuttrick/openalex-ror-predictor) by Adam Buttrick

* [fastText ROR Predictor (cpu-based)](https://github.com/adambuttrick/ror-predictor-fasttext) by Adam Buttrick

* [lr_predictor](https://github.com/adambuttrick/lr_matches) by Adam Buttrick

* [ROR experimental affiliation matching](https://github.com/ror-community/affiliation-matching-experimental) - A collection of data and code for training models and experimenting with automatically matching affiliation strings to ROR IDs. Not production code, and not officially supported by ROR.

# Testing and training data

ROR has collected sets of data from Springer Nature, the American Physical Society, OpenAlex, and Crossref for testing and training affiliation matching strategies, and these datasets are openly available at [https://github.com/ror-community/affiliation-matching-experimental/tree/main/test_data](https://github.com/ror-community/affiliation-matching-experimental/tree/main/test_data). These datasets include affiliation text strings from production systems that have been matched to ROR IDs with varying levels of human review.

Crossref has also published a dataset of DOI metadata with over 140 million affiliation assertions from Crossref metadata records through March 2025 that has been used to test the ROR API [single search affiliation matching strategy](https://ror.readme.io/docs/api-affiliation#/single-search-strategy). It includes automatically-detected matches for over 94 million affiliation assertions.
