---
title: Transition from GRID to ROR
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: >-
    Digital Science's GRID has retired its public data after 6 years, with the
    final public dataset published on 16 Sep 2021. The transition to ROR has
    been completed, with ROR IDs now available for every GRID record.
  robots: index
next:
  description: ''
---
After 6 years, Digital Science’s GRID retired its public data, with the [final public dataset published 16 Sep 2021](https://doi.org/10.6084/m9.figshare.16685428). For background on this transition, see the [announcement from July 2021](https://ror.org/blog/2021-07-12-ror-grid-the-way-forward).

We’re excited about this transition and thankful to Digital Science for providing the GRID seed data to launch ROR and for helping to coordinate registry updates. 

> 📘 Final GRID public release published 16 Sep 2021
>
> **GRID published its final public released on 16 Sep 2021:** [https://doi.org/10.6084/m9.figshare.16685428](https://doi.org/10.6084/m9.figshare.16685428)
>
> The final GRID release contains ROR IDs for every GRID record in that release, so that users can easily map from GRID to ROR. 
>
> **A corresponding ROR dataset was released on 23 Sep 2021:** [https://doi.org/10.5281/zenodo.5534443](https://doi.org/10.5281/zenodo.5534443)
>
> This ROR release contains all new/updated records from the final GRID release. Additionally, a simplified CSV file with the ROR ID, name, country and GRID ID from each record is available at [https://doi.org/10.5281/zenodo.5534785](https://doi.org/10.5281/zenodo.5534785)

## I’ve been downloading the GRID dataset and using it in my system. How can I switch to ROR?

* Recent ROR datasets are in [Zenodo](https://zenodo.org/communities/ror-data). You can use the Zenodo API (which works very similarly to Figshare's API) to download them. For instructions, see [downloading the ROR data dump using the Zenodo API](doc:data-dump#download-ror-data-dumps-programmatically-with-the-zenodo-api). 

* ROR datasets prior to Sep 2021 are available in [Figshare](https://doi.org/10.6084/m9.figshare.c.4596503.v9), so you can download existing datasets from the Figshare UI or API, just like with GRID. Copies will be added to [Zenodo](https://zenodo.org/communities/ror-data) soon, so that all ROR data dumps will be available in Zenodo.

* Version 1 of [ROR's JSON data structure](doc:ror-data-structure) is identical to GRID, so you should be able to adapt any code that you used to process GRID data in order to process ROR data relatively easily.  

* Every GRID ID that existed as of Sep 2021 has a one-to-one match to a ROR ID. In ROR records, you'll find the corresponding GRID ID in `external_ids.all` where the `type` is `grid`. If a given record also contains a non-null value in `externals_ids.preferred`, it is identical to the value in `externals_ids.all`, since ROR and GRID records have a 1 to 1 correlation.

* All records in the [final GRID release on 16 Sep 2021](https://doi.org/10.6084/m9.figshare.16685428) include a ROR ID, which you will find in `externals_ids.ROR.all`.

* New ROR records added after Sep 2021 do not have a corresponding GRID ID.

## I have GRID IDs in my system. How can I find the corresponding ROR IDs?

* If you want to find a single ROR record that matches a single GRID ID, you can enter the GRID ID in the ROR web search interface at [https://ror.org/search](https://ror.org/search). Be sure to surround it with double quotation marks, e.g., "grid.463243.4" - [https://ror.org/search?page=1\&query=%22grid.463243.4%22](https://ror.org/search?page=1\&query=%22grid.463243.4%22)

* If you have entire GRID metadata records in your system and are using v1 of the ROR metadata schema or API, you can find the corresponding ROR ID in `externals_ids.ROR.all`. If a given record also contains a non-null value in `externals_ids.ROR.preferred`, it is identical to the value in `externals_ids.ROR.all`, since ROR and GRID records have a 1 to 1 correlation.

* If you only have GRID IDs and not entire metadata records, you can search for the matching ROR ID using the ROR API, data dump, or this [CSV file extracted from the Sep 2021 ROR data dump](https://doi.org/10.5281/zenodo.5114330). For more information and examples, see our guide [Map other organization ID types to ROR](doc:mapping).

## Will ROR continue to add GRID IDs to its data after GRID becomes non-public?

No. After the final GRID release, the ROR dataset will diverge from GRID and new records added to ROR will not contain a GRID ID.

## Once the public-facing GRID website is gone, what will happen when I try to resolve a GRID ID URL?

As of 30 June 2022, GRID identifier URLs no longer resolve to their corresponding GRID record. GRID has chosen to redirect all GRID ID URLs to its landing page, [https://grid.ac](https://grid.ac). Historical GRID records can be found in the [Internet Archive Wayback Machine](https://web.archive.org/web/*/https://grid.ac/institutes)

Since they no longer resolve, ROR removed links to GRID records from its search UI as of 1 July 2022.
