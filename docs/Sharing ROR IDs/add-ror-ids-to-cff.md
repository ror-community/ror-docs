---
title: Add ROR IDs to CITATION.cff files
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: >-
    ROR IDs can be used in the Citation File Format (CFF), which enables the
    inclusion of citation metadata for software or datasets in easily readable
    plaintext files for both humans and machines.
  keywords:
    - software
    - ' citation'
  robots: index
next:
  description: ''
  pages:
    - type: link
      title: Citation File Format on GitHub
      url: https://github.com/citation-file-format/citation-file-format
---
The Citation File Format (CFF) lets you provide citation metadata for software or datasets in plaintext YAML files that are easy to read by both humans and machines. Beginning with version 1.3.0 of CFF, ROR IDs can be used to identify entities in CFF, including for the institutional affiliations of software authors and for organizations credited as institutional authors of the software. 

> ℹ️ Read more about CFF
> 
> To learn more about the Citation File Format, consult the [CFF README](https://github.com/citation-file-format/citation-file-format/blob/main/README.md) and the [Guide to the CFF schema](https://github.com/citation-file-format/citation-file-format/blob/main/schema-guide.md) .

Here is an example of a CFF file:

```yaml
cff-version: 1.3.0  
message: If you use this software, please cite it using these metadata.  
title: My Research Software  
abstract: This is my awesome research software. It does many things.  
authors:
  - family-names: Druskat  
    given-names: Stephan  
    orcid: "https://orcid.org/1234-5678-9101-1121"
    affiliation: 
      - name: German Aerospace Center
        ror: https://ror.org/04bwf3e34
      - name: "The Netherlands eScience Center"
        ror: https://ror.org/00rbjv475
version: 0.11.2  
date-released: "2021-07-18"  
identifiers:
    - description: This is the collection of archived snapshots of all versions of My Research Software  
      type: doi  
      value: "10.5281/zenodo.123456"
    - description: This is the archived snapshot of version 0.11.2 of My Research Software  
      type: doi  
      value: "10.5281/zenodo.123457"  
license: Apache-2.0  
repository-code: "https://github.com/citation-file-format/my-research-software"

```

Note that in this example, the software has two authors: a person named "Stephan Druskat" who is affiliated with the German Aerospace Center, and an organization named "The Netherlands eScience Center." Both the person's affiliation and the organizational author can be identified with ROR IDs.