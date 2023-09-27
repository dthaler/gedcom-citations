# Source Derivation GEDCOM Extension (Proposed)

FHISO's [Citations: Goals and Considerations](https://fhiso.org/TR/citation-goals) lists some things
that a data model should support, including provenance.  It notes:

> One kind of layering is derivation: a source can be a copy, translation, transcript,
> abridgement, etc. of another source. The other kind of layering is transition of stewardship:
> sources can be held by stewards who obtained the source from other stewards. Not all family history
> citation styles express both kinds of layering, but enough do that both should be modelled.

The proposed Source Derivation GEDCOM Extension allows recording that a
source was derived in some way from another source.

See also the [`_CITE` YAML file](yaml/_CITE.yaml) for more details.

Open Questions:

1. Is `PAGE` the correct superstructure?
2. Should we have a `TYPE` enumeration for type of derivation
   (copy, translation, transcript, abridgement, etc.)?

## GEDCOM Schema

```
n SOUR @<XREF:SOUR>@                    {1:1}  g7:SOUR
   ...
  +1 PAGE <Text>                        {0:1}  g7:PAGE
      ...
     +2 _CITE                           {0:1}  ext:_CITE
        +3 <<SOURCE_CITATION>>          {1:M}
```

### `_CITE` (Citation)  `ext:_CITE`

A set of source citations for a cited source.

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
3 _CITE
4 SOUR @S2@
5 PAGE 214
0 @S1@ SOUR
1 AUTH Joseph M. Lalley Jr.
1 TITL Baptismal Records, Wilmington, Delaware
2 PUBL http://www.lalley.com : accessed 28 January 2007
0 @S2@ SOUR
1 TITL St. Paul Church Baptisms, December 1869-August 1893
```
