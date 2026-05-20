---
title: Updates and curation
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR updates and curation
  description: >-
    This document explains how to request additions or changes to the ROR
    registry, which uses a community-based curation model to review and approve
    requests before they are added to new versions of the registry released at
    least once a month.
  robots: index
next:
  pages:
    - title: Journey of a ROR Curation Request
      type: link
      url: https://doi.org/10.71938/T128-EA02
---
# Requesting additions and changes

Anyone can ask for a new organization to be added to ROR or for an existing ROR record to be changed via a public [curation request form](http://curation-request.ror.org), which is linked to from each ROR record in the browser-based [ROR search](https://ror.org/search). You do not need to be affiliated with an organization to suggest changes to the organization’s record in ROR, since each request is evaluated on its own merits by ROR curators.

If you have more than one request, please submit a separate [curation request form](http://curation-request.ror.org) for each organization/record or [create the corresponding issues in our ror-updates GitHub repository](https://github.com/ror-community/ror-updates/issues/new/choose). The latter method requires a GitHub account, but it is generally faster to create a GitHub issue than to complete the form.

If you want to submit a large number of requests, you can also download a bulk request spreadsheet template in XLSX format, complete it, and email it to [support@ror.org.](mailto:support@ror.org.) See [Bulk Requests](https://ror.org/registry/#bulk-requests)  on the ROR website for templates and instructions. Please be mindful that large bulk submissions may take us some time to process.

[Read more about how the registry is curated](https://ror.org/registry).

<Callout icon="🚧" theme="warn">
  ## Does the organization already have a ROR ID?

  Before submitting a request to add a new organization to ROR, be sure to search [https://ror.org/search](https://ror.org/search) to see if it already exists.
</Callout>

# Community-based curation model

ROR uses a community-based curation model to maintain the registry. After a registry request is submitted, the proposed change is reviewed by ROR’s metadata curation lead and, if necessary, by the volunteer ROR Curation Advisory Board to ensure it is in scope and in line with ROR’s metadata policies.

Approved changes are assigned to a future release and the records go through a metadata preparation process and schema validation check before they are deployed on the ROR production site and made available in the ROR API and data dump.

ROR curation activities are openly available in [the ror-updates GitHub repository](https://github.com/ror-community/ror-updates). Anyone can follow along with the curation process by watching individual issues in the GitHub repository and following their progress on the [ROR Updates tracker](https://github.com/ror-community/ror-updates/projects/1).

<Callout icon="📘" theme="info">
  ## Not all requests are approved

  We accept any and all requests for changes and additions to ROR records, but we do not approve all requests. We review all requests carefully and approve only those that are in line with ROR's [scope](http://ror.org/registry/#scope-and-criteria-for-inclusion) and [policies](https://github.com/ror-community/ror-updates/wiki/ROR-Metadata-Policies#policies-for-specific-metadata-elements).
</Callout>

# Update frequency and process

Changes to the ROR registry are made in batches and added to new versions of the entire registry, which are then released at least once a month, usually twice a month. An approved request therefore can take anywhere from 2 to 6 weeks to appear in the registry or even longer, depending on volume of requests and the timing of the request in relation to our release schedule. When a new version of the ROR registry is released, the data is immediately available in the [ROR search interface](https://ror.org/search), in the [ROR API](doc:rest-api), and in the [public ROR data dump on Zenodo](https://zenodo.org/communities/ror-data). [Learn how to download the data dump programmatically](https://ror.readme.io/docs/data-dump#download-ror-data-dumps-programmatically-with-the-zenodo-api).

To see which records were added or modified in in a given release, see the ROR [release notes](https://github.com/ror-community/ror-updates/releases) on GitHub. Release notes for each release give a summary of the changes, the number of records added, the number of records modified, and a full list of added and modified records. Join the [ROR Tech Forum](https://groups.google.com/a/ror.org/g/ror-tech) to receive announcements of each new release.

# Changes made in registry releases

Each new release of the ROR registry is different, but typically a new release includes modifications and additions [requested](https://github.com/ror-community/ror-updates/issues) by the public. The ROR Curation Lead and community curation board also initiate quality assurance and metadata reconciliation [projects](https://github.com/ror-community/ror-updates/projects/1) that result in new and updated records.

<Callout icon="📘" theme="info">
  ## Changes to geographical information in updated records

  Some geographical information in ROR comes from the [GeoNames geographical database](https://www.geonames.org/). When a ROR record is updated for any reason, geographical information from GeoNames is also updated, which can result in changes in other fields. Common examples include alteration of the values in fields `lat` (latitude) and `lng` (longitude).
</Callout>
