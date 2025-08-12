---
title: Add ROR IDs to Crossref DOIs
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    The document discusses the support for ROR IDs in Crossref metadata schema
    for various types of content with affiliations, such as journal articles and
    grants. Multiple affiliations for contributors are supported, aligning with
    JATS v1.1+. ROR is supported in direct XML deposits but not yet in the web
    deposit form.
  robots: index
next:
  description: ''
---
# ROR in Crossref DOIs

[As of v5.3.0 (released in July 2021), the Crossref metadata schema supports ROR IDs](https://www.crossref.org/blog/some-rip-roring-news-for-affiliation-metadata) for all types of content that have an affiliation or institution element, such as journal articles, reports, book chapters, preprints, datasets, dissertations, and many more. See Crossref's [documentation on Affiliations and ROR](https://www.crossref.org/documentation/schema-library/markup-guide-metadata-segments/affiliations/) for more information. ROR is also supported in Crossref's Grant ID metadata schema. 

Note that the `institution` element is repeatable in order to support multiple affiliations for a given contributor, such as when a researcher is affiliated with two organizations. Repeated `institution` elements may also be used for multiple affiliations within a single organization, as for example when a researcher is affiliated with both a university and a subsidiary research institute. 

Affiliations in Crossref schema align with [JATS v1.1+](https://jats.nlm.nih.gov/publishing/tag-library/1.1d1/n-tmv0.html). 

> 📘 Which Crossref tools/services support ROR?
> 
> - ROR **is** supported in [direct XML deposits](https://www.crossref.org/documentation/register-maintain-records/choose-content-registration-method/) using [v5.3.x](https://www.crossref.org/documentation/member-setup/choose-content-registration-method/#00492). 
> - ROR **is not yet** supported in Crossref's [Web deposit form](https://www.crossref.org/documentation/register-maintain-records/web-deposit-form/).
> - ROR **is** supported in Crossref's web-based [Grant registration form](https://www.crossref.org/documentation/register-maintain-records/grant-registration-form/).

## Example: Contributor affiliation (journal article)

Below is an example showing just the contributors section for a journal article deposit using Crossref schema v5.3.0. For the full XML, see the [v5.3.0 journal article best practice example](https://gitlab.com/crossref/schema/-/blob/master/best-practice-examples/journal.article5.3.0.xml). 

For more details about Crossref schema v5.3.0, see the [release notes in GitLab](https://gitlab.com/crossref/schema/-/releases/0.2.0).

```xml
<contributors>
  <person_name sequence="first" contributor_role="author">
    <given_name>Minerva</given_name>
    <surname>Housecat</surname>
    <!--affiliations information (change from 4.x versions)-->
    <affiliations>
      <!-- This example includes all supported institution metadata for an individual affiliation, but if an identifier (ROR, ISNI, or Wikidata) is supplied, the name, place, and acronym metadata is not recommended.-->
      <institution>
        <!-- either institution_name or institution_id are required -->
        <institution_name>University of Chicago</institution_name>
        <!-- multiple institution_id values may be provided, IDs must be provided as a https URL -->
        <institution_id type="ror">https://ror.org/024mw5h28</institution_id>
        <institution_id type="isni">https://isni.org/isni/0000000419367822</institution_id>
        <!-- acronym is optional, not recommended if an identifier is supplied -->
        <institution_acronym>UC</institution_acronym>
        <!-- place metadata is not recommended if an institution_id is supplied -->
        <institution_place>Chicago, IL</institution_place>
        <!-- Department may be supplied when relevant -->
        <institution_department>Department of Physics</institution_department>
      </institution>
      <institution>
        <!-- A ROR, ISNI, or Wikidata ID may be provided on its own as it's all we need to identify an organization's name, acronyms, and location.-->
        <institution_id type="ror">https://ror.org/020hgte69</institution_id>
      </institution>
    </affiliations>
    <ORCID authenticated="true">https://orcid.org/0000-0002-4011-3590</ORCID>
  </person_name>
  <person_name sequence="additional" contributor_role="author">
    <given_name>Josiah</given_name>
    <surname>Carberry</surname>
    <affiliations>
      <institution>
        <institution_id type="ror">https://ror.org/05gq02987</institution_id>
      </institution>
    </affiliations>
    <ORCID authenticated="true">https://orcid.org/0000-0002-1825-0097</ORCID>
  </person_name>
  <anonymous sequence="first" contributor_role="author">
    <!-- affiliations may be provided for an anonymous contributor -->
    <affiliations>
      <institution>
        <institution_id type="ror">https://ror.org/04jrbrj58</institution_id>
      </institution>
    </affiliations>
  </anonymous>
</contributors
```

## Example: Investigator affiliation (grant ID)

Below is an example showing just the investigators section for a grant ID registration deposit using Crossref Grant ID schema v0.0.1. For the full XML, see the [v0.0.1 grant ID full metadata XML example](https://gitlab.com/crossref/schema/-/blob/master/examples/grantid_full_example.xml)

For more details about registering Grant IDs with Crossref, see Crossref's [introduction to grants](https://www.crossref.org/documentation/research-nexus/grants/) and the full [Crossref grant ID v0.0.1 XSD](https://gitlab.com/crossref/schema/-/blob/master/schemas/grant_id0.0.1.xsd)

```xml
<investigators>
  <person role="lead_investigator" start-date="2018-01-01">
    <givenName>Elvis</givenName>
    <familyName>Yausudder</familyName>
    <alternateName>Elvis Slozzuffer</alternateName>
    <affiliation>
      <institution country="US">Crossref</institution>
      <ROR>https://ror.org/02twcfp32</ROR>
    </affiliation>
    <affiliation>
      <institution>University of Metadata</institution>
    </affiliation>
    <ORCID>https://orcid.org/0000-0002-4011-3590</ORCID>
  </person>
  <person role="co-lead_investigator" start-date="2019-01-01" end-date="2019-03-31">
    <givenName>Valerie</givenName>
    <familyName>Torf</familyName>
    <affiliation>
      <institution country="US">Crossref</institution>
      <ROR>https://ror.org/02twcfp32</ROR>
    </affiliation>
  </person>
</investigators>
```