# gedcom-citations

This repository contains proposed [FamilySearch GEDCOM](https://gedcom.io/specs/) extensions
for reliably capturing and exchanging EE-style citations, Chicago-style, citations, etc.

This repository is intended to contain work in progress by the Citations project team.
For more information, see [Project Teams](https://gedcom.io/community/#project-teams). 

* [Background and requirements](background.md)

Extensions
----------

The following extensions are proposed for discussion:

* (to be added)

All proposed extensions use [documented extension tags](https://gedcom.io/specifications/FamilySearchGEDCOMv7.html#extension-tags).  If they later become incorporated into the FamilySearch GEDCOM standard,
standard tags will be defined at that time.

Rather than write out URIs in full, we use prefix notation: any URI beginning with one
of the following short prefixes followed by a colon is shorthand for a URI beginning
with the corresponding URI prefix

| Short Prefix | URI Prefix                                     |
|:-------------|:-----------------------------------------------|
| `g7`         | `https://gedcom.io/terms/v7/`                  |
| `ext`        | `https://github.com/dthaler/gedcom-citations/` |
