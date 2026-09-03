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
The ROR API allows retrieving, searching and filtering the organizations indexed in ROR. The results are returned in JSON. The [code for the ROR API is openly available on GitHub](https://github.com/ror-community/ror-api), and the README on the repository includes instructions for how to run the ROR API locally using Docker.

To suggest features for or report problems with the ROR API, [open an issue on the ROR roadmap](https://github.com/ror-community/ror-roadmap) or email [support@ror.org.](mailto:support@ror.org.)

<Callout icon="📘" theme="info">
  ## ROR API&#x20;

  `https://api.ror.org/v2/organizations/`
</Callout>

# Responses

Queries to the ROR API will return all [Fields and sub-fields](doc:fields) in ROR's [Data structure](doc:ror-data-structure) regardless of whether they have a value. JSON will include null values and empty arrays and objects if there is no value available for the given organization.

Values in fields that contain multiple values are sorted by Unicode value, which is alphabetical for characters in the Basic Latin set.

Beginning 1 Dec 2022, the ROR API by default returns only records whose [status](doc:ror-data-structure#status) is "active". Records with the status values "inactive" and "withdrawn" can be included using the query parameter `?all_status`. In addition, after this date, some ROR records contain the new values "Predecessor" and "Successor" in `relationships.type`. See the changelog post [2022-12-01 Organization status changes](changelog:2022-12-01-organization-status-changes) and our documentation on [Relationships and hierarchies](doc:relationships) for more details.

# Parameters

The ROR API supports three primary parameters, each of which has different use cases.

* The [Query parameter](doc:api-query) is best for finding organization records by name keywords or by non-ROR identifiers (GRID, Wikidata, Crossref Funder ID, ISNI) and is the recommended parameter to use when [creating user-facing forms](doc:forms). The ROR [web search](doc:web-search) is powered by the query parameter of the ROR API. Queries can be [filtered](doc:api-filtering) by location, organization type, and record status.
* The [Advanced query parameter](doc:api-advanced-query) is best for finding organization records by created or last modified date, domain, website, Wikipedia page, or relationship to other organizations. Queries can be [filtered](doc:api-filtering) by location, organization type, and record status.
* The [Affiliation parameter](doc:api-affiliation) is best for large-scale programmatic matching of complex, unstructured text strings to ROR IDs.

# Registration and rate limits

No registration is currently required to use the ROR API, but note that the rate limit is a **maximum of 2000 requests in a 5-minute period** per IP address, and API traffic can be quite heavy at popular times like midnight UTC. If you need to make more requests or want to ensure faster response times, you can also run the entire ROR API locally in Docker. [See the README on the ROR API GitHub repository](https://github.com/ror-community/ror-api#readme) for instructions on running the ROR API locally.

The API is best for use cases that involve querying or retrieving individual records. The maximum number of results that can be retrieved via the API is 10,000, which means that **it is currently not possible to retrieve all 120,000+ records from the ROR API**. If you need to use the entire ROR dataset in your application, please download the [data dump](https://ror.readme.io/docs/data-dump).

# News and support

Users of the ROR API are strongly encouraged to sign up for the [ROR Technical Forum](https://groups.google.com/a/ror.org/g/ror-tech) Google Group in order to receive announcements, calls for feedback, release notifications, and other important information about the ROR API. Message volume is about twice monthly. ROR API users are also welcome to ask technical questions in the group.

# Heartbeat

If your application uses the ROR API and you'd like it to send a quick health check to determine if the ROR API is operational, you can send a query to the ROR API heartbeat at [https://api.ror.org/heartbeat](https://api.ror.org/heartbeat). If the ROR API is up, you will receive a status of `OK`.

# Status and uptime

If you'd like to check manually on the status of the ROR API or assess its uptime, see [https://ror1.statuspage.io/](https://ror1.statuspage.io) for full API status details and history. Our current API status and recent history is below.

<Embed title="" typeOfEmbed="iframe" url="https://ror1.statuspage.io" height="580px" href="https://ror1.statuspage.io" />

# Usage insights

Want to see how others are using the ROR API? Visit the [public ROR API usage insights dashboard by DataDog](https://p.datadoghq.eu/sb/db1aec04-0c1a-11ec-860a-da7ad0900005-7d7c572812608235cca3359ee5ec591a).
