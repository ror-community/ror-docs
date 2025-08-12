---
title: Add ROR IDs to DataCite DOIs
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    DataCite now supports ROR IDs in fundingReference and creator/contributor
    affiliation fields for all resource types, with examples provided in the
    document.
  robots: index
next:
  description: ''
---
# ROR in DataCite DOIs

[As of v4.3 (released in Aug 2019), the DataCite metadata schema supports ROR IDs](https://blog.datacite.org/identify-your-affiliation-with-metadata-schema-4-3/) in [fundingReference](https://support.datacite.org/docs/datacite-metadata-schema-v44-recommended-and-optional-properties#19-fundingreference) and [creator/contributor affiliation fields](https://support.datacite.org/docs/datacite-metadata-schema-v44-mandatory-properties#25-affiliation) for all resource types.

ROR IDs are supported in DataCite's [Fabrica form](https://support.datacite.org/docs/create-a-doi-via-form), as well as the [MDS API](https://support.datacite.org/docs/mds-api-guide#register-a-doi) and [REST API](https://support.datacite.org/docs/api-create-dois).

When retrieving DOIs with the REST API, you'll need to add the parameter `&affiliation=true` to receive affiliation details in the response. See [DataCite's support site](https://support.datacite.org/docs/can-i-see-more-detailed-affiliation-information-in-the-rest-api) for more information.

## Example: Creator/contributor affiliation

Below are examples showing just the creator and contributor sections for a DataCite DOI. For the full XML, see the [example with affiliation file](https://schema.datacite.org/meta/kernel-4.4/example/datacite-example-affiliation-v4.xml) linked from the latest [DataCite metadata schema documentation](https://schema.datacite.org/meta/kernel-4.4/). 

* `affiliation` element is repeatable, to support multiple affiliations for a given creator/contributor

```xml
<creator>
	<creatorName nameType="Personal">Carberry, Josiah</creatorName>
	<givenName>Josiah</givenName>
	<familyName>Carberry</familyName>
	<nameIdentifier schemeURI="https://orcid.org/" nameIdentifierScheme="ORCID">0000-0002-1825-0097</nameIdentifier>
	<affiliation affiliationIdentifier="https://ror.org/05gq02987" affiliationIdentifierScheme="ROR">Brown University</affiliation>
	<affiliation affiliationIdentifier="grid.268117.b" affiliationIdentifierScheme="GRID" schemeURI="https://grid.ac/institutes/">Wesleyan University</affiliation>
</creator>
```

```xml
<contributors>
  <contributor contributorType="ProjectLeader">
  <contributorName>Starr, Joan</contributorName>
  <givenName>Joan</givenName>
  <familyName>Starr</familyName>
  <nameIdentifier schemeURI="https://orcid.org/" nameIdentifierScheme="ORCID">0000-0002-7285-027X</nameIdentifier>
  <affiliation affiliationIdentifier="https://ror.org/03yrm5c26" affiliationIdentifierScheme="ROR">California Digital Library</affiliation>
  </contributor>
</contributors>
```

## Example: Funding reference

Below is an example showing just the fundingReference section for a DataCite DOI.

* `fundingReference` element is repeatable, to support describing multiple funding sources

```xml
<fundingReferences>
  <fundingReference>
    <funderName>European Commission</funderName>
    <funderIdentifier funderIdentifierType="ROR">https://ror.org/00k4n6c32</funderIdentifier>
    <awardNumber awardURI="https://cordis.europa.eu/project/rcn/100180_en.html">282625</awardNumber>
    <awardTitle>MOTivational strength of ecosystem services and alternative ways to express the value of BIOdiversity</awardTitle>
    </fundingReference>
  <fundingReference>
    <funderName>European Commission</funderName>
    <funderIdentifier funderIdentifierType="ROR">https://ror.org/00k4n6c32</funderIdentifier>
    <awardNumber awardURI="https://cordis.europa.eu/project/rcn/100603_en.html">284382</awardNumber>
    <awardTitle>Institutionalizing global genetic-resource commons. Global Strategies for accessing and using essential public knowledge assets in the life sciences</awardTitle>
  </fundingReference>
</fundingReferences>
```

For more details see DataCite's schema documentation at [https://schema.datacite.org](https://schema.datacite.org) (also available on the [DataCite support site](https://support.datacite.org/docs/datacite-metadata-schema-44))
