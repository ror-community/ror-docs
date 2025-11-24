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

**Organization nam**

University of Pisa

**Organization name and location as structured data**

|        Name        | City | Country |
| :----------------: | :--: | :-----: |
| University of Pisa | Pisa |  Italy  |

<br />

**Unstructured affiliation string including sub-affiliation and address information**

Department of Civil and Industrial Engineering, University of Pisa, Largo Lucio Lazzarino 2, Pisa 56126, Italy

## Match organization names to ROR using OpenRefine

The ROR OpenRefine Reconciler is a fairly labor-intensive way of matching organization names and unstructured affiliation strings to ROR IDs, but it works well for those who have no more than a few thousand items to match to ROR IDs, those who want to have a high degree of control and oversight over the matching process, and those who do not want to write code.

[OpenRefine](https://openrefine.org) (formerly Google Refine) is a free, open source desktop tool for cleaning up messy data stored in common formats like CSV, XLSX, JSON, and XML. You can even use it to connect to SQL-based databases and Google Sheets.

See [ROR OpenRefine Reconciler](doc:openrefine-reconciler) for written usage instructions, screenshots, and a tutorial video.

## Match organization names to ROR IDs using the ROR API

The [ROR API](doc:api-about) offers several ways to search ROR that all work differently and return different results. Choose the best method for your data.

### What kind of data do you have?

* Organization identifiers - see [mapping] and use the query parameter
* Organization names only - use the query parameter
* Organization names and locations stored separately - use the query parameter
* Organization names and websites stored separately - use the advanced query parameter
* Unstructured affiliation strings that often include sub-affiliations and addresses - use the affiliation parameter
* **[Query parameter](#query-parameter-approach)** `/organizations?query=`: Searches `names` and `external_ids` fields only in ROR records and returns all matching records. Does NOT include a matching score or true/false indicator of whether the score is high enough to be considered a reliable match. Will return the same results as the [Web search](doc:web-search).

For cases where you have very common organization names that are stored separately from any other , use the [query parameter approach](#query-parameter-approach) to look for keywords from the organization's name or the exact name of the organization surrounded by double quotation marks, and consider using filters for organization type and country. Examples:

* Ministry of Health

* National Research Council

* York University

* **[Affiliation parameter](#affiliation-parameter-approach)**  `/organizations?affiliation=`:  Searches `names` field using different search algorithms and returns the best match(es) based on matching score. Includes matching score and true/false indicator of whether the score is high enough to be considered a reliable match. Produces about 85% correct matches in our tests; individual implementations may see better or worse results.

Examples of organization names and affiliation information as text strings and the corresponding ROR ID:

* University of Haifa > [https://ror.org/02f009v59](https://ror.org/02f009v59)
* University of ManchesterGreater Manchester Mental Health NHS Foundation TrustNational Institute for Health Research (NIHR) Greater Manchester Patient Safety Translational Research Centre > [https://ror.org/05sb89p83](https://ror.org/05sb89p83) .
* Atmospheric Sciences and Global Change Division, Pacific Northwest National Laboratory, Richland, WA, USA > [https://ror.org/05h992307](https://ror.org/05h992307)

For cases where you have relatively unique organization names or full affiliation strings, use the [affiliation parameter approach](#affiliation-parameter-approach). Works for both English and non-English name variations. Examples:

* Incorporated Research Institutions for Seismology (IRIS)
* Universitätsbibliothek der Ludwig-Maximilians-Universität München
* Department of Civil and Industrial Engineering, University of Pisa, Largo Lucio Lazzarino 2, Pisa 56126, Italy

> 📘 Retrieving active and inactive organizations
>
> By default, the ROR API returns only records with an active [status](doc:data-structure#status): `status: "active"`. Consider whether you also want to retrieve records with an inactive status; inactive records generally represent organizations that no longer operate. See [API filtering](doc:api-filtering) for details.
>
> Be aware too that inactive organizations may be succeeded by a new organization under a different name with a different ROR ID. If you do retrieve inactive organizations, check the `relationships` field of an inactive record to see if it has a [Successor organization](doc:data-structure#relationships).

## Match organization names to ROR IDs using the data dump

Instead of using the ROR API, you can use your own scripts or processing tools on the [ROR data dump](doc:data-dump) to match organization names to ROR IDs. Advantages of this approach include:

* Fine-grained control over matching criteria
* Faster processing in cases where you have many IDs to map
* No chance of error responses due to network interruptions or API

Remember, too, that you can run the ROR API locally with a copy of the ROR data dump. See instructions for installing the ROR API locally with Docker in the [README file of the ROR API GitHub repository](https://github.com/ror-community/ror-api/blob/master/README.md).

## Match organization names to ROR IDs using third-party tools

Several projects and researchers have developed scripts and/or machine learning and artificial intelligence tools that match textual organization information to ROR IDs. Some of these tools are fast and can work with large amounts of data with accuracy rates before human intervention ranging from about 85% to 95%. These tools are not officially supported by ROR, but we list them here in case you find them useful.

* Selected Python scripts that match organization names to ROR IDs area available in the [ror-utilities Github repository](https://github.com/ror-community/ror-utilities).

* [OpenAlex Institution Parsing](https://github.com/ourresearch/openalex-institution-parsing) by OurResearch

* [S2AFF - Semantic Scholar Affiliations Linker](https://github.com/allenai/S2AFF/) by the Allen AI Institute

* [RORRetriever](https://github.com/Metadata-Game-Changers/RORRetriever) by Metadata Game Changers

* [EMBL-EBI ROR Predictor prototype](https://gitlab.ebi.ac.uk/literature-services/public-projects/ROR-proto-EMBL) by EMBL-EBI for [Project FREYA](https://www.project-freya.eu/Plone/en)

* [dataESR affiliation matcher](https://github.com/dataesr/affiliation-matcher) developed by Anne L'Hôte and Eric Jeangirard for the French Ministry of Higher Education

* [OpenAlex ROR Predictor (gpu-based)](https://github.com/adambuttrick/openalex-ror-predictor) by ROR Curation Lead Adam Buttrick

* [fastText ROR Predictor (cpu-based)](https://github.com/adambuttrick/ror-predictor-fasttext) by ROR Curation Lead Adam Buttrick

* [lr_predictor](https://github.com/adambuttrick/lr_matches) by ROR Curation Lead Adam Buttrick

* [ROR experimental affiliation matching](https://github.com/ror-community/affiliation-matching-experimental) - A collection of data and code for training models and experimenting with automatically matching affiliation strings to ROR IDs. Not production code, and not officially supported by ROR.

## Testing and training data

ROR has collected sets of data from Springer Nature, the American Physical Society, OpenAlex, and Crossref for testing and training affiliation matching strategies, and these datasets are openly available at [https://github.com/ror-community/affiliation-matching-experimental/tree/main/test_data](https://github.com/ror-community/affiliation-matching-experimental/tree/main/test_data). These datasets include affiliation text strings from production systems that have been matched to ROR IDs with varying levels of human review.

Crossref has also published a dataset of DOI metadata with over 140 million affiliation assertions from Crossref metadata records through March 2025 that has been used to test the ROR API [single search affiliation matching strategy](https://ror.readme.io/docs/api-affiliation#/single-search-strategy). It includes automatically-detected matches for over 94 million affiliation assertions.

<br />
