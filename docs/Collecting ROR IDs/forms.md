---
title: Create ROR-powered forms
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    This document provides best practices for implementing a typeahead widget
    that prompts users to select an organization from the ROR registry,
    capturing top-level affiliations and allowing users to search for variant
    names. It also advises against requiring ROR IDs, allowing users to enter
    text strings if no match is found, and considering filtering suggestions
    based on context.
  robots: index
next:
  pages:
    - title: ROR Typeahead Demos
      type: link
      url: https://ror-community.github.io/ror-typeahead-demos/
    - title: ROR Typeahead Demos code repository
      type: link
      url: https://github.com/ror-community/ror-typeahead-demos
    - title: >-
        Video by Marameo Design on designing a UI with ROR for the Research Data
        Alliance
      type: link
      url: https://youtu.be/3aBjcM3ou1M?si=ncRAm5c6lXarhNLu
---
> 🚧 Using ROR with standard form solutions
>
> The information on this page is aimed at developers who are building and maintaining web applications that include custom forms. If you are using a standard form solution such as Google Forms, we recommend collecting organization names as text in your form, generating a CSV file from the form responses, and then [matching the organization names to ROR IDs](doc:matching).

If your system includes fields that users enter affiliation information into, you can standardize that input and capture corresponding ROR IDs by adding a typeahead (or "autosuggest") widget that prompts users to select an organization from ROR. Learn more about best practices and implementation steps for using ROR in your application's forms here.

<Image alt="ROR typeahead demo showing a user entering variations on the name Cracow University of Economics into a form" border={false} src="https://files.readme.io/4f93231-cracow-ror-typeahead-new.gif" />

# General best practices

## Capture top-level affiliation with ROR

Use a ROR-powered typeahead to capture creator affiliations (e.g., University of Wisconsin-Madison). Departments, faculties, colleges, and other sub-units should be captured in a separate field, as in the examples below.

## Allow users to search for variant names

The `names` field includes variations on an organization's name such as its name in other languages, acronyms, and aliases, any of which a user might and should be able to search for. The `?query` parameter of the ROR API will search an index of all these name fields, but if you build your own search logic, make sure you configure your typeahead to allow searching for all the values in the `names` field. Read more about the `names` field in ROR's [Data structure](doc:ror-data-structure).

