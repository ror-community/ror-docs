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

```xml JATS XML
<article-meta>
 ...  
 <contrib-group>
  <contrib contrib-type="author">
   <name><surname>Bezus</surname>
    <given-names>Evgeni A.</given-names></name>
   <xref ref-type="aff" rid="aff1"/>
  </contrib>
  <contrib contrib-type="author">
   <name><surname>Bezus</surname>
    <given-names>Evgeni A.</given-names></name>
   <xref ref-type="aff" rid="aff1"/>
  </contrib>
 </contrib-group>
 <aff id="aff1">
  <institution-wrap>
   <institution-id institution-id-type="ROR">https://ror.org/03134gf68</institution-id>
   <institution content-type="university">Samara National Research University</institution>
  </institution-wrap>
 </aff>
 ...
</article-meta>
```

See also the 2019 ROR poster from JATS-Con ["Unambiguously Identify Research Organizations in JATS with ROR IDs."](https://jats-con.figshare.com/articles/poster/Unambiguously_Identify_Research_Organizations_in_JATS_with_ROR_IDs_/8137961)
