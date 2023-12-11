# Source Stewardship GEDCOM Extension (Proposed)

FHISO's [Citations: Goals and Considerations](https://fhiso.org/TR/citation-goals) lists some things
that a data model should support, including provenance.  It notes:

> One kind of layering is derivation: a source can be a copy, translation, transcript,
> abridgement, etc. of another source. The other kind of layering is transition of stewardship:
> sources can be held by stewards who obtained the source from other stewards. Not all family history
> citation styles express both kinds of layering, but enough do that both should be modelled.

The proposed Source Stewardship GEDCOM Extension allows recording the
transition of stewardship of a given source by permitting a date (range)
per source repository citation.  For example, stewardship of a Family
Bible can be recorded using a series of source repository citations,
each with a date.

See also the [`_DATE` YAML file](yaml/_DATE.yaml) for more details.

**Note**: the `_DATE` YAML file is an anomaly here since `_DATE` is a
[*relocated standard structure*](https://github.com/FamilySearch/GEDCOM/blob/main/specification/gedcom-1-hierarchical-container-format.md#extensions)
which means it reuses the `g7:DATE` URI, which has its own
[YAML file](https://github.com/FamilySearch/GEDCOM-registries/blob/main/structure/standard/DATE.yaml) already.
There is an [issue](https://github.com/FamilySearch/GEDCOM.io/issues/97) tracking
possible ways to allow automated validation of relocated standard structures,
such as a wrapper YAML file.  In that light, an example YAML file is provided here that
may be used to help construct such a proposal.

## GEDCOM Schema

```gedstruct
n REPO @<XREF:REPO>@                       {1:1}  g7:REPO
  +1 <<NOTE_STRUCTURE>>                    {0:M}
  +1 CALN <Special>                        {0:M}  g7:CALN
     +2 MEDI <Enum>                        {0:1}  g7:MEDI
        +3 PHRASE <Text>                   {0:1}  g7:PHRASE
  +1 _DATE <DateValue>                     {0:1}  g7:DATE
     +1 TIME <Time>                        {0:1}  g7:TIME
     +1 PHRASE <Text>                      {0:1}  g7:PHRASE
```

### `_DATE` (DATE)  `g7:DATE`

The date or date range when a source was held by a specific repository.

### Example 1

The following example is taken from the "Private Holdings: Family Bible
Records" QuickCheck Model on page 107 of "Evidence Explained", which has a first
(full) reference note of:

1. John Lynde Greene Sr. Family Bible Records, 1807-1944, *The Holy
   Bible* (Philadelphia: Matthew Carey, 1811), "Marriages"; privately
   held by John Lynde Greene IV, Germantown, Maryland, 2001.

This example just has a year when the private holding was known to be
in the stated repository.

```
0 @I1@ INDI
1 SOUR @S1@
2 PAGE Marriages
0 @S1@ SOUR
1 AUTH John Lynde Greene Sr. Family Bible Records, 1807-1944
1 TITL The Holy Bible
1 PUBL Philadelphia: Matthew Carey, 1811
1 REPO @R1@
2 _DATE 2001
0 @R1@ REPO
1 NAME John Lynde Greene IV
2 ADDR Germantown, Maryland
3 CITY Germantown
4 STAE Maryland

```

### Example 2

The following example gives the stewardship of a Family Bible
that was passed between private holders over time.

```
0 @S1@ SOUR
1 TITL Adam Smith Family Bible
1 REPO @R1@
2 _DATE TO 1950
1 REPO @R2@
2 _DATE FROM 1950
0 @R1@ REPO
1 NAME Adam Smith
0 @R2@ REPO
1 NAME John Smith
```

### Example 3

The following example is taken from the "Journal Articles: Online Archives of Print
Journals" QuickCheck Model on page 780 of "Evidence Explained", which has a first
(full) reference note of:

1. Deborah A. Rosen, "Women and Property across Colonial America: A Comparison of Legal
Systems in New Mexico and New York," *The William and Mary Quarterly* 91 (April 2003); online archives,
*The History Cooperative* (http://www.historycooperative.org/journals/wm/60.2/rosen.html :
accessed 25 April 2007), para. 21.

This example has an "accessed" date when the information was known to be
on a given website.

```
0 @I1@ INDI
1 SOUR @S1@
2 PAGE para. 21.
0 @S1@ SOUR
1 AUTH Deborah A. Rosen
1 TITL "Women and Property across Colonial America: A Comparison of Legal Systems in New Mexico and New York"
1 PUBL The William and Mary Quarterly 91 (April 2003)
1 REPO @R1@
2 CALN http://www.historycooperative.org/journals/wm/60.2/rosen.html
2 _DATE 25 APR 2007
0 @R1@ REPO
1 NAME The History Cooperative
1 WWW http://www.historycooperative.org
```
