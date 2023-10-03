# Source Derivation GEDCOM Extension (Proposed)

FHISO's [Citations: Goals and Considerations](https://fhiso.org/TR/citation-goals) lists some things
that a data model should support, including provenance.  It notes:

> One kind of layering is derivation: a source can be a copy, translation, transcript,
> abridgement, etc. of another source. The other kind of layering is transition of stewardship:
> sources can be held by stewards who obtained the source from other stewards. Not all family history
> citation styles express both kinds of layering, but enough do that both should be modelled.

The proposed Source Derivation GEDCOM Extension allows recording that a
source was derived in some way from another source.

See also the [`_SOUR` YAML file](yaml/_SOUR.yaml) for more details.

**Note**: the `_SOUR` YAML file is an anomaly here since `_SOUR` is a
[*relocated standard structure*](https://github.com/FamilySearch/GEDCOM/blob/main/specification/gedcom-1-hierarchical-container-format.md#extensions)
which means it reuses the `g7:SOUR` URI, which has its own
[YAML file](https://github.com/FamilySearch/GEDCOM-registries/blob/main/structure/standard/SOUR.yaml) already.
There is an [issue](https://github.com/FamilySearch/GEDCOM.io/issues/97) tracking
possible ways to allow automated validation of relocated standard structures,
such as a wrapper YAML file.  In that light, an example YAML file is provided here that
may be used to help construct such a proposal.

## GEDCOM Schema

```
n SOUR @<XREF:SOUR>@                    {1:1}  g7:SOUR
   ...
  +1 PAGE <Text>                        {0:1}  g7:PAGE
      ...
     +2 _SOUR                           {0:M}  g7:SOUR
        ...
```

### `_SOUR` (Source)  `g7:_SOUR`

This relocated standard structure allows a `<<SOURCE_CITATION>>`
to appear as a substructure of `PAGE`, to indicate a
source citation for a cited source.

### Example

The following example is taken from the "Church records database, online"
QuickCheck Model on page 320 of "Evidence Explained", which has a first
(full) reference note of:

1. Joseph M. Lalley Jr., "Baptismal Records, Wilmington, Delaware,"
   database, *19th Century Immigrant Roots: Records for Wilmington,
   Delaware, USA, and Vicinity* (http://www.lalley.com : accessed 28 January
   2007), entry for John Keely, baptized 26 August 1888; citing St. Paul
   Church Baptisms, December 1869-August 1893, p. 214.

```
0 @I1@ INDI
1 SOUR @S1@
2 PAGE entry for John Keely, baptized 26 August 1888
3 _SOUR @S2@
4 PAGE 214
0 @S1@ SOUR
1 AUTH Joseph M. Lalley Jr.
1 TITL Baptismal Records, Wilmington, Delaware
2 PUBL http://www.lalley.com : accessed 28 January 2007
0 @S2@ SOUR
1 TITL St. Paul Church Baptisms, December 1869-August 1893
```
