---
title: ROR basics
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR basics
  description: >-
    ROR is a global registry of open persistent identifiers for research
    organizations, making it easy to connect organizations to researchers and
    research outputs. It includes identifiers for over 100,000 organizations and
    provides tools like a search interface and API for access.
  robots: index
next:
  description: ''
---
# What is ROR?

[The Research Organization Registry (ROR)](https://ror.org) (pronounced "roar") is a global, community-led, curated registry of open persistent identifiers for research organizations. ROR makes it easy for anyone or any system to disambiguate research organization affiliations and connect research organizations to researchers and research outputs.

The ROR registry includes identifiers and metadata for more than 120,000 organizations: companies, universities, labs, research centers, nonprofits, and government organizations -- any organization involved in scholarly research. ROR also includes “child” and "related" organizations such as a university's research institutes, hospitals, and laboratories or a multinational company's branches in different countries.

To see which organizations are already included in the registry, you can:

* Use the public search interface: [https://ror.org/search](https://ror.org/search)
* Use the ROR API:
  * [https://api.ror.org/v2/organizations](https://api.ror.org/v2/organizations)
* Download the entire ROR data file: [https://zenodo.org/communities/ror-data](https://zenodo.org/communities/ror-data)

ROR is the first and only organization identifier that is openly available (<Anchor label="CC0" target="_blank" href="https://creativecommons.org/public-domain/cc0/">CC0</Anchor> data available via an open REST API and public data dump), specifically focused on identifying affiliations in scholarly metadata, developed as a community initiative to meet community use cases, and designed to be integrated into open scholarly infrastructure. It is the preferred organization identifier of Crossref, DataCite, and ORCID.

For more information, see ROR's [About page](https://ror.org/about) and [FAQs](https://ror.org/about/faqs).

# Tools and services

ROR provides a set of open tools for interacting with ROR data and integrating ROR IDs. These include:

* [REST API](doc:rest-api)
* [Data dump](doc:data-dump)
* [Web search](doc:web-search)
* [OpenRefine reconciler](doc:openrefine-reconciler)

# Uses

ROR can be used in any system where research organization information is collected or distributed. ROR is therefore useful for any stakeholders who need to track research by institution, including publishers, repositories, funders, and research administrators. [See a list of current integrations](https://ror.org/integrations).

# Governance

ROR is operated as a collaborative initiative by [California Digital Library](https://cdlib.org), [Crossref](https://www.crossref.org/), and [DataCite](https://datacite.org), in consultation with a broad network of [community advisors](https://ror.org/community/#community-advisory-group). Read more about ROR's [governance model](https://ror.org/about/#governance-model).

# Roadmap

The [ROR roadmap is available on GitHub](https://github.com/ror-community/ror-roadmap).

# History

The first "Minimum Viable Registry" iteration of ROR was launched in January 2019 using seed data from Digital Science's [Global Research Identifier (GRID)](https://www.grid.ac/) project. The MVR and first registry release included ROR IDs and metadata for 91,625 organizations and also included mechanisms for accessing and querying ROR data via a search interface, REST API, and data dump.

From 2019 to early 2022, GRID data and ROR data were synchronized. In July of 2021, GRID announced its intention to cease its public releases and [pass the torch to ROR](https://www.digital-science.com/grid-passes-the-torch-to-ror-faqs/), and in March of 2022, ROR [published its first release independently of GRID](https://ror.org/blog/2022-03-17-first-independent-release/).

See [ROR history](https://ror.org/about/#history) for more about ROR's origins and history.

# Glossary

* **ROR, ROR registry:** The Research Organization Registry, available in the UI at <Anchor label="https://ror.org/search" target="_blank" href="https://ror.org/search">https://ror.org/search</Anchor>  or the API at [https://api.ror.org/v2/organizations](https://api.ror.org/v2/organizations).
* **ROR identifier/ROR ID:** The identifier for a particular organization, ex: [https://ror.org/03yrm5c26](https://ror.org/03yrm5c26)
* **ROR record:** The metadata associated with a ROR identifier, ex:  [[https://ror.org/03yrm5c26](https://ror.org/03yrm5c26)]([https://ror.org/03yrm5c26](https://ror.org/03yrm5c26)) or [\<[https://api.ror.org/v2/organizations/https://ror.org/03yrm5c26>](https://api.ror.org/v2/organizations/https://ror.org/03yrm5c26)]([https://api.ror.org/v2/organizations/https://ror.org/03yrm5c26](https://api.ror.org/v2/organizations/https://ror.org/03yrm5c26))
