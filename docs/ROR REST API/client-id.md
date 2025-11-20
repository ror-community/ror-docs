---
title: Register for a client ID
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  pages:
    - title: 'Form: Register for a ROR API client ID'
      type: link
      url: https://ror.org/api-client-id
---
> 🚧 Register a client ID by January 2026 in order to continue receiving the current rate limit for the ROR REST API.

In October 2024, ROR [asked for community feedback](https://ror.readme.io/docs/feedback-docs#ror-api-client-identification--rate-limiting) on ways to ensure the health and stability of the free ROR REST API amid growing use, including an increase in "impolite" use. After extensive community discussion, the final proposal adopted was to **add lightweight API client identification to the ROR API** that will be required in order to receive a rate limit of **2000 requests per 5-minute period**. Requests without identification will receive a lower rate limit of 50 requests per 5-minute period.

The ROR REST API is a free and open community resource. Client IDs enable better monitoring and managing of API traffic, which is essential to prevent many forms of malicious or inadvertent misuse that can degrade service for everyone. Client IDs further allow the ROR team to contact API users whose requests generate lots of errors or are incorrectly formatted and help them improve the structure of their requests. Additionally, client IDs give the ROR team more insight into API usage patterns than can be gathered from IP addresses alone, and this information can help guide technical decisions and architecture approaches.

## How to register a client ID

ROR client IDs are free and easy to obtain. You will fill out [a brief form](https://ror.org/api-client-id) in which the only required field is an email address, your client ID will be sent to the email address you provide, and you can then begin to include the client ID you receive in the header of your requests to the ROR API as explained below.

Note that ROR API client IDs are not used for either authentication or authorization, and therefore they are not secret and can be transmitted in plain text. They are used only to differentiate users of the API.

**Register now for a ROR API client ID:[ https://ror.org/api-client-id](https://ror.org/api-client-id).**

## When to register a client ID

### July 2025

Beginning in July 2025, client IDs for the ROR REST API will be in a preview period. Users can obtain a valid client ID, but no special rate limiting will be applied to unidentified users; both identified and unidentified users of the ROR API will continue to receive a rate limit of 2000 requests per 5-minute period.

### January 2026

**By January of 2026, the ROR API will require a client ID in HTTP request headers in order to receive a rate limit of 2000 requests per 5-minute period.** ROR API requests without a valid client ID will receive a lower rate limit of 50 requests per 5-minute period.

## How to use a client ID

When you submit the [client ID registration form](https://ror.org/api-client-id), you will receive an email at the address you have provided that contains the client ID. In order to receive a rate limit of 2000 requests per 5-minute period, please include this client ID with your ROR API requests in a custom HTTP header named `Client-Id` as in the following example.

```curl
curl -H "Client-Id: RA0Y3G4PL5734ZYTC3ZONUYP33PSBMW6" https://api.ror.org/v2/organizations?query=oxford
```

We do not provide a way to recover, reset, or revoke a lost client ID. If you lose track of your client ID, please register a new one by submitting the [client ID registration form again](https://ror.org/api-client-id).

## More about client IDs

* Client IDs are free to obtain.
* The only information needed to obtain a client ID is a contact email address. Additional fields in the [client ID registration form](https://ror.org/api-client-id) such as name, organization, country and ROR use case are optional.
* Email addresses supplied at registration are used only to contact API users for support and troubleshooting purposes. Email addresses supplied at registration are not used for any other purpose and are not shared outside of ROR technical infrastructure.
* Initially, there will be no hard limit on the number of client IDs that can be generated with a single email address, but we may impose a limit in the future if issues arise.
* Rate limits listed above may be subject to change in the future.
* For help with client IDs, please contact [support@ror.org.](mailto:support@ror.org.)
