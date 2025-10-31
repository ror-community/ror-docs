---
title: About the ROR REST API
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: About the ROR REST API
  description: >-
    Recent changes to the ROR REST API include default return of only active
    records, new query parameter for all statuses, and updated values in status
    and relationships. Users can access the API without registration, but there
    is a rate limit of 2000 requests in a 5-minute period. Join the ROR
    Technical Forum for updates and support, and check the API status at
    ror1.statuspage.io.
  keywords:
    - ROR REST API
  robots: index
---
> 👍 ROR REST API v2
>
> This page documents v2 of the ROR REST API. For v1 documentation of the ROR REST API, see [https://ror.readme.io/v1/docs/rest-api](https://ror.readme.io/v1/docs/rest-api). You can also read more about ROR [API versions](doc:api-versions) and a summary of what's new in [Schema 2.0](doc:schema-v2) and [Schema 2.1](doc:schema-2-1).The ROR REST API allows users to retrieve, search, and filter the organizations indexed in ROR. The API is built with Django, indexing and search is enabled by Elasticsearch, and results are returned as JSON. Version 2 of the ROR REST API, released in April 2024, is available at **[https://api.ror.org/v2/organizations](https://api.ror.org/v2/organizations)**.

> ❗️ Version 1 of the ROR schema and API will be sunset in December 2025
>
> In December 2025, version 1 of the ROR schema and API will be sunset, meaning that ROR API requests with v1 in the path will no longer return a response, v1 files will no longer be included in the ROR data dump, and v1 documentation will no longer be available. Read more in our [changelog](https://ror.readme.io/changelog/2025-07-01-sunset-of-version-1#/).

Queries to the ROR API will return all [fields](doc:fields) in ROR's [data structure](doc:ror-data-structure) regardless of whether they have a value. JSON will include null values and empty arrays and objects if there is no value available for the given organization. Values in fields that contain multiple values are sorted by Unicode value, which is alphabetical for characters in the Basic Latin set.

Beginning 1 Dec 2022, the ROR API by default returns only records whose [status](doc:data-structure#status) is "active". Records with the new status values "inactive" and "withdrawn" can be included using the new query parameter `?all_status`. In addition, after this date, some ROR records contain the new values "Predecessor" and "Successor" in `relationships.type`. See the [2022-12-01 changelog post](https://ror.readme.io/changelog/2022-12-01-organization-status-changes) for more details.

# Registration and rate limits

No registration is currently required to use the ROR API, but note that the rate limit is a **maximum of 2000 requests in a 5-minute period** per IP address, and API traffic can be quite heavy at popular times like midnight UTC. If you need to make more requests or want to ensure faster response times, you can also run the entire ROR API locally in Docker. [See the README on the ROR API GitHub repository](https://github.com/ror-community/ror-api#readme) for instructions on running the ROR API locally.

The API is best for use cases that involve querying or retrieving individual records. The maximum number of results that can be retrieved via the API is 10,000, which means that **it is currently not possible to retrieve all 110,000+ records from the ROR API**. If you need to use the entire ROR dataset in your application, please download the [data dump](https://ror.readme.io/docs/data-dump).

> 🚧 Register for a client ID by December 2025
>
> Beginning in December 2025, ROR API requests will need to use a client ID in order to receive the current rate limit of 2000 requests per 5 minute period. Requests without identification will receive a lower rate limit of 50 requests per 5 minute period. There is no cost to register a client ID. Register at [https://ror.org/api-client-id](https://ror.org/api-client-id) and read more about ROR API client IDs at [https://ror.readme.io/docs/client-id](https://ror.readme.io/docs/client-id).

# News and support

Users of the ROR API are strongly encouraged to sign up for the [ROR Technical Forum](https://groups.google.com/a/ror.org/g/ror-tech) Google Group in order to receive announcements, calls for feedback, release notifications, and other important information about the ROR API. Message volume is about twice monthly. ROR API users are also welcome to ask technical questions in the group.

# Heartbeat

If your application uses the ROR API and you'd like it to send a quick health check to determine if the ROR API is operational, you can send a query to the ROR API heartbeat at [https://api.ror.org/heartbeat](https://api.ror.org/heartbeat). If the ROR API is up, you will receive a status of `OK`.

# Status and uptime

If you'd like to check manually on the status of the ROR API or assess its uptime, see [https://ror1.statuspage.io/](https://ror1.statuspage.io) for full API status details and history. Our current API status and recent history is below.

<Embed url="https://ror1.statuspage.io" href="https://ror1.statuspage.io" height="580px" iframe="true" />

# Usage insights

Want to see how others are using the ROR API? Visit the [public ROR API usage insights dashboard by DataDog](https://p.datadoghq.eu/sb/db1aec04-0c1a-11ec-860a-da7ad0900005-7d7c572812608235cca3359ee5ec591a).