In the below example, the ROR record for the University of Wisconsin-Madison, [https://ror.org/01y2jtd41](https://ror.org/01y2jtd41), contains several values in the `names` field: the `ror_display` name "University of Wisconsin-Madison", the `alias` "UW-Madison", the `acronym` "UW",  and `labels` for the organization's name in French and Spanish. All values are searchable.

<Image align="center" border={false} src="https://files.readme.io/8f07cc7-ror-typeahead-UW-Madison.png" />

## Show additional information besides an organization name

In addition to an organization name, display other ROR record fields in order to help users select the correct organization. We recommend including:

* Name variations such as acronyms, aliases, and names in other languages. This is particularly important because a user's query should be able to match any name variation in a ROR record.

* Geographic information such as city and country.

* Organization type.

We do not recommend displaying ROR IDs to end users. If you wish to display the ROR ID to end users, please consult our [display guidelines](doc:display) for ROR IDs and the ROR logo.  

> 📘 Special consideration for displaying geographical information from ROR: organizations with multiple locations
>
> Beginning with [Schema 2.0](doc:schema-v2), ROR metadata supports multiple locations in a single ROR record. The large majority of ROR records include only a single location; [ROR metadata policies](https://github.com/ror-community/ror-updates/wiki/ROR-Metadata-Policies#multiple-locations) outline the rare circumstances in which a ROR record will qualify for multiple locations.
>
> Developers who wish to pull location information from ROR may therefore wish to plan to accommodate multiple locations, for example by displaying all locations in a drop down or typeahead. In cases where records have multiple locations, no priority is specified and order is not significant. For this reason, displaying only the first location may not be appropriate for all cases. Additionally, in some cases, developers may wish to allow users to edit location fields within their application(s) to reflect the actual location of the user rather than forcing the use of a location from a ROR record.

## Select names for display by language or type

Beginning with [Schema 2.0](doc:schema-v2), organization names in ROR metadata are listed in an array, and ROR no longer assigns a "primary" name for any organization, since these decisions are highly subjective and are often inappropriately Anglocentric. Additionally, a single “primary” name in only one language is inappropriate for organizations located in countries with multiple official languages. See [Data structure - names](https://ror.readme.io/docs/ror-data-structure#names) for more information.

At the same time, many ROR users desire a default name for use in their applications. Therefore, each ROR record has exactly one name of type `ror_display`. The `ror_display` name is used as the main heading on the organization record in the ROR [Web search](doc:web-search) and can be used by those who want to select a single name for primary display in their applications.

<Image align="center" border={true} width="500px" src="https://files.readme.io/988f805e1977fe18da42ad5010c4195186234dab1afb971a0b22ddb2b8b8a90f-Screenshot_2026-01-20_at_4.06.48_PM.png" className="border" />

> 🚧 Do not select the first name in the array
>
> The order of names in the array in the ROR record is neither significant nor predictable. Therefore, order position in the array should not be used to select a name: use the name type or the name language instead.

## Do not require the ROR ID

We strongly discourage requiring users to enter only organizations with ROR IDs, because there are many valid reasons why a user may be unable to select an organization with a ROR ID as their affiliation:

* The user may be affiliated with a research organization that is in scope for ROR but has not yet been added to the registry
* The user may be affiliated with a research organization that is not in scope for ROR, such as a single-person consultancy
* The user may be an independent researcher

Forms can still require users to give an institutional affiliation by allowing users to enter free text strings if no appropriate suggestion is made by the ROR typeahead.

## Do not allow editing of the ROR organization name

After a user chooses an organization from the list, don't allow them to edit the name, as this will likely result in incorrectly matched ROR IDs. Instead, prompt them to choose a new organization from the ROR list or else enter an organization name as a free text string.

<Image border={false} src="https://lh4.googleusercontent.com/s2orC1Hyx-blwbnAwAT2zvBEaU8jLZlvHXd_MBGoO41BRCvo8GZZtlFG_rkXet4mJwbA_sGT_LjOX0EsYhyJulpli2LTy9Kc_-U9rNap04olBUXQ5qb902aMnJrFw7wPLSkYd8UP5G2tYY47chwCqVZ7T7MBZ6toR_0ns_SYXOKjLC9sW2U7VgrQie2b" />

## Allow entering a text string if no ROR match is found

Allow the user to enter the organization name as a text string if no appropriate option is suggested by the typeahead.

<Image border={false} src="https://lh6.googleusercontent.com/lZl0oiKuYUAytx8H-Rv9Oa93tcVCOtqhR6vrQlEnz5D3RxAy4hwV0Luja73MaHa4q6AqLFea4FHJzYfdDDCiXtH1QQdTcXcKy-_2-US5rEr3rSrUqxJIa8mgG0jya0k5_hQqJgbwrdqujOrpoly7Fb1Gn1h3Xsy54ohcyueV-OBZfU023GC0TI2bBrMS" />

## Allow users to provide multiple affiliations

Allow users to provide multiple affiliations, since many users are affiliated with multiple organizations. In addition, many users are affiliated with **both** a high-level organization such as the [University of Wisconsin-Madison](https://ror.org/01y2jtd41) **and** a "child" research organization such as the [Morgridge Institute for Research](https://ror.org/05cb4rb43). Making the ROR-powered institutional affiliation field repeatable enables users to provide both affiliations.

See [ROR hierarchies and relationships](doc:relationships) for more information about parent, child, and sibling research organizations in ROR.

<Image border={false} src="https://files.readme.io/7b7847d-typeahead-morgridge.png" />

## Follow accessibility best practices

Follow accessibility best practices for form controls, such [W3C Web accessibility initiative (WAI)](https://www.w3.org/WAI/), including:

* Label and group form controls correctly

* Include form instructions in a way that can be read by assistive technologies, such as screen readers

* Ensure that keyboard/tab navigation is possible

* Ensure that form controls function at a variety of screen sizes/zoom levels

* Ensure that colors with sufficient contrast are used, and that color alone is not used to convey information

## Consider filtering the list of suggestions displayed to the user

Consider [Filtering](doc:api-filtering) the list of suggestions displayed to the user based on context, such as the user's email domain, organization type, or location (either browser geolocation or location information entered in other fields on the same form).

<Image border={false} src="https://lh3.googleusercontent.com/nr_HsqKJoVWObgIx27yqGTZNx84kWzSFUxQUwEercYX-01H1FzdYN2c0w5_hlTUbzoZd3nMsQVDexwhbdpQTH1-MWFotYjIhNyQ6d0IvLmP4JPbo6Zc2qBqAu54vaTiCjjRPSeMhCfTIyyPsybmSUGMpVKLBDaGAi8eX9C1Hav--EoI_A9E6itkT3rVh" />

## Consider populating other fields using ROR data

Consider populating other fields in your form automatically using data from the ROR record selected by the user. See the list of ROR record fields in [ROR data structure](doc:ror-data-structure).

<Image border={false} src="https://lh6.googleusercontent.com/aTclvWe9JZnZaMLH7W6e6tT91l1YC_TrpfOoDVgW1dRHY7vqxY2AOxORlSU3T2Ixfjz6vREyMjhooVAaVVQGv7ua20EqM9VaI-arubiYV6lEOjIKvcx59TB2zeMHydV7kNf9XeKAOPP5XVkKRbynRwl9ROTVtHtT8-86HUhpsJNP-95ZWVRqXTWkHzEZ" />

# Implementation approaches

## ROR API

A simple way to populate a typeahead widget is to query the ROR [REST API](doc:rest-api) in real time as the user types. While this approach has the advantage of being easy to implement, it comes with some disadvantages:

* Slower/more variable response time vs querying data stored locally

* Depends on being able to reach the ROR API

* Performance may be impacted by other ROR API users

Steps to implement a typeahead widget using the ROR [REST API](doc:rest-api) will vary depending on your system architecture, but may include the following:

1. Send user input to the ROR API.

As the user types their affiliation, send the input to the ROR API. We recommend using the [query parameter](doc:api-query) search. You may also wish to first [check the heartbeat of the ROR API](doc:rest-api#heartbeat).

2. Parse results.

Results are returned as a list of ROR records, ordered by matching score. Note that the matching score itself is not returned. Results are paginated, and the first 20 results are returned by default. See example responses in the [Query parameter](doc:api-query) section of the REST API guide.

3. Display results.

Display the top results to the user. In addition to the name, include information from ROR like acronyms, aliases, names in other languages, city, country, URL and organization type to help the user choose the correct result. We do not recommend displaying ROR IDs to users.

4. Store selected result.

Store the ROR ID for the selected result in your system along with any other required information. We recommend storing and displaying ROR IDs as full URLs in the form [https://ror.org/01y2jtd41](https://ror.org/01y2jtd41). Read more about the [ROR identifier pattern](doc:identifier).

## ROR data dump

Regardless of the source data, typeahead widgets perform faster when querying data stored locally vs making requests to external resources.

While performance is much better and both data and search results can be tuned to meet the needs of a particular system, this approach requires more initial development time, as well as ongoing maintenance.

Steps to implement a widget using the ROR [data dump](doc:data-dump) vary depending on your system architecture, but may include the following:

1. Download and parse the ROR data dump into a local database.

2. Create a search index.

3. Create an internal API endpoint with logic to query the search index.

4. Use your internal API endpoint to populate your typeahead widget, following the best practices above.

5. Update your database and search index when new ROR data dumps are released.

# Typeahead demos and code

* Try our demonstration version of a ROR typeahead at [https://ror-community.github.io/ror-typeahead-demos/](https://ror-community.github.io/ror-typeahead-demos/)
* Take a look at the code for our ROR typeahead demos at [https://github.com/ror-community/ror-typeahead-demos](https://github.com/ror-community/ror-typeahead-demos)
* Watch the [short video by Marameo Design](https://youtu.be/3aBjcM3ou1M?si=ncRAm5c6lXarhNLu) on designing a UI with ROR for the Research Data Alliance
