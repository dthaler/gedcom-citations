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
