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
  description: ''
  pages:
    - type: link
      title: ROR Data on Zenodo
      url: https://doi.org/10.5281/zenodo.6347574
---

> 🚧 Version 1 of the ROR schema and API will be sunset in December 2025
>
> In December 2025, version 1 of the ROR schema and API will be sunset, meaning that ROR API requests with v1 in the path will no longer return a response, v1 files will no longer be included in the ROR data dump, and v1 documentation will no longer be available. Read more in our [changelog](https://ror.readme.io/changelog/2025-07-01-sunset-of-version-1#/).

The entire ROR registry dataset is freely available on Zenodo at [https://zenodo.org/doi/10.5281/zenodo.6347574](https://zenodo.org/doi/10.5281/zenodo.6347574) in both JSON and CSV formats for both v1 and v2 of the ROR metadata schema. All ROR IDs and metadata in the data dump are provided under the [Creative Commons CC0 1.0 Universal Public Domain Dedication](https://creativecommons.org/publicdomain/zero/1.0//).

# Data format

Beginning with release v1.45 on 11 April 2024, data releases contain JSON and CSV files formatted according to both schema v1 and schema v2. Version 2 files have `_schema_v2` appended to the end of the filename, e.g., `v1.45-2024-04-11-ror-data_schema_v2.json`. In order to maintain compatibility with previous release, version 1 files have no version information in the filename, e.g., `v1.45-2024-04-11-ror-data.json`.

For both versions, the CSV file contains a subset of fields from the JSON file, some of which have been flattened for easier parsing. As ROR records and the ROR schema are maintained in JSON, CSVs are for convenience only. JSON is the format of record.

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

Beginning with v1.45 in April 2024, ROR has introduced schema versioning, with files available in schema v1 and schema v2. The ROR API default version, however, remains v1 and will be changed to v2 in April 2025. To align with the API, the data dump major version will remain 1 until the API default version is changed to v2. At that time, the data dump major version will be incremented to 2 per below.

## Past

From Apr 2019 to Sep 2021, changes to ROR data happened in collaboration with GRID. After each GRID release, ROR assigned new IDs to each new organization in GRID and published a new ROR release that corresponded to the most recent GRID release. Releases were named only with a date, e.g., 2021-09-23-ror-data.zip.

[GRID announced the sunset of its public data offering in July 2022](https://ror.readme.io/docs/gridror-transition-faq) and published its final data release in Sept 2021. ROR published a corresponding data release ([2021-09-23-ror-data.zip](https://doi.org/10.5281/zenodo.5534443)) in Sept 2021. Both releases contained the same set of organizations, with GRID and ROR IDs assigned to all.

# Record status

As of December 1, 2022, the data dump contains all records with all statuses (active, inactive, withdrawn), while the API defaults to returning records with a status of active. Prior to 1 Dec 2022, all records had a status of active. Beginning with data dump [https://doi.org/10.5281/zenodo.7387951](https://doi.org/10.5281/zenodo.7387951), inactive and withdrawn records are also included. See the [2022-12-01 changelog post](https://ror.readme.io/changelog/2022-12-01-organization-status-changes) for details about this change.

Records with a status of "inactive" or "withdrawn" may have one or more successor organizations listed in their relationships. Successor relationships indicate that another organization continues the work of an organization that has become inactive or has been withdrawn. Depending on your use case, you may wish to update references to inactive ROR records in your system(s) to the corresponding Successor organization(s).

See [ROR data structure](doc:ror-data-structure) for more information about status and relationships.

# Download ROR data dumps programmatically with the Zenodo API

> ❗️ Changes to Zenodo API requests
>
> Zenodo completed a major software upgrade on 13 Oct 2023. As a result, API requests to fetch ROR data dumps have changed. These changes have not been updated in Zenodo API documentation but are noted below. If you use the Zenodo API to retrieve ROR data dumps, please update your code. These instructions may be subject to change as Zenodo continues making changes to its services.

1. Get a list of the records in the ror-data community

   **New request (after 13 Oct 2023 Zenodo update)**  
   Request path and query structure have changed. The community name `ror-data` is now part of the path portion and the sort parameter value is now `newest`.

   ```curl
   curl "https://zenodo.org/api/communities/ror-data/records?q=&sort=newest"
   ```

   **Previous request (before 13 Oct 2023 Zenodo update)**

   ```curl
   curl "https://zenodo.org/api/records/?communities=ror-data&sort=mostrecent"
   ```
2. In the response, the most recent record will be `hits.hits[0]`. Find the latest file attached to this record by checking the last item in `hits.hits[0].files`.  The file download URL is in `[files.links.download](files.links.self)`.

   **New response (after 13 Oct 2023 Zenodo update)**  
   Record structure has changed. Only the URL for the most recent file download is located in`files.links.self`. Previously, download URLs for all versions were included in  `files`, with the first files item being the most recent version. To obtain previous versions, get a list of previous version records using the URL in `hits.hits[0].links.versions`.

```Text json
   ...
   {
     "hits": {
       "hits": [
         {
           "created": "2023-10-12T23:45:15.640072+00:00",
           "modified": "2023-10-13T02:26:58.828749+00:00",
           "id": 8436953,
           "conceptrecid": "6347574",
           "doi": "10.5281/zenodo.8436953",
           "conceptdoi": "10.5281/zenodo.6347574",
           "doi_url": "https://doi.org/10.5281/zenodo.8436953",
           ...
           "files": [
             {
               "id": "3b963cb2-9160-44cd-adce-26c03a0ea735",
               "filename": "v1.34-2023-10-12-ror-data.zip",
               "filesize": 31778797,
               "checksum": "6798489c7630abe56de7a00fe4440779",
               "links": {
                 "download": "https://zenodo.org/api/records/8436953/files/v1.34-2023-10-12-ror-data.zip/content"
               }
             }
           ]
   ...
```

**Previous response (before 13 Oct 2023 Zenodo update)**

```Text json
...
   "hits" : {
      "hits" : [
         {
            "conceptdoi" : "10.5281/zenodo.6347574",
            "conceptrecid" : "6347574",
            "created" : "2022-06-17T15:07:41.543813+00:00",
            "doi" : "10.5281/zenodo.6657125",
            "files" : [
               {
                  "bucket" : "25d4f93f-6854-4dd4-9954-173197e7fad7",
                  "checksum" : "md5:88307ac74fad1c388f4987c00d09f769",
                  "key" : "v1.0-2022-03-17-ror-data.json.zip",
                  "links" : {
                     "self" : "https://zenodo.org/api/files/25d4f93f-6854-4dd4-9954-173197e7fad7/v1.0-2022-03-17-ror-data.json.zip"
                  },
                  "size" : 20378730,
                  "type" : "zip"
               },
               {
                  "bucket" : "25d4f93f-6854-4dd4-9954-173197e7fad7",
                  "checksum" : "md5:045f3edcb72e323a742e88bf81361a26",
                  "key" : "v1.1-2022-06-16-ror-data.zip",
                  "links" : {
                     "self" : "https://zenodo.org/api/files/25d4f93f-6854-4dd4-9954-173197e7fad7/v1.1-2022-06-16-ror-data.zip"
                  },
                  "size" : 20454781,
                  "type" : "zip"
               }
            ]
...
```

3. Download the file to a local directory using the link

**New request (after 13 Oct 2023 Zenodo update)**  
Request path has changed; make sure to specify file output option in cURL request.

```curl curl
curl -o "v1.34-2023-10-12-ror-data.zip" https://zenodo.org/api/records/8436953/files/v1.34-2023-10-12-ror-data.zip/content
```

**Previous request (before 13 Oct 2023 Zenodo update)**

```Text curl
curl "https://zenodo.org/api/files/25d4f93f-6854-4dd4-9954-173197e7fad7/v1.1-2022-06-16-ror-data.zip"
```

For additional information about retrieving files from Zenodo using the API, see [https://developers.zenodo.org/#records](https://developers.zenodo.org/#records).
