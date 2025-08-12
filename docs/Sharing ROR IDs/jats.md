---
title: Add ROR IDs to JATS XML
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    This document explains how to include ROR IDs in JATS XML using the
    `<institution-id>` element, with examples provided in the JATS
    `<institution-id>` tag library documentation.
  robots: index
next:
  description: ''
---
ROR IDs can be included in JATS XML. Recommended practice is to use the `<institution-id>` element. See the [JATS `<institution-id>` tag library documentation](https://jats.nlm.nih.gov/publishing/tag-library/1.3/element/institution-id.html) for more examples and instructions. 
[block:code]
{
  "codes": [
    {
      "code": "<article-meta>\n ...  \n <contrib-group>\n  <contrib contrib-type=\"author\">\n   <name><surname>Bezus</surname>\n    <given-names>Evgeni A.</given-names></name>\n   <xref ref-type=\"aff\" rid=\"aff1\"/>\n  </contrib>\n  <contrib contrib-type=\"author\">\n   <name><surname>Bezus</surname>\n    <given-names>Evgeni A.</given-names></name>\n   <xref ref-type=\"aff\" rid=\"aff1\"/>\n  </contrib>\n </contrib-group>\n <aff id=\"aff1\">\n  <institution-wrap>\n   <institution-id institution-id-type=\"ROR\">https://ror.org/03134gf68</institution-id>\n   <institution content-type=\"university\">Samara National Research University</institution>\n  </institution-wrap>\n </aff>\n ...\n</article-meta>",
      "language": "xml",
      "name": "JATS XML"
    }
  ]
}
[/block]
See also the 2019 ROR poster from JATS-Con ["Unambiguously Identify Research Organizations in JATS with ROR IDs."](https://jats-con.figshare.com/articles/poster/Unambiguously_Identify_Research_Organizations_in_JATS_with_ROR_IDs_/8137961)