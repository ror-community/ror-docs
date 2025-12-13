---
title: Schema versions
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR schema versions
  description: >-
    The document outlines plans for versioning the ROR metadata schema and API,
    with new major versions released no more than once a year and previous
    versions supported until their planned sunset date. Feedback on new schema
    versions will be sought through various communication channels.
  robots: index
---
This page documents policies around ROR schema versioning. To request a change to the ROR metadata schema, [submit a schema change request](https://github.com/ror-community/ror-roadmap/issues/new/choose) to the ROR Roadmap on GitHub.

ROR [Schema 2.0](doc:schema-v2) was deployed to production in April 2024, with a minor, non-breaking update [Schema 2.1](doc:schema-2-1) deployed in December 2024. Schema 1.0 of ROR [was sunset in December 2025](https://ror.readme.io/changelog/2025-12-04-version-1-of-the-ror-api-and-schema-is-sunsetting).

> 📘 JSON Schema
>
> JSON schema documents used to generate and validate ROR records are available at [https://github.com/ror-community/ror-schema](https://github.com/ror-community/ror-schema).

# Community feedback

In the autumn of 2022, ROR asked for community feedback on plans for versioning the ROR metadata schema and API. Read the draft and final proposals below.

* [Handling schema & API versioning in ROR (Draft)](https://docs.google.com/document/d/1882i-nUt8rqhd1bLqMJ3hF-wq5i8HYktUYmN_gUR1xM/edit?usp=sharing) - proposal open for comment through November 2022
* [Handling schema & API versioning in ROR (Final Draft)](https://docs.google.com/document/d/18nl6pq0kdCU5ApcdbNjKnV7xHIw9eEY7DJG1WHjaLSs/edit?usp=sharing) - proposal adopted November 2022

# Schema versioning

The ROR metadata schema and API will be versioned in lockstep for major versions, meaning that when a new major schema version is introduced, the API version will also be incremented so that users can unambiguously request a response in a specific schema version.

> 📘 API versioning
>
> The ROR schema and API are versioned together, so a new major version of the schema will be accompanied by a new major version of the API. Read more about [API versioning](doc:api-versions).

The ROR metadata schema will use semantic versioning:

* A minor version (ex, X.1, X.2, etc) will be incremented when non-breaking changes are made, such as adding an element. Since minor schema versions are non-breaking, they will be rolled into the appropriate major version of the API.
* A major version (ex, 1.X, 2.X, etc) will be incremented when breaking changes are made, such as removing or restructuring an element, changing the structure or data type of a schema element (e.g., changing a single value to an array), or removing items from controlled lists of allowed values.

# Changes that require versioning

## Minor version change

* Adding schema elements

## Major version change

* Removing or renaming schema elements
* Changing the structure or data type of a schema element (ex, changing a single value to an array)
* Removing items from controlled lists of allowed values

# Changes that do not require versioning

* Adding items to controlled lists of allowed values

# Deploying new schema versions

No more than 1 new major schema version will be released each year, and ROR will not seek to release a major version each year, unless there's need for it. It's very likely that there will be several years between major version releases.

New versions will be made available in the ROR staging environment for approximately 1 month prior to production release. Users will have an opportunity to provide feedback on the technical implementation of the new schema version via ROR communication channels, such as the [ROR Technical Forum](https://groups.google.com/a/ror.org/g/ror-tech).

# Supporting and sunsetting previous schema versions

When a new version is deployed to production, any supported previous versions will continue to be supported until their planned sunset date. This may sometimes result in supporting 3 versions concurrently.

Plans to sunset a previous version will be announced at least 1 year prior to the planned sunset date, via the [ROR community meetings](https://ror.org/events) and other communication channels such as the [ROR blog](https://ror.org/blog), [Mastodon](https://mastodon.social/@ResearchOrgs), [Bluesky](https://bsky.app/profile/researchorgs.bsky.social), [LinkedIn](https://www.linkedin.com/company/ror-research-organization-registry), the [ROR Technical Forum](https://groups.google.com/a/ror.org/g/ror-tech), and the [ROR Community Forum](https://groups.google.com/a/ror.org/g/ror-community). Regular reminders will continue prior to the sunset date.

# Schema version implementation

Schema files are stored in [ror-schema](https://github.com/ror-community/ror-schema). When a new schema version is created, a corresponding new file will be created and named ror-schema-X.X.json.  

Previous schema files will not be deleted.

Data dumps will be created for all currently supported schema versions (typically 2 versions, but possibly 3 versions if a new schema version has recently been released

Data dumps will continue to be added to a single Zenodo record as versions, versioned based on the snapshot date.
