---
title: Logos and display guidelines
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR logos and display patterns
  description: >-
    This document provides guidelines for using the ROR logo in various
    materials and for displaying ROR IDs in web applications, emphasizing the
    importance of following specific design and accessibility standards.
  robots: index
next:
  pages:
    - title: >-
        Video by Marameo Design on designing a UI with ROR for the Research Data
        Alliance
      type: link
      url: https://youtu.be/3aBjcM3ou1M?si=ncRAm5c6lXarhNLu
---
## Using the ROR logo and name

You can use the ROR logo and name in graphics, slide decks, blog posts, websites, research papers, posters, applications or other material that discusses or mentions the Research Organization Registry (ROR) initiative or that uses the ROR API or ROR data. When doing so, please follow these guidelines:

* Use the [scalable SVG icon version](https://raw.githubusercontent.com/ror-community/ror-logos/main/ror-icon-rgb.svg) wherever possible for best image quality.
* Scale the ROR icon to match the line height of the surrounding text, but no smaller than a height of 16px.
* Link to [https://ror.org](https://ror.org) when using the ROR logo to refer to the ROR initiative.
* Display the image as given without rotating it, changing its colors, adding drop shadows or animations, or otherwise altering its appearance.
* Use ROR brand colors <span style={{ color: "#53baa1", fontWeight: "bold" }}>#53baa1</span> or{" "} <span style={{ color: "#2c2c2c", fontWeight: "bold" }}>#2c2c2c</span> whenever possible. In cases where use of ROR brand colors is not possible, use black or white.
* When discussing ROR, use the full name with acronym -- "Research Organization Registry (ROR)" -- upon first mention before using the acronym "ROR". When using ROR IDs only, the acronym alone is acceptable.
* The ROR logo is available under a [CC BY-ND 4.0 license](https://creativecommons.org/licenses/by-nd/4.0/), which means that you must credit ROR (a link is sufficient) and that you may not make derivatives of the image. ROR IDs and metadata are provided under the [Creative Commons CC0 1.0 Universal Public Domain Dedication.](https://creativecommons.org/publicdomain/zero/1.0/)

Approved ROR logo files are available for download from the sources listed under [ROR logo files](#ror-logo-files).

## ROR ID display guidelines

ROR IDs are primarily intended for use in underlying metadata and for internal use by web applications and services, for instance when [creating ROR-powered typeaheads in forms](doc:create-ror-powered-typeaheads). ROR IDs are designed for machine readability to help software and systems disambiguate organizations. **Before displaying the ROR ID, consider carefully whether the reader, viewer, or user needs to see the ROR ID and will understand what the ROR ID represents.** In many cases, a human being may expect or prefer to see an organization's name or a link to the organization's website instead of a ROR ID.

If you do choose to display ROR IDs, please follow the below guidelines.

* Include a link to the ROR record.

* Do not link an organization's name to a ROR record, since users expect that a linked organization name will lead to the organization's website.

* If you choose to display a ROR logo or icon, please use an [approved ROR image file](#ror-logo-files) rather than one from another source.

* For accessibility and usability, link both the ROR ID text (e.g., [https://ror.org/03yrm5c26](https://ror.org/03yrm5c26)) and the icon rather than just the icon.

* Use the [scalable SVG icon version](https://raw.githubusercontent.com/ror-community/ror-logos/main/ror-icon-rgb.svg) wherever possible for best image quality.

* Scale the ROR icon to match the line height of the surrounding text, but no smaller than a height of 16px.

* Make sure that the target size for links to ROR records is at least 44 x 44px in order to comply with [WCAG accessibility guidelines](https://www.w3.org/WAI/WCAG21/quickref/?showtechniques=255#target-size).

* Display the image as given without rotating it, changing its colors, adding drop shadows or animations, or otherwise altering its appearance.

* Use ROR brand colors <span style={{ color: "#53baa1", fontWeight: "bold" }}>#53baa1</span> or{" "} <span style={{ color: "#2c2c2c", fontWeight: "bold" }}>#2c2c2c</span> whenever possible. In cases where use of ROR brand colors is not possible, use black or white.

Approved ROR logo files are available for download from the sources listed under [ROR logo files](#ror-logo-files).

## ROR ID display formats

The preferred form of the ROR ID is the full URL, as explained in our guidance to the ROR [Identifier pattern](doc:identifier). However, the ROR ID may also be displayed in a short form that is still resolvable by web browsers and the ROR API.

### Full ROR ID

Full ROR ID URL (including scheme, host and path) linked to the corresponding ROR record:

[https://ror.org/03yrm5c26](https://ror.org/03yrm5c26)

***

Optionally, include the name of the organization and the term "ROR ID" before the full ROR ID:

California Digital Library, ROR ID [https://ror.org/03yrm5c26](https://ror.org/03yrm5c26)

***

Optionally, include the ROR icon before or after the ID URL:

<a href="https://ror.org/03yrm5c26">
  <img alt="ROR logo" src="https://raw.githubusercontent.com/ror-community/ror-logos/main/ror-icon-rgb.svg" height="24" /> [https://ror.org/03yrm5c26](https://ror.org/03yrm5c26)
</a>

```html Code sample
<a href="https://ror.org/03yrm5c26">
	<img alt="ROR logo" src="https://raw.githubusercontent.com/ror-community/ror-logos/main/ror-icon-rgb.svg" height="24" /> https://ror.org/03yrm5c26
</a>
```

### Short ROR ID

ROR ID domain and unique string, linked to the corresponding ROR record:

[ror.org/03yrm5c26](https://ror.org/03yrm5c26)

***

Optionally, include the name of the organization and the term "ROR ID" before the short ROR ID:

California Digital Library, ROR ID [ror.org/03yrm5c26](https://ror.org/03yrm5c26)

***

Optionally, include the ROR icon before or after the short ROR ID:

<a href="https://ror.org/03yrm5c26">ror.org/03yrm5c26 <img alt="ROR logo" src="https://raw.githubusercontent.com/ror-community/ror-logos/main/ror-icon-rgb.svg" height="24" /></a>

```html Code sample
<a href="https://ror.org/03yrm5c26">
        ror.org/03yrm5c26 <img alt="https://raw.githubusercontent.com/ror-community/ror-logos/main/ror-icon-rgb.svg" height="24" /></a>
```

## ROR logo files

Approved ROR logo image files (png, svg) for use in graphics, slides, websites, and applications are available from the following sources:

* **GitHub:** [https://github.com/ror-community/ror-logos](https://github.com/ror-community/ror-logos)
* **Zenodo:** [https://doi.org/10.5281/zenodo.4701802](https://doi.org/10.5281/zenodo.4701802)
* **Wikimedia Commons:**  [https://commons.wikimedia.org/wiki/File:ROR_logo.svg](https://commons.wikimedia.org/wiki/File:ROR_logo.svg)

Remember that all ROR image files are available under a [CC BY-ND 4.0 license](https://creativecommons.org/licenses/by-nd/4.0/), which means that you must credit ROR (a link is sufficient) and that you may not make derivatives of the image.

If you would like to use an alternate form of the ROR logo, please email [support@ror.org](mailto:support@ror.org) for approval.