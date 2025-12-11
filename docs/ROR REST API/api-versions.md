---
title: API versions
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR API Versions
  description: >-
    The document outlines plans for versioning the ROR metadata schema and API,
    with a new version expected to be released in 2024. Changes will be
    implemented using semantic versioning, with major versions requiring a
    version in the API request path, and previous versions supported until their
    planned sunset date.
  robots: index
next:
  description: ''
---
This page documents policies around ROR API versioning. ROR API v2 [was deployed to production in April 2024](https://ror.readme.io/changelog/2024-04-11-schema-api-v2). ROR API v1 [was sunset in December 2025](https://ror.readme.io/changelog/2025-12-04-version-1-of-the-ror-api-and-schema-is-sunsetting).

# Community feedback

In the autumn of 2022, ROR asked for community feedback on plans for versioning the ROR metadata schema and API. Read the draft and final proposals below.

* [Handling schema & API versioning in ROR (Draft)](https://docs.google.com/document/d/1882i-nUt8rqhd1bLqMJ3hF-wq5i8HYktUYmN_gUR1xM/edit?usp=sharing) - proposal open for comment through November 2022
* [Handling schema & API versioning in ROR (Final Draft)](https://docs.google.com/document/d/18nl6pq0kdCU5ApcdbNjKnV7xHIw9eEY7DJG1WHjaLSs/edit?usp=sharing) - proposal adopted November 2022

# API versioning

The ROR metadata schema and API will be versioned in lockstep, meaning that when a new major schema version is introduced, the API version will also be incremented so that users can unambiguously request a response in a specific schema version.

> 📘 Schema versioning
>
> The ROR API and schema are versioned together, so a new minor or major version of the API will be accompanied by a new major or minor version of the schema. Read more about [Schema versions](doc:schema-versions).

The ROR API will use semantic versioning:

* A minor version (ex, X.1, X.2, etc) will be incremented when substantial non-breaking changes are made, such as changing existing API functionality so that the response to a given request has the same structure but a different meaning, e.g., changing the ?query search behavior to include ?query.advanced behavior.
* A major version (ex, 1.X, 2.X, etc) will be incremented when breaking changes are made, such as removing existing API functionality or significantly changing existing API functionality so that the response to a given request is different in structure, e.g., removing the container element from the ?affiliation response.

# Changes that require versioning

## Minor version change

* Changing existing API functionality such that the response to a given request has the same structure but a different meaning or nature (e.g., current `?query` search behavior is changed to `?query.advanced` behavior)

## Major version change

* Removing API functionality
* Significantly changing existing API functionality such that the response to a given request is different in structure (e.g., removing container element from `?affiliation` response)

# Changes that do not require versioning

* Minor (non-breaking) changes to existing API functionality, such as bug fixes and incremental improvements to search behavior to improve performance/accuracy
* Adding new features to the API that do not impact existing features/functionality (e.g., adding a new endpoint)

# Deploying new API versions

No more than 1 new major API version will be released each year, and ROR will not seek to release a major version each year unless there's need for it. It's very likely that there will be several years between major version releases.

New versions will be made available in the ROR [staging environment](https://api.staging.ror.org/organizations) for approximately 1 month prior to production release. Users will have an opportunity to provide feedback on the technical implementation of the new schema version via ROR communication channels, such as the [ROR Technical Forum](https://groups.google.com/a/ror.org/g/ror-tech).

# Supporting and sunsetting previous API versions

When a new API version is deployed to production, any supported previous versions will continue to be supported until their planned sunset date. This may sometimes result in supporting 3 versions concurrently.

Plans to sunset a previous version will be announced at least 1 year prior to the planned sunset date, via the [ROR community meetings](https://ror.org/events) and other communication channels such as the [ROR blog](https://ror.org/blog), [Mastodon](https://mastodon.social/@ResearchOrgs), [Bluesky](https://bsky.app/profile/researchorgs.bsky.social), [LinkedIn](https://www.linkedin.com/company/ror-research-organization-registry), the [ROR Technical Forum](https://groups.google.com/a/ror.org/g/ror-tech), and the [ROR Community Forum](https://groups.google.com/a/ror.org/g/ror-community). Regular reminders will continue prior to the sunset date.

# Implementing API versions

Major versions will be specified in the path portion of an API request, e.g., `https://api.ror.org/X.X/organizations`.

Minor (non-breaking) API version changes will be implemented within the current major version.

2 major API versions will be supported concurrently (the current major version and the most recent previous version). Requests using an unsupported API version will return a 410 Gone error with a detailed message.
