---
title: Data dump
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR data dump
  description: >-
    The ROR registry dataset is available on Zenodo in JSON and CSV formats,
    with new versions released monthly. The data dump includes all ROR IDs and
    metadata under the Creative Commons CC0 1.0 Universal Public Domain
    Dedication.
  robots: index
next:
  pages:
    - slug: zenodo
      title: Retrieve ROR data from Zenodo
      type: basic
---
The entire ROR registry dataset is freely available on Zenodo at [https://doi.org/10.5281/zenodo.6347574](https://doi.org/10.5281/zenodo.6347574). All ROR IDs and metadata in the data dump are provided under the [Creative Commons CC0 1.0 Universal Public Domain Dedication](https://creativecommons.org/publicdomain/zero/1.0//). Both current and previous versions / releases of the ROR registry are available.

See [Retrieve ROR data from Zenodo](doc:zenodo) for instructions on how to download ROR datasets from Zenodo programmatically. 

# Data formats

* Data releases beginning with release 1.0 on 2022-03-17 up to and including release 1.20 on 2023-02-28 contain JSON files formatted according to ROR [Schema 1.0](doc:schema-v1).

* Data releases beginning with release 1.21 on 2023-03-16 up to and including release 1.44 on 2024-03-28 contain both JSON and CSV files formatted according to ROR [Schema 1.0](doc:schema-v1).

* Data releases beginning with release 1.45 on 2024-04-11 up to and including release 1.74 on 2025-11-24 contain JSON and CSV files formatted according to both [Schema 1.0](doc:schema-v1) and [Schema 2.0](doc:schema-v2). Version 2 files have `_schema_v2` appended to the end of the filename, e.g., `v1.45-2024-04-11-ror-data_schema_v2.json`. In order to maintain compatibility with previous releases, version 1 files have no version information in the filename, e.g., `v1.45-2024-04-11-ror-data.json`.

* Data releases beginning with release 1.58 on 2024-12-11 include additional information added in [Schema 2.1](doc:schema-v2-1).

* Data releases after release 1.74 on 2025-11-24 no longer contain JSON and CSV files formatted according to ROR [Schema 1.0](doc:schema-v1).

* Data releases beginning with release 2.0 on 2025-12-16 have no version information in the filename, e.g., `v2.0-2025-12-16-ror-data.json`.

For both schema versions, the CSV file contains a subset of fields from the JSON file, some of which have been flattened for easier parsing. Because ROR records and the ROR schema are maintained in JSON, CSVs are for convenience only. JSON is the format of record.

# Releases

When new [releases](https://github.com/ror-community/ror-updates/releases/) of the ROR registry are issued, typically at least once a month, the ROR REST API is updated automatically and a new data dump version is uploaded to Zenodo. You can use the Zenodo API to download these data dumps as per the instructions below.

ROR datasets prior to Sep 2021 are available from [Figshare](https://figshare.com/collections/ROR_Data/4596503). See also the Zenodo [ROR Data Community](https://zenodo.org/communities/ror-data/records).

For more information on how the ROR registry is updated, see [Updates to ROR records](doc:updates).

# Release versioning

## Current

Data releases are versioned as follows:

* Minor versions (ex 1.1, 1.2, 1.3):  Contain changes to data, such as new records and updates to existing records. No changes to the data model/structure.
* Patch versions (ex 1.0.1): Used infrequently to correct errors in a release. No changes to the data model/structure.
* Major versions (ex 1.x, 2.x, 3.x):  Contains changes to data model/structure, as well as the data itself. Major versions will be released with significant advance notice.

For convenience, the date is also included in the release file name, ex: v1.0-2022-03-15-ror-data.zip.

## Past

From Apr 2019 to Sep 2021, changes to ROR data happened in collaboration with GRID. After each GRID release, ROR assigned new IDs to each new organization in GRID and published a new ROR release that corresponded to the most recent GRID release. Releases were named only with a date, e.g., 2021-09-23-ror-data.zip.

[GRID announced the sunset of its public data offering in July 2022](https://ror.readme.io/docs/gridror-transition-faq) and published its final data release in Sept 2021. ROR published a corresponding data release ([2021-09-23-ror-data.zip](https://doi.org/10.5281/zenodo.5534443)) in Sept 2021. Both releases contained the same set of organizations, with GRID and ROR IDs assigned to all.

# Record status

As of December 1, 2022, the data dump contains all records with all statuses (active, inactive, withdrawn), while the API defaults to returning records with a status of active. Prior to 1 Dec 2022, all records had a status of active. Beginning with data dump [https://doi.org/10.5281/zenodo.7387951](https://doi.org/10.5281/zenodo.7387951), inactive and withdrawn records are also included. See the [2022-12-01 changelog post](https://ror.readme.io/changelog/2022-12-01-organization-status-changes) for details about this change.

Records with a status of "inactive" or "withdrawn" may have one or more successor organizations listed in their relationships. Successor relationships indicate that another organization continues the work of an organization that has become inactive or has been withdrawn. Depending on your use case, you may wish to update references to inactive ROR records in your system(s) to the corresponding Successor organization(s).

See [ROR data structure](doc:ror-data-structure) for more information about status and relationships.

<br />
