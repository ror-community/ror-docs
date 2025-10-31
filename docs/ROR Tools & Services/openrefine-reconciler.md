---
title: OpenRefine reconciler
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR OpenRefine Reconciler
  description: >-
    OpenRefine is a tool for cleaning up messy data, and it integrates with ROR
    to match organization names to ROR IDs without coding. It's best for small
    organization lists, and the process involves confirming matches manually.
  robots: index
next:
  pages:
    - title: Open Refine
      type: link
      url: https://openrefine.org/
---
## What is OpenRefine

[OpenRefine](https://openrefine.org) (formerly Google Refine) is a free, open source desktop tool for cleaning up messy data stored in common formats like CSV, JSON, XML, XLS. You can even connect to SQL-based databases and Google Sheets. OpenRefine is a popular tool for tasks like normalizing text values in a dataset because it has a simple user interface and doesn't require coding.

## How does ROR integrate with OpenRefine?

OpenRefine integrates with many external services that support the [W3C Reconciliation Service API protocol](https://reconciliation-api.github.io/specs/latest/) for matching data on the Web, including ROR.

We've built a [reconciliation API extension](https://reconciliation-api.github.io/testbench/) - the **ROR OpenRefine Reconciler** - that allows matching organization names in an OpenRefine project to ROR IDs using the ROR REST API, but with no coding needed!

**Note that the ROR OpenRefine Reconciler matches names to ROR records with a status of`active` only. ROR records with a status of `inactive` or `withdrawn` will not be displayed as possible matches.**

## What use cases is this tool best for?

The Reconciler requires manually confirming matches between organization names and ROR IDs, so it's useful for cases where you have a relatively small number of organizations names (up to hundreds or perhaps several thousand, if you have time and patience).

For large organization lists with many thousands of records, we recommend using the [REST API](doc:rest-api) or [data dump](doc:data-dump), but this will require some coding. See our guide [Match organization names to ROR IDs](doc:matching) for tips and code examples.

## Using the ROR OpenRefine Reconciler

### Prerequisites

* [Download and install OpenRefine](https://openrefine.org/download.html) on your computer

* [Create a project by importing data](https://docs.openrefine.org/manual/starting#create-a-project-by-importing-data) that contains a column with organization names

### Usage instructions

1. Click the arrow beside the heading of the organization names column and choose **Reconcile > Start reconciling...**

<Image border={false} src="https://files.readme.io/eea1c62-col-reconcile.png" title="col-reconcile.png" />

2. In the window that opens, click **Add standard service...** , enter `https://reconcile.ror.org/reconcile` and click **Add service**

<Image border={false} src="https://files.readme.io/4dd3b4e-add-service.png" title="add-service.png" />

3. Leave the other settings as they are and click **Start reconciling**

<Image border={false} src="https://files.readme.io/2050b30-start-reconciling.png" title="start-reconciling.png" />

4. Processing may take a few minutes, especially for long lists

<Image border={false} src="https://files.readme.io/9e1ac90-processing.png" title="processing.png" />

5. A list of possible ROR matches (if available) are displayed below the original organization name value each cell. Hover over each match to see more information from ROR. Choose your preferred ROR match by clicking the checkbox beside it. Click the double checkbox to assign your chosen ROR match to the current cell and any identical cells in the same column.

<Callout icon="❗️" theme="error">
  When you select an organization match from ROR, OpenRefine will change the original value in your organization name column to the name in the corresponding ROR record. If you want to retain the original names, make a copy of your organization names column in OpenRefine before you start using the ROR Reconciler.
</Callout>

<Image border={false} src="https://files.readme.io/c18c7d0-choose-match.png" title="choose-match.png" />

6. In cases where no match was found, you can search ROR for name variations by clicking **Search for match** and entering variations in the search box. If you find a good match, choose it from the dropdown and click **Match**. If not, click **Don't reconcile cell**.

<Callout icon="📘" theme="info">
  If you're not able to find a ROR ID for a particular research organization, you can suggest additions, which are handled through the ROR community curation process. Learn [how to suggest additions and changes to ROR](doc:updates) .
</Callout>

<Image border={false} src="https://files.readme.io/02321a7-no-match.png" title="no-match.png" />

7. Next, we'll copy just the ROR IDs to a new column. Click the arrow beside the heading of the organization names column and choose **Edit column > Add column based on this column...**

<Image border={false} src="https://files.readme.io/4e25d9b-add-id-col.png" title="add-id-col.png" />

8. Enter a name for your ROR IDs column in the **New column name** field, enter `cell.recon.match.id` in the **Expression** field and Click OK.

<Image border={false} src="https://files.readme.io/8210898-new-col-name.png" title="new-col-name.png" />

9. You should now have a list of organization names and the corresponding ROR IDs that you selected. Export your project to your desired format following the directions in the [OpenRefine User Manual: Exporting your work](https://docs.openrefine.org/manual/exporting)

<Image border={false} src="https://files.readme.io/8355e97-org-names-ror-ids.png" title="org-names-ror-ids.png" />

## Tutorial video

<Embed url="https://www.youtube.com/watch?v=woJiFHBmRCE" href="https://www.youtube.com/watch?v=woJiFHBmRCE" html="%3Ciframe%20class%3D%22embedly-embed%22%20src%3D%22%2F%2Fcdn.embedly.com%2Fwidgets%2Fmedia.html%3Fsrc%3Dhttps%253A%252F%252Fwww.youtube.com%252Fembed%252FwoJiFHBmRCE%253Ffeature%253Doembed%26display_name%3DYouTube%26url%3Dhttps%253A%252F%252Fwww.youtube.com%252Fwatch%253Fv%253DwoJiFHBmRCE%26image%3Dhttps%253A%252F%252Fi.ytimg.com%252Fvi%252FwoJiFHBmRCE%252Fhqdefault.jpg%26key%3Df2aa6fc3595946d0afc3d76cbbd25dc3%26type%3Dtext%252Fhtml%26schema%3Dyoutube%22%20width%3D%22854%22%20height%3D%22480%22%20scrolling%3D%22no%22%20title%3D%22YouTube%20embed%22%20frameborder%3D%220%22%20allow%3D%22autoplay%3B%20fullscreen%22%20allowfullscreen%3D%22true%22%3E%3C%2Fiframe%3E" />

## Additional OpenRefine resources

* [OpenRefine User Manual](https://docs.openrefine.org/)
* [Library Carpentry OpenRefine Lesson](https://librarycarpentry.org/lc-open-refine/)

<br />
