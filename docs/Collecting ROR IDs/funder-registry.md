---
title: Transition from Open Funder Registry to ROR
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    Crossref and ROR are transitioning from the Open Funder Registry to the
    Research Organization Registry (ROR) as the standard identifier for funders,
    with answers to frequently asked questions provided for a smooth transition.
  robots: index
next:
  description: ''
---
In September 2023, Crossref and ROR [announced a long-term plan to deprecate the Open Funder Registry](https://www.crossref.org/blog/open-funder-registry-to-transition-into-research-organization-registry-ror/) (formerly known as FundRef) and merge it into ROR, the Research Organization Registry. We're thrilled that ROR is set to become the standard identifier for funders, and we're determined to help make this transition as easy as possible for all concerned. Here are some answers to frequently asked questions about this transition, including guidance on how to switch from using Funder IDs to using ROR IDs.

# Why is this transition happening?

Merging the two registries will make workflows more efficient and less confusing for all concerned and will enable the development of robust new services that use organization identifiers. The [Open Funder Registry](https://www.crossref.org/services/funder-registry/) currently includes about 44,000 IDs and records for funders, and a large majority of the most-used of these Funder IDs already exist in ROR, with others being continually added through ongoing curation work. Funders, publishers, and others should not need to decide whether to use Funder IDs or ROR IDs and should not need to use both: a single registry, ROR, can fulfill the purpose of identifying research organizations -- including funders.

# When is this transition happening?

This transition is happening now! Users of the Open Funder Registry can begin the work of switching to ROR at any time. ROR's [data structure](doc:ror-data-structure) has always included an external identifier field to enable mapping of ROR IDs to Funder IDs, and ROR has been [working since 2022 to add and update ROR records from Open Funder Registry data](https://ror.org/blog/2023-10-12-ror-funder-registry-overlap/). The Open Funder Registry will continue to be updated and its existing technical infrastructure will remain available for the foreseeable future, but future systems and processes at Crossref will be designed to use ROR IDs to identify funders.

# Can I use ROR IDs instead of Funder IDs in Crossref metadata?

Yes, you can! If you are a Crossref member, you can [now use ROR IDs to identify funders](https://www.crossref.org/blog/come-ror-with-us-using-ror-ids-in-place-of-funder-ids/) in any place in Crossref metadata where you currently use Funder IDs. You can consult [Crossref documentation on funding metadata](https://www.crossref.org/documentation/funder-registry/funding-data-overview/) to learn more.

# Will every Funder ID have a corresponding ROR ID?

Our aim is to provide comprehensive coverage of Funder IDs that are widely used, are not duplicates or errors, and are in [scope](https://ror.org/registry/#scope-and-criteria-for-inclusion) for ROR. Based on our analysis of funding data in Crossref, DataCite, and ORCID metadata, about one-third of the Funder Registry accounts for the majority of actual usage, so we’re creating and updating ROR IDs for funders on this basis. Currently, there are ROR IDs for over 94% of cases where a Funder ID is used in DataCite and Crossref DOI records. We will continue to improve this coverage to better support the funder identification use case. See [How ROR and the Funder Registry Overlap](https://ror.org/blog/2023-10-12-ror-funder-registry-overlap/) to learn more and check the [ROR / Funder Registry Overlap tool](https://rorfunderregistryoverlap.streamlit.app/) for current data on Funder ID to ROR ID mapping.

# Will Funder IDs continue to resolve?

Absolutely! Funder IDs are DOIs, and as such are persistent. The Funder ID for the Alfred P. Sloan Foundation, for instance, is [\<https://doi.org/10.13039/100000879>](https://doi.org/10.13039/100000879) (or `100000879` in some usages), and Crossref will ensure that all such Funder IDs continue to resolve to a page with information about that funder.

# Will the Crossref Funder API remain available?

Yes, Crossref will maintain the current funder API endpoints at `https://api.crossref.org/funders` at least until ROR IDs become the predominant funding identifier for newly registered content, and perhaps beyond.

# Will the Crossref Funding Data search remain available?

Yes, Crossref will maintain the current [funding data search](https://search.crossref.org/search/funders), which connects funders to published works, at least until ROR IDs become the predominant funding identifier for newly registered content, and perhaps beyond.

# I use Funder IDs in my system. How can I switch to ROR?

We're here to help! Take a look at the guidance below and write [support@ror.org](mailto:support@ror.org) with any questions.

> 👍 Three quick resources for switching from Funder IDs to ROR IDs
>
> * [ROR / Funder Registry Overlap tool](https://rorfunderregistryoverlap.streamlit.app/) - web application for searching, visualizing, and downloading ROR ID & Funder ID data
> * [Map other organization IDs to ROR IDs](doc:mapping) - guide to help you map Funder IDs to ROR IDs using the ROR API, data dump, or sample scripts
> * [Create ROR-powered forms](doc:forms) - guide to creating a funder lookup field in your system using ROR

## Find both IDs for a single funder

Here are two ways to find both the Funder ID and the ROR ID for a single funder.

1. Go to [https://ror.org/search](https://ror.org/search) and search for a funder name to determine whether that funder has a ROR record. If the ROR record exists and is mapped to a Funder ID, the ROR record will include both the ROR ID and the Crossref Funder ID.

<Image align="center" alt="ROR record for the Alfred P Sloan Foundation" border={true} caption="ROR web record for the Alfred P. Sloan Foundation" src="https://files.readme.io/7221b92-Screenshot_2024-04-04_at_3.24.41_PM.png" width="500px" />

2. Use the [Funder Mapping Lookup](https://rorfunderregistryoverlap.streamlit.app/) to search for a funder name and find its corresponding Funder ID and ROR ID:

<Image align="center" border={true} width="500px" src="https://files.readme.io/8bf7879-Screenshot_2024-03-13_at_3.10.55_PM.png" className="border" />

## Get a CSV file of all Funder ID to ROR ID mappings

> 🚧 ROR IDs and Funder IDs are not always mapped on a one-to-one basis
>
> A single ROR ID can often be mapped to multiple Funder IDs. When a ROR ID has multiple corresponding Funder IDs, the preferred Funder ID is stored in the field `external_ids.preferred` in v2 of the ROR metadata schema. Be sure to check the [status](https://ror.readme.io/docs/ror-data-structure#/status) of the mapped ROR record to ensure that it has a status of "active."

Here are two ways to get a CSV file of all Funder ID to ROR ID mappings.

1. Get the latest copy of the ROR [Data dump](doc:data-dump) from Zenodo at [https://zenodo.org/doi/10.5281/zenodo.6347574](https://zenodo.org/doi/10.5281/zenodo.6347574) and download and unzip the .zip file. The provided CSV will show mappings between ROR IDs and, where available, one or more Funder IDs in the `external_ids.all` field. Remember that one ROR ID can have multiple corresponding Funder IDs. When this is the case, the preferred match is stored in the field `external_ids.preferred`.
2. In the [ROR / Funder Registry Overlap tool](https://rorfunderregistryoverlap.streamlit.app/), click the "Download mapped funders as CSV" button on either "Aggregate overlap" page to download a CSV file that maps Funder IDs to ROR IDs.

<Image align="center" border={true} width="300px" src="https://files.readme.io/6676c0b-Screenshot_2024-03-14_at_11.58.11_AM.png" className="border" />

The resulting CSV file will have two columns: Funder ID and ROR ID. Additional metadata is not included. Remember that one ROR ID can have multiple corresponding Funder IDs.

Note that the [ROR / Funder Registry Overlap tool](https://rorfunderregistryoverlap.streamlit.app/) is manually updated periodically with both new ROR records and new Open Funder Registry records. Be sure to check the last updated date on the left-hand side of the screen to see which versions of the registries have been loaded.

## Get a CSV file of Funder ID to ROR ID mappings by Crossref member

When Crossref members deposit DOI metadata for a journal article or other research output, they can include Funder IDs to identify the sources of funding for that research. For instance, the Royal Society of Chemistry (RSC) is the publisher of an article that acknowledges three funding sources, and each funder is identified by name and by Funder ID. This single article therefore has three "funding assertions":

<Image align="center" border={true} width="500px" src="https://files.readme.io/b145196-Screenshot_2024-03-14_at_12.32.26_PM.png" className="border" />

A single funder such as the Japan Society for the Promotion of Science may be referenced in many funding assertions, but that funder has a single Funder ID (`501100001691` or `10.13039/501100001691` or [https://doi.org/10.13039/501100001691](https://doi.org/10.13039/501100001691) ) and a single corresponding ROR ID ( [https://ror.org/00hhkn466](https://ror.org/00hhkn466) ).

You can use the [ROR / Funder Registry Overlap tool](https://rorfunderregistryoverlap.streamlit.app/) to look up a Crossref member, see how many funding assertions they have made in Crossref DOI records, and download CSV files that map the Funder IDs the member has used to ROR IDs.

The resulting CSV file will have two columns: Funder ID and ROR ID. Additional metadata is not included. Remember that one ROR ID can have multiple corresponding Funder IDs.

<Image align="center" border={true} width="500px" src="https://files.readme.io/df86e8c-Screenshot_2024-03-13_at_3.20.17_PM.png" className="border" />

## Map a custom list of Funder IDs to ROR IDs

Because Funder IDs are included in ROR records in the [external_ids](https://ror.readme.io/docs/ror-data-structure#external_ids)  field, you can map a custom list of Funder IDs to ROR IDs using the ROR API or ROR data dump. This method will also allow you to retrieve additional ROR metadata. For more information and examples, see our guide [Map other organization IDs to ROR IDs](doc:mapping).

## Build ROR-powered forms

You can build ROR-powered funder lookups into your system so that researchers, editors, funder staff, and other users can easily select a funder by name that is connected to a ROR ID and its associated metadata. See [Create ROR-powered forms](doc:forms) for guidance.

# How can I give feedback on or get support for this transition?

* For ROR technical or metadata questions, write [support@ror.org](mailto:support@ror.org).
* For Open Funder Registry technical or metadata questions, [contact Crossref](https://support.crossref.org/hc/en-us/requests/new).

# Where can I learn more?

* **[Crossref Services - Open Funder Registry](https://www.crossref.org/services/funder-registry/)** - general information about the Open Funder Registry
* **[Crossref Open Funder Registry - GitLab](https://gitlab.com/crossref/open_funder_registry)** - complete downloadable Open Funder Registry data
* **[Crossref Funder Advisory Group](https://www.crossref.org/working-groups/funders/)** - advisory group for Crossref funder members
* **[ROR / Funder Registry Overlap tool](https://rorfunderregistryoverlap.streamlit.app/)** - tool to help analyze and export ROR ID and Funder ID information
* **[Open Funder Registry to transition into ROR](https://www.crossref.org/blog/open-funder-registry-to-transition-into-research-organization-registry-ror/)**, blog post, September 7, 2023
* **[How ROR and the Open Funder Registry Overlap: A Closer Look at the Data](https://ror.org/blog/2023-10-12-ror-funder-registry-overlap/)**, blog post, October 12, 2023
* **[ROR-ing ahead: using ROR in place of the Open Funder Registry](https://www.crossref.org/blog/roring-ahead-using-ror-in-place-of-the-open-funder-registry/)**, blog post, January 30, 2024
* **[Why We All Need Good Funding Metadata](https://ror.org/events/2024-01-31-why-we-all-need-good-funding-metadata/)**, panel slides and recording, January 31, 2024
* **[Come ROR with us: Using ROR IDs in place of Funder IDs](https://www.crossref.org/blog/come-ror-with-us-using-ror-ids-in-place-of-funder-ids/)**, blog post, March 4, 2025
