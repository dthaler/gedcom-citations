# Source Provenance GEDCOM Extension

FHISO's [Citations: Goals and Considerations](https://fhiso.org/TR/citation-goals) lists some things
that a data model should support, including provenance.  It notes:

> One kind of layering is derivation: a source can be a copy, translation, transcript,
> abridgement, etc. of another source. The other kind of layering is transition of stewardship:
> sources can be held by stewards who obtained the source from other stewards. Not all family history
> citation styles express both kinds of layering, but enough do that both should be modelled.

## GEDCOM Schema


```
n SOUR @<XREF:SOUR>@                    {1:1}  g7:SOUR
   ...
  +1 PAGE <Text>                        {0:1}  g7:PAGE
      ...
     +2 _CITE                           {0:1}  g7:CITE
        +3 <<SOURCE_CITATION>>          {1:M}
```

### `_CITE` (Citation)  `g7:CITE`

A source citation for a cited source.
