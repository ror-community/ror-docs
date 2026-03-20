---
title: Add ROR IDs to ORCID records
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    ORCID now supports ROR as an organization identifier in researcher profiles
    and the API. Organizations can also include ROR IDs in affiliations using
    the ORCID Affiliation Manager tool.
  robots: index
next:
  description: ''
---
ROR is intended to help connect people (researchers), places (organizations, like funders and research institutions) and things  (research outputs, like publications and datasets) through the power of open persistent identifiers (PIDs), in order to easily answer questions like:

* Which publications and datasets were authored by researchers at a particular institution?
* Which publications and datasets are the result of research supported by a particular funding agency?
* Which researchers are affiliated with a given institution?

To harness the power of PIDs in this way, ROR identifiers need to be included in metadata for other identifiers, such as [ORCID](https://orcid.org/) records for researchers.

## ROR in ORCID profiles

[As of Oct 2021](https://info.orcid.org/add-research-institution-identifiers-with-ror/), ORCID supports ROR as an organization identifier for any schema element that includes an [organization field](https://github.com/ORCID/orcid-model/blob/578cd716cb7222f9a7adfbfdd8a8c94aad254b7e/src/main/resources/common_3.0/common-3.0.xsd#L180), such as education and employment affiliations, funding items, research resources and peer reviews.

Researchers who add or update these activities in their profile will see a list of suggestions as they begin to type that come from both ROR and the Crossref Open Funder Registry. Once they select an organization and save the activity, the update will appear in the profile. Clicking "Show more detail" will reveal the identifier and metadata associated with the selected organization.

<Image align="center" border={true} src="https://files.readme.io/65232ca3c1acd262da384ea424a3a5a071bf91b0aa05cc125336a69f5f592401-orcid-profile-ror.gif" className="border" />

## Organizations without a ROR ID in ORCID

Organizations that do not yet have a ROR ID can also be used in ORCID profiles by typing the name and other required information in the ORCID form and clicking "Save changes." To request a ROR ID for an organization, complete the <Anchor label="ROR curation request form" target="_blank" href="https://curation-request.ror.org">ROR curation request form</Anchor>. Please be aware that it might take several weeks for the new ROR ID to be created (if the request is approved) and then to appear in the list of organizations in ORCID.

<Image align="center" alt="Entering an organization without a ROR ID into an ORCID profile" border={true} src="https://files.readme.io/7d046fbabec2e91d9f6b54421aab2aa31fafe93899620876d4be5179ca3a61bb-orcid-no-ror.gif" className="border" />

## ROR in the ORCID API

Below is an example showing just the organization section for an employment affiliation item using ORCID schema v3.0. For a full XML example, see [ORCID's employment XML sample file](https://github.com/ORCID/orcid-model/blob/578cd716cb7222f9a7adfbfdd8a8c94aad254b7e/src/main/resources/record_3.0/samples/write_samples/employment-3.0.xml).

For more information on using the ORCID API, see the [ORCID API Guide](https://github.com/ORCID/orcid-model/blob/master/src/main/resources/record_3.0/README.md) and [ORCID API Tutorials](https://info.orcid.org/documentation/api-tutorials/).

```xml
<common:organization>
  <common:name>ORCID</common:name>
  <common:address>
    <common:city>Bethesda</common:city>
    <common:region>MD</common:region>
    <common:country>US</common:country>
  </common:address>
  <common:disambiguated-organization>
    <common:disambiguated-organization-identifier>https://ror.org/04fa4r544</common:disambiguated-organization-identifier>
    <common:disambiguation-source>ROR</common:disambiguation-source>
  </common:disambiguated-organization>
</common:organization>
```

## ROR in the ORCID Affiliation Manager

Those using the [Affiliation Manager tool](https://info.orcid.org/documentation/member-portal/member-portal-affiliation-manager-guide/) in the [ORCID Member Portal](https://info.orcid.org/documentation/member-portal/) can include ROR IDs in affiliations added to ORCID records.

* If you are using the [CSV bulk upload](https://info.orcid.org/documentation/member-portal/member-portal-affiliation-manager-guide/#Bulk_upload_CSV), include the full ROR URL in the `disambiguated-organization-identifier` column.
* If you are using the [manual entry form](https://info.orcid.org/documentation/member-portal/member-portal-affiliation-manager-guide/#Add_affiliations_manually), include the full ROR URL in the Organization ID field.
