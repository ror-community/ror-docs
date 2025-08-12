---
title: Identifier pattern
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR Identifier Pattern
  description: >-
    The preferred form of a ROR identifier is the entire URL, but other forms
    are also recognized by the ROR API. The unique string in ROR identifiers
    follows a consistent pattern that can be validated with regular expressions.
  robots: index
next:
  description: ''
  pages:
    - type: link
      title: Discussion of string vs URL value for ROR IDs
      url: https://github.com/ORCID/ORCID-Source/issues/6520
---
The preferred form of a ROR identifier (a "ROR ID", the `id` element in a ROR record) is the entire URL: `https://ror.org/02mhbdp94.` However, the ROR API will also recognize forms such as `ror.org/02mhbdp94` or `02mhbdp94`, so these forms of the ROR ID may also be used.

The entire URL is the preferred form of a ROR identifier because URL changes can easily be made backward compatible via 301 redirects, whereas IDs stripped of URL components can make it difficult for humans and machines to determine how to resolve that ID. See <https://github.com/ORCID/ORCID-Source/issues/6520> for a fuller discussion of this issue.

The unique strings in ROR identifiers are assigned randomly, not sequentially, and contain no organizational information; therefore, the ROR ID of one organization cannot be predicted from the ROR ID of a related organization. 

The unique portions of ROR identifiers have a consistent pattern and can be validated with regular expressions. The unique string consists of 9 characters: a zero, 6 characters that can be either lower-case letters or integers, and 2 concluding integers. The unique string always begins with a zero so that ROR ID landing pages such as <https://ror.org/02mhbdp94> can be easily differentiated from ROR web pages such as <https://ror.org/about>. 

The ROR ID pattern uses [base 32 Crockford encoding](https://www.crockford.com/base32.html), which excludes letters "I", "L", "O", and "U". The last 2 digits of the ROR identifier are a checksum that follows the [ISO/IEC 7064:2003](https://www.iso.org/standard/31531.html) standard. See [the code that generates the ROR ID and the checksum calculation](https://github.com/ror-community/ror-api/blob/bd040a0d2558a478c06a89118a29eeb9b6142710/rorapi/management/commands/generaterorid.py#L8) for further details. 

Regular expressions to validate the unique portion of the ROR ID pattern include the following:

```text RegEx
^0[a-z|0-9]{6}[0-9]{2}$
```

```text RegEx
^0[a-hj-km-np-tv-z|0-9]{6}[0-9]{2}$
```