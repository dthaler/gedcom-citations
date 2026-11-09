# Source Templates Extension Rationale

This document contains the survey and requirements motivating the [Source Templates Extension](templates-extension.md).

## Use of Property Sets and Templates

[Citations: Goals and Considerations (fhiso.org)](https://web.archive.org/web/20230626185821/https://fhiso.org/TR/citation-goals) says:

> Property set design requires a trade-off between complexity and expressiveness. Academic
> property set citation standards typically have between 25 and 100 defined property names,
> with some sort of catch-all "miscellaneous" name to handle citations with properties outside
> that set. FHISO representatives have spoken with teams who attempted extending this model to
> family history citations who reported requiring hundreds more names to cover source types
> like rows in a tabular census and inscriptions on a grave marker. When they attempted to add
> in names for common provenance relationships, the number of names quickly passed a thousand
> and the projects were abandoned as leading to an unusable end.

The present proposal starts from the assumption that the "unusable end" conclusion is incorrect.
Indeed, we will show in subsections below a number of existence proofs that show it is actually common practice.

The present proposal does not try to create a set of standard property names usable across
templates, but rather leaves it up to each template to specify the property names for that
template.  There is not necessarily any expectation that two templates use the same property
name to refer to a common data element, nor that the same property name doesn't have different
meanings in different templates.

[Citations: Goals and Considerations (fhiso.org)](https://web.archive.org/web/20230626185821/https://fhiso.org/TR/citation-goals) also says
that the "Property Sets and Templates" category of solutions does not support the Provenance or
the Customise requirements.  However, the present proposal does support the Customise requirement.
And the Provenance requirement is not precluded but will be covered in a separate extension
that can easily be used with or without this extension.

### General Precedents ("Valuable")

It is common in general (not specific to genealogy) library and citation tools and APIs to support
citations in a variety of formats using a common set of data, where the citations are composed
dynamically from a set of stored citation elements, rather than stored fully in each format separately.

A few examples include:

- EasyBib (<http://www.easybib.com/cite/form>) has a web entry form and can generate citations in a variety of citation formats, including APA, Chicago, MLA, Turabian, etc.
- EndNote (<https://clarivate.com/webofsciencegroup/support/endnote/>) manages source and can generate citations in a variety of formats (see for example position 6:00 in the Windows video), including APA, Chicago, Turabian, etc.
- The OCLC Citations API (<https://developer.api.oclc.org/citations-api>) can access the WorldCat database and generate citations in a variety of formats (see CitationStyle), including APA, Chicago, MLA, Turabian, etc.

[Citations: Goals and Considerations (fhiso.org)](https://web.archive.org/web/20230626185821/https://fhiso.org/TR/citation-goals) also explains:

> *Property sets* and *templates* are used by BibTeX, CiteProc, CSL, EndNote, Zotero, and other
> academic document preparation toolchains. *Citation data* is provided in this format by most
> academic-oriented online repositories and archives. *Templates* in this format are offered by
> most academic publication venues.

A [comparison of reference management software](https://en.wikipedia.org/wiki/Comparison_of_reference_management_software) 
shows that many other widely-used software implementations support multiple citation styles
(APA, Chicago, MLA, etc.) using templates such as BibTeX style files or CSL files, and the ability
to import references from databases such as OCLC, Google Scholar, and ProQuest.

### GEDCOM Precedents ("Used")

[Citations and the GEDCOM (evidentiasoftware.com)](https://evidentiasoftware.com/citations-and-the-gedccom/) and [Easier Source Citation With Your Genealogy Software (familytreemagazine.com)](https://www.geneamusings.com/2011/02/peeking-at-rootsmagic-4-source.html)
discuss a number of popular genealogy applications that appear to use source templates, including
Family Tree Maker, RootsMagic, and Legacy.  Source templates are also used by various other
FamilySearch-certified applications, including Family Historian and MagiKey Family Tree.

#### RootsMagic 6

As noted in [Citations and the GEDCOM (evidentiasoftware.com)](https://evidentiasoftware.com/citations-and-the-gedccom/) and
[Genea-Musings: Peeking at RootsMagic 4 Source Citations in a GEDCOM File - Post 1 (geneamusings.com)](https://www.geneamusings.com/2011/02/peeking-at-rootsmagic-4-source.html),
RootsMagic uses a `_TMPLT` structure within a source record that contains citation element names
and values.  For example:

```gedcom
0 @S2@ SOUR
1 _TMPLT
2 TID 43
2 FIELD
3 NAME Country
2 FIELD
3 NAME CensusID
3 VALUE 1930 U.S. Census
```

It also uses a `_BIBL` structure to contain the formatted citation for a bibliography entry.

```gedcom
1 _BIBL Virginia. Warwick County. 1930 U.S. Census, population schedule. Digita
2 CONC l images. Ancestry.com. https://www.ancestry.com : .
```

As noted in the links above, one could use the citation elements to reconstruct the
citation if you know the pattern, but RootsMagic does not expose the patterns.  And
the `TID` structure contains a template ID that appears to be meaningful only to RootsMagic.

[Source Templates - RootsMagic Wiki](http://wiki.rootsmagic.com/wiki/RootsMagic_8:Source_Templates)
shows that RootsMagic 8 continues to use source templates, and
[Sentence Template Language - RootsMagic Wiki](http://wiki.rootsmagic.com/wiki/RootsMagic_8:Sentence_Template_Language)
shows the syntax of patterns it uses to construct formatted citations.  For example:

```
[Author:Reverse]. <i>[Title]</i>. [PubPlace]: [Publisher], [PubDate:Year].
```
where `[Name]` indicates variable expansion, potentially with a modifier specific to that variable
(like "Reverse" and "Year" above), and limited HTML-based markup is permitted using
`<i>`, `<b>`, `<u>`, `<sc>`, `<sup>`, and `<sub>`.

It does support conditional expansion, as noted in the links above:

> In the following source templates examples:
> `<privately held by [LastKnownOwner], >`
> would only write something if "[LastKnownOwner]" has a value, or in this example:
> `<[Format],|digital image,>`
> would write "database and digital images," if that's what you entered into the [Format] field,
> or it would write "digital image," if you didn't enter anything into the [Format] field.

And it furthermore explains:

> A value switch is similar to a simple switch except that it allows you to check for a value without actually writing that value. It is indicated by a "?".

> `<?[Expression]| Show this if True.>`

> `<?[Expression]| Show this if True. | Show this if False.>`

And escapes are supported as follows:

> If you ever want to write an actual <, >, /, [, or ] in your sentence, you must precede it by a "/".

While the above patterns are powerful, readable, and short, the use of / as an escape character is surprising, and the use of <> can be confusing due to use of HTML.  And of course the syntax is proprietary, not based on any open standard or language or open source de facto standard.

#### Family Historian 7

As discussed in Family Historian's [Sources and Source Templates](https://www.family-historian.co.uk/help/fh7/sourcesandsourcetemplates.html)
and [Source Template Formats](https://www.family-historian.co.uk/help/fh7/sourcetemplateformats.html) documents,
Family Historian 7 supports creating sources from source templates, and appears to allow users to define their own
as well.  Like RootsMagic, the syntax is proprietary, not based on any open standard or language
or open source de facto standard.

As covered in the last document above, the syntax of patterns it uses to construct formatted citations uses
`{}` around field names, such as:

```
Manuscript Record Document: {Collection} - {Document_Title}
```

where "{Collection}" and "{Document_Title}" refer to "Collection" and "Document Title" fields.  It distinguishes
between "citation-specific field codes" that are not valid in the context of bibliography entries for example,
and other fields. Thus, "citation-specific field codes" appear to apply to a GEDCOM `SOURCE_CITATION` rather than
a `SOURCE_RECORD`.

Family Historian 7 allows a limited set of HTML codes in templates as well, including `<i>`, `<b>`, `<u>`, and
`<caps>`, and uses `\` as an escape to allow literal characters like `\<`.

It supports a set of "field code qualifiers" that map to conceptual functions on a single field, such as
`:YEAR` which extracts the year from a field containing a date.  It also supposed a set of explicit functions,
using a syntax like `=ToUpper("McKinley")` with a number of arguments defined by the function.

It also supports a set of expressions, such as:

- `<({code1}, {code2} and {code3})>` where the `(` prefix and `)` suffix are output if any of the codes are
  non-empty and the `, ` and ` and ` separators are displayed if any expression to the left is non-empty and
  the one immediately to the right is non-empty, so you get `(apple, pear)` if code1=apple, code2=pear, and code3 is
  blank.
- nested angle-bracket expressions
- `<{code1}|EMPTY!>` to display "EMPTY!" if code1 is blank
- `<{code1}|{code2}|{code3}>` to display alternatives if a code is blank, with a variable number of alternatives
  allowed.

In top level (not within angle brackets) expressions, some characters are treated specially, including
`.`, `;`, `,`, `:`, `-`, and ` `, where they are only output under specific conditions.

It also allows [data references](https://www.family-historian.co.uk/help/fh7/understandingdatareferences.html)
that can be used to navigate the equivalent of GEDCOM.  For example, in a data reference for an individual (not
a source), `%INDI.FAMC[2]>HUSB>NAME%` would get the father's name in the second family the individual is a child in,
so presumably something like `%SOUR.REPO[2]>CALN%` would get the source's call number in the second repository
associated with the source record.

As noted in [Export Gedcom File Dialog](https://www.family-historian.co.uk/help/fh7/exportgedcomfile.html),
source templates can be exported as GEDCOM records using a GEDCOM extension.  The source templates themselves
are not present in GEDCOM files, only the fields defined in a template, which are under a `SOURCE_CITATION` or
a `SOURCE_RECORD`.  For example, citation-specific fields appear as:

```gedcom
2 SOUR @S4@
3 _FIELD TX-PAGE
4 TEXT 35
```

In the above example, a custom text (TX) field named "PAGE" is defined, whose value is "35".  No `PAGE`
structure is present, so citation-specific fields do not transfer to other programs that don't recognize
Family Historian's extension.

Non-citation-specific fields appear as, for example:

```gedcom
0 @S4@ SOUR
1 _FIELD TX-NAME
2 TEXT My name
1 _FIELD DT-DATE
2 DATE 2023
1 TITL My name, 2023
```

In the above example, a custom text (TX) field named "NAME" and a custom date (DT) field named "DATE" are defined,
with values of "My name" and "2023", respectively. An expression defined by the template, but not exported in
GEDCOM files, is used to generate the `TITL` so that these fields can transfer to other programs as a generic
`TITL`.

#### Family Tree Maker 2012

As shown in [Citations and the GEDCOM (evidentiasoftware.com)](https://evidentiasoftware.com/citations-and-the-gedccom/),
Family Tree Maker 2012 does not expose the patterns it uses in source templates, and
does not export the citation elements in GEDCOM.

#### Legacy 7.5

As shown in [Citations and the GEDCOM (evidentiasoftware.com)](https://evidentiasoftware.com/citations-and-the-gedccom/),
Legacy 7.5 does not expose the patterns it uses in source templates, and does not export
the citation elements in GEDCOM.

#### MagiKey Family Tree

MagiKey Family Tree uses the following structure for citation elements in source records:

```gedcom
0 @S12@ SOUR
1 _MODL template_name
2 _FIEL varname
3 TEXT value
2 _FIEL varname
3 TEXT value
2 SOUR @S13@
```

where `SOUR @S13@` indicates that `S12` is a member of collection `S13`.

And it uses the following structure in source citations in other types of records:

```gedcom
1 SOUR @S12@
2 PAGE 123
3 _FIEL varname
4 TEXT value
3 _FIEL varname
4 TEXT value
...
```

Here, no template is referenced in the source citation because the template is
specified inside the referenced source record.  Source templates are only supported
in source records, not in source citations with `SOUR @voidPtr@`.

Unlike RootsMagic where source templates are identified by an integer, MagiKey Family
Tree uses a name to identify the source template.  It does not export the patterns in
GEDCOM files, but they are in plain text files that come with the application.

Patterns use a code syntax like:

```
 if ($PubPlace != "") {
    $PubPlace
 }
 if ($PubPlace == "") {
     "No place"
 }
 ": "
 if ($Publisher != "") {
     $Publisher
 }
 if ($Publisher == "") {
     "No publisher"
 }
 ", "
 if ($Year != "") {
     $Year
 }
 if ($Year == "") {
     "No date"
 }
```

to generate HTML-formatted output, where `$Name` indicates a variable expansion.  No
user-friendly facility for creating custom templates is currently provided (though it
could be done by manually editing additional text files).

Like RootsMagic and Family Historian, MagiKey Family Tree also supports
functions, where the functions are well-known, not per template.  For example:

```
 "<i>" $html($newspapername($Newspaper, $Location)) "</i>"
```

Like RootsMagic and Family Historian, the syntax is proprietary, not based on any open standard or language
or open source de facto standard, but it is very similar to standard programming languages.
On the other hand, it is much more verbose than RootsMagic's and Family Historian's template languages.

#### GEN_DO!

According to a [post by Albert Emmerich](https://github.com/FamilySearch/GEDCOM/issues/348#issuecomment-5990573138),
[GEN_DO!](https://www.gen-do.de/) splits the `PAGE` structure into more detailed structures
in a custom `_CIT` record.  The detailed structures are then used when displaying citations.
For example:

```
0 @C1@ _CIT
1 SOUR @S1@
1 EVEN DEAT
1 PAGE_BD TrB 1845-1852
1 PAGE_KP Sterbefälle
1 PAGE_JG 1852
1 PAGE_PG 41
1 PAGE_NR 35 w:18
1 PAGE_WW https://www.archion.de/p/373e7ccbcf/
1 DATA
2 TEXT [1679] 15) Dienstgag p Dom Sept: Henrich Schomaker aus GE begrb 59 Jahr alt
1 CNOTE Archion Bild Nr. 114
1 CHAN
2 DATE 27 JAN 2026
3 TIME 00:08:50
1 UID 0199a1ec-9e38-7000-885d-5f4d86f83d53
```

where

* `PAGE_BD`: volume
* `PAGE_KP`: chapter
* `PAGE_JG`: year
* `PAGE_PG`: page number
* `PAGE_NR`: number of entry
* `PAGE_WW`: web link
* `PAGE_ZZ`: other information for page
* `CNOTE`: an inline `NOTE` attached to the citation.

(Note that the tags above are non-conformant in that they are non-standard and do not start
with an underscore, but they are what is implemented.)

Although GEN_DO! does export the citation elements in GEDCOM,
it does not expose the patterns it uses in source templates.

