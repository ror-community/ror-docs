---
title: Relationships and hierarchies
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ROR relationships and hierarchies
  description: >-
    ROR records store organizational connections in the `relationships` field,
    allowing for the display of hierarchies and connections between
    organizations. The `relationships` element is an array, enabling multiple
    parent, child, and lateral relationships.
  robots: index
next:
  description: >-
    Read our blog post explaining in more detail how ROR handles organizational
    hierarchies and relationships
  pages:
    - type: link
      title: Parents, Children, and Other Relationships in ROR
      url: >-
        https://ror.org/blog/2023-02-27-parents-children-and-other-relationships-in-ror/
---
> 👍 ROR Schema v2.1
>
> This page documents relationships in ROR metadata schema v2.1 For documentation of relationships in ROR metadata schema v1, see [\<https://ror.readme.io/v1/docs/relationships>](https://ror.readme.io/v1/docs/relationships). You can also read more about ROR [schema versions](doc:schema-versions) and a summary of what's new in [Schema 2.0](doc:schema-v2) and [Schema 2.1](doc:schema-2-1).

# How ROR handles relationships

ROR records store both structural and temporal connections in the `relationships` field with the values "Parent", "Child", "Related", "Successor", and "Predecessor", allowing systems to understand and display organizational hierarchies and connections. The `relationships` element is an array, so organizations can have multiple children, multiple parents, and multiple lateral relationships.

If an organization ceases operations and passes on its work to another organization, that relationship is also reflected in ROR through the "Predecessor" and "Successor" relationship types. See [ROR data structure: relationships](doc:ror-data-structure#relationships) for a full explanation.

# Examples

The US [National Cancer Institute](https://ror.org/040gcmg81) is a child organization of the National Institutes of Health (NIH), and it also has multiple child organizations of its own and multiple related organizations.

```json
{
   "admin" : {
      "created" : {
         "date" : "2018-11-14",
         "schema_version" : "1.0"
      },
      "last_modified" : {
         "date" : "2024-01-10",
         "schema_version" : "2.0"
      }
   },
   "domains" : [],
   "established" : 1937,
   "external_ids" : [
      {
         "all" : [
            "100000054",
            "100008637",
            "100007316"
         ],
         "preferred" : "100000054",
         "type" : "fundref"
      },
      {
         "all" : [
            "grid.48336.3a"
         ],
         "preferred" : "grid.48336.3a",
         "type" : "grid"
      },
      {
         "all" : [
            "0000 0004 1936 8075"
         ],
         "preferred" : null,
         "type" : "isni"
      },
      {
         "all" : [
            "Q664846"
         ],
         "preferred" : null,
         "type" : "wikidata"
      }
   ],
   "id" : "https://ror.org/040gcmg81",
   "links" : [
      {
         "type" : "website",
         "value" : "http://www.cancer.gov/"
      },
      {
         "type" : "wikipedia",
         "value" : "https://en.wikipedia.org/wiki/National_Cancer_Institute"
      }
   ],
   "locations" : [
      {
         "geonames_details" : {
            "country_code" : "US",
            "country_name" : "United States",
            "lat" : 38.98067,
            "lng" : -77.10026,
            "name" : "Bethesda"
         },
         "geonames_id" : 4348599
      }
   ],
   "names" : [
      {
         "lang" : "fr",
         "types" : [
            "label"
         ],
         "value" : "Institut National du Cancer"
      },
      {
         "lang" : "es",
         "types" : [
            "label"
         ],
         "value" : "Instituto Nacional del Cáncer"
      },
      {
         "lang" : null,
         "types" : [
            "acronym"
         ],
         "value" : "NCI"
      },
      {
         "lang" : null,
         "types" : [
            "ror_display",
            "label"
         ],
         "value" : "National Cancer Institute"
      }
   ],
   "relationships" : [
      {
         "id" : "https://ror.org/05bjen692",
         "label" : "Center for Cancer Research",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/03v6m3209",
         "label" : "Frederick National Laboratory for Cancer Research",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/05n6zrm60",
         "label" : "SWOG Cancer Research Network",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/00vkwep27",
         "label" : "Division of Cancer Epidemiology and Genetics",
         "type" : "child"
      },
      {
         "id" : "https://ror.org/01cwqze88",
         "label" : "National Institutes of Health",
         "type" : "parent"
      },
      {
         "id" : "https://ror.org/00w52vt71",
         "label" : "MUSC Hollings Cancer Center",
         "type" : "related"
      },
      {
         "id" : "https://ror.org/02qyzaf42",
         "label" : "The Cancer Imaging Archive",
         "type" : "related"
      }
   ],
   "status" : "active",
   "types" : [
      "government"
   ]
}
```

In the [ROR search interface](https://ror.org/search), the number of relationships (if any) is shown on each record in the list view, and clicking the "View Details" button will take the user to the individual record, where all relationships are displayed.

<Image align="center" border={true} src="https://files.readme.io/4e6a01d-Screenshot_2024-04-03_at_1.50.11_PM.png" className="border" />

Pictured below is an image of an organizational "family tree" created from ROR records with an [organization tree script](https://github.com/ror-community/ror-utilities/tree/main/organization-tree-scripts). The United States Department of Energy is the top node and has many "children" and "grandchildren" and one "great-grandchild" organization. (Laterally related organizations are not shown in this view.)

<Image align="center" alt={1050} border={false} caption="List of University of California system children and grandchildren expressed as nodes in an indented list from the ROR organization tree script written by Sandra Mierz." title="ror-doe-hierarchy.png" src="https://files.readme.io/be45520-ror-doe-hierarchy.png" />
