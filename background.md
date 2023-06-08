# Background and Requirements

## Terminology

This document uses the following terminology:

**Source Template:** A schematized template for how to compose a bibliography entry, footnote, or other type of citation.  This corresponds to what Evidence Explained calls a “QuickCheck Model”.

**Master Source:** An entire source.  In GEDCOM this is a `SOURCE_RECORD`.

**Source Details:** The part of a source relevant to a specific fact, such as a specific page or entry.  In GEDCOM this is a `SOURCE_CITATION` that points to the source record.

**Citation Element:** As defined in [Citation Elements: Concepts (fhiso.org)](https://fhiso.org/TR/cev-concepts), a logically self-contained piece of information in a citation layer that might reasonably be included in a formatted citation.  RootsMagic calls this a `FIELD` structure in GEDCOM.

**Formatted Citation:** As defined in [Citation Elements: Concepts (fhiso.org)](https://fhiso.org/TR/cev-concepts), a citation that has been rendered into human-readable form, typically as a sentence or short paragraph that might be used as a footnote, endnote, tablenote or bibliography entry. There is no single standard on the correct form of formatted citations; many different style guides exist, each giving their own rules on how to construct a formatted citation.

## Requirements

[Citations: Goals and Considerations (fhiso.org)](https://fhiso.org/TR/citation-goals) lists some things
that a data model should support:

- **Display:** Some operations on citations require a formatted presentation that can be displayed to the user and printed out.
- **Process:** Some operations on citations require knowledge of the underlying meaning of the citations: notably, searching, sorting, validating, and de-duplicating.
- **Generate:** Some operations on citations require the ability to generate formatted presentation from the underlying meaning: notably, report generation, changing style guides, and merging citation sets presented in different styles.
- **Provenance:** One kind of layering is derivation: a source can be a copy, translation, transcript, abridgement, etc. of another source. The other kind of layering is transition of stewardship: sources can be held by stewards who obtained the source from other stewards. Not all family history citation styles express both kinds of layering, but enough do that both should be modelled.
- **Shorten:** Some style guides recommend that a citation have as many as four different levels of verbosity:
  - A full citation with provenance information.
  - A full citation of just the most important layer.
  - An abbreviated citation of just the most important items and properties of the most important layer.
  - A citation reference with just enough information to locate the citation in a related works section.

  Not all styles use all four of these, but enough use each that tools to generate briefer citations from fuller data have been requested.

- **Customise:** Some operations on citations require the ability to let users specify parts of the presentation without modifying the underlying meaning: notably, when users have stylistic opinions that have not be fully captured by generation algorithms.

We add additional or refine existing requirements as follows:

1. Support conformance to *Evidence Explained* (EE) style guidelines.
2. Support conversion between different style guides for different purposes.  For example, a user may want to view or generate a report using source citations in EE style, while the same or a different user may want to view or generate a report using source citations in a different style such as Register style, after importing the same GEDCOM file.  Thus, the data model serialization in GEDCOM must support such agility.
3. The Display, Generate, and Shorten requirements above thus apply to the EE format as well as other style formats, as chosen by the user.
4. The GEDCOM extension must be backwards compatible in the sense that a program that does not support this extension must still see sources as GEDCOM 7.0 compliant sources.
5. Ability to support multiple layers of containment (e.g., a specific article, in a specific volume, in a specific series, found in a specific repository). 
6. Permit templates for many source types (ideally everything in *Evidence Explained*).
7. Custom source templates must be permitted.

## Gaps in FamilySearch GEDCOM 7.0 (“Absent”)

Currently in [FamilySearch GEDCOM 7.0](https://gedcom.io/specifications/FamilySearchGEDCOMv7.html):

- `<<SOURCE_RECORD>>` only supports one `PUBL` text string containing “information such as the city of publication, name of the publisher, and year of publication” for example.  However, the exact format of that information varies by style format.  As such, FamilySearch GEDCOM 7.0 currently only supports one style format per source.  If sources from different users are merged into the same GEDCOM file, there is thus no consistency of format.
- `<<SOURCE_RECORD>>` does not have a way to indicate containment in another source, only containment in a repository.
- `<<SOURCE_CITATION>>` only supports one `PAGE` text string containing “where in the source the relevant material can be found”, or (with a `voidPtr` source reference) “the entire source”.  However, the exact format of that information varies by style format.  As such, FamilySearch GEDCOM 7.0 currently only supports one style format per source citation.  If source citations from different users are merged into the same GEDCOM file, there is thus no consistency of format.
