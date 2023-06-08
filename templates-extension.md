# Source Templates Extension

## Use of Property Sets and Templates

[Citations: Goals and Considerations (fhiso.org)](https://fhiso.org/TR/citation-goals) says:

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

[Citations: Goals and Considerations (fhiso.org)](https://fhiso.org/TR/citation-goals) also says
that the "Property Sets and Templates" category of solutions does not support the Provenance or
the Customise requirements.  However, the present proposal does support the Customise requirement.
And the Provenance requirement is not precluded but will be covered in a separate extension
that can easily be used with or without this extension.

### General Precedents ("Valuable")

It is common in general (not specific to genealogy) library and citation tools and APIs to support
citations in a variety of formats using a common set of data, where the citations are composed
dynamically from a set of stored citation elements, rather than stored fully in each format separately.

A few examples include:

- EasyBib (http://www.easybib.com/cite/form) has a web entry form and can generate citations in a variety of citation formats, including APA, Chicago, MLA, Turabian, etc.
- EndNote (https://clarivate.com/webofsciencegroup/support/endnote/) manages source and can generate citations in a variety of formats (see for example position 6:00 in the Windows video), including APA, Chicago, Turabian, etc.
- The OCLC Citations API (https://developer.api.oclc.org/citations-api) can access the WorldCat database and generate citations in a variety of formats (see CitationStyle), including APA, Chicago, MLA, Turabian, etc.

[Citations: Goals and Considerations (fhiso.org)](https://fhiso.org/TR/citation-goals) also explains:

> *Property sets* and *templates* are used by BibTeX, CiteProc, CSL, EndNote, Zotero, and other
> academic document preparation toolchains. *Citation data* is provided in this format by most
> academic-oriented online repositories and archives. *Templates* in this format are offered by
> most academic publication venues.

### GEDCOM Precedents ("Used")

[Citations and the GEDCOM (evidentiasoftware.com)](https://evidentiasoftware.com/citations-and-the-gedccom/) and [Easier Source Citation With Your Genealogy Software (familytreemagazine.com)](https://www.geneamusings.com/2011/02/peeking-at-rootsmagic-4-source.html)
discuss a number of popular genealogy applications that appear to use source templates, including
Family Tree Maker, RootsMagic, and Legacy.  Source templates are also used by various other
FamilySearch-certified applications, such as MagiKey Family Tree.

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

Like RootsMagic's use of "Reverse" and "Year" above, MagiKey Family Tree also supports
transforms using a function syntax, where the transforms are well-known, not per template.  For example:

```
 "<i>" $html($newspapername($Newspaper, $Location)) "</i>"
```

Like RootsMagic, the syntax is proprietary, not based on any open standard or language
or open source defacto standard, but it is very similar to standard programming languages.
On the other hand, it is much more verbose than RootsMagic's template language.

## Source Template GEDCOM Extension

This section proposes a GEDCOM extension for use of source templates.

### Patterns

This proposal relies on agreement on picking a pattern format.  A pattern format must
support the following requirements:

- Citation element expansion
- Output using a subset of HTML
- Conditional checks (e.g., for citation element empty and non-empty), including nested conditional checks
- Able to support a set of standard transforms
- Using patterns not only for output, but also for matching input, to aid in constructing a templated source citation from a non-templated one

And preferably:

- Has an interpreter implemented in existing open source libraries in a variety of programming languages.
- Minimal overhead.
- Safety, in that a user-defined custom pattern cannot do arbitrary things (e.g., infinite loops).
- Avoid confusion between HTML markup and other operations such as conditional checks.

Some possibilities (in addition to the custom ones in RootsMagic and MagiKey Family Tree) include more powerful programming languages such
as Perl, Tcl, Python, AWK, Lua, and Ruby. [c - Embeddable language with good string manipulation support - Stack Overflow](https://stackoverflow.com/questions/1281073/embeddable-language-with-good-string-manipulation-support)
and [Why is Perl the best choice for most string manipulation tasks? - Stack Overflow](https://stackoverflow.com/questions/1490745/why-is-perl-the-best-choice-for-most-string-manipulation-tasks) has some discussion.  However, these tend to fail the goal of safety.

Other pattern languages that allow empty/non-empty checks with parameter expansion include:

- [Bash shell parameter expansion](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html), which isn't available in a library form usable in other projects
- [XSLT](https://www.w3.org/TR/1999/REC-xslt-19991116), which is designed for transforming XML documents into other XML documents

Since there does not seem to already be general open source libraries that simply do what we want, it seems that specifying our own pattern language is the only viable option.

Using a bash-style syntax would seem to be the closest to a standard, while avoiding confusion between HTML markup and non-HTML operations.

#### Bash-style patterns

`${variable}`
:Replace with the value of the specified variable.

`${variable:-text}`
:If variable is non-empty, replace with its value. Otherwise replace with the specified text, which may itself have bash-style patterns.

`${variable:+text}`
:If variable is empty, do nothing. Otherwise replace with the specified text, which may itself have bash-style patterns.

`\$`
:Use escaped character ($) literally.

`$(function arg1 arg2)`
:Replace with the output of the specified function, passing the specified arguments (two in this example).	

### Source Template Schema

It is proposed that we store source templates in YAML files, as is done for GEDCOM tag, record, and enumset specifications at [GEDCOM/README.md at main · FamilySearch/GEDCOM · GitHub](https://github.com/FamilySearch/GEDCOM/blob/main/extracted-files/README.md)
and [GEDCOM-registries/record-INDI.yaml at main · FamilySearch/GEDCOM-registries · GitHub](https://github.com/FamilySearch/GEDCOM-registries/blob/main/standard/record-INDI.yaml).
As with the other YAML file types, source template YAML files might be standard or might be application-specific.  Either way they are identified by a URI, preferably by a URL resolvable to the source template YAML file.

The following [JSON schema](https://datatracker.ietf.org/doc/html/draft-bhutton-json-schema-01#name-meta-schemas) describes the YAML file syntax:

```json
{
  "$schema": "http://json-schema.org/draft/2020-12/schema#",
  "title": "JSON Schema for GEDCOM source YAML templates",
  "type": "object",
  "properties": {
    "lang": {
      "type": "string",
      "description": "IANA language tag of the language of this template.",
    },
    "uri": {
      "type": "string",
      "description": "The URI of this source template.",
    },
    "name": {
      "type": "string",
      "description": "Textual name which could be displayed to users in a list of source templates.",
    },
    "description": {
      "type": "string",
      "description": "Textual description which could be displayed to users in a list of source templates.",
    },
    "collection": {
      "type": "string",
      "description": "URI of the source template for the containing collection.",
    },
    "keywords": {
      "type": "array",
      "description": "A list of keywords for use, in addition to any words in the name, when filtering source templates by a search string.",
      "items": {
        "type": "string"
      }
    },
    "sfields": {
      "type": "array",
      "description": "Array of citation elements for a master source.",
      "items": {
        "type": "object",
        "properties": {
          "variable": {
            "type": "string",
            "description": "Machine-readable variable name of the citation element.",
          },
          "label": {
            "type": "string",
            "description": "Human-readable label associated with the citation element.",
          },
          "required": {
            "type": "boolean",
            "description": "Whether the citation element is required to have a  non-empty value.",
          },
        },
        "required": ["variable", "label"],
        "additionalProperties": false,
      }
    },
    "dfields": {
      "type": "array",
      "description": "Array of citation elements for source details.",
      "items": {
        "type": "object",
        "properties": {
          "variable": {
            "type": "string",
            "description": "Machine-readable variable name of the citation element.",
          },
          "label": {
            "type": "string",
            "description": "Human-readable label associated with the citation element.",
          },
          "required": {
            "type": "boolean",
            "description": "Whether the citation element is required to have a  non-empty value.",
          },
        },
        "required": ["variable", "label"],
        "additionalProperties": false,
      }
    },
    "repository": {
      "type": "boolean",
      "description": "Whether this type of source would normally require listing whose possession the source is in (i.e., the repository). This should be true for unique items such as a family bible, diary, or letter. Defaults to false if not present.",
    },
    "quality": {
      "type": "number",
      "description": "An enumerated value from set g7:enumset-QUAY indicating the credibility of the type of source as a whole. For example, a birth certificate might be Primary, whereas a family bible might be Secondary (since the information may have been recorded at a later time). If this structure is not present, it is the same as if Questionable (1) had been specified. Note: The structures for representing the strength of and confidence in various claims are known to be inadequate and are likely to change in a future version of this specification.",
    },
    "formats": {
      "type": "array",
      "description": "Array of one or more formats of citations",
      "items": {
        "type": "object",
        "properties": {
          "name": {
            "type": "string",
            "description": "The name of the citation format.",
          },
          "abbr": {
            "type": "string",
            "description": "Pattern that specifies how to construct an abbreviated title for the master source. The abbreviated title constructed can be displayed in a source list when the source record does not have an ABBR structure. (An implementation might store the abbreviated title constructed in an ABBR structure, but this is not required.) The payload must not contain variables defined in the dfields array, but may contain variables defined in the sfields array, and if a collection is specified then it may also use the variable \"CollectionAbbr\" which will expand to the collection's abbreviated title.",
          },
          "auth": {
            "type": "string",
            "description": "Pattern that specifies how to construct an AUTH payload for the master source. The payload must not contain variables defined in the dfields array but may contain variables defined in the sfields array.",
          },
          "titl": {
            "type": "string",
            "description": "Pattern that specifies how to construct a TITL payload. The payload must not contain variables defined in the dfields array but may contain variables defined in the sfields array.",
          },
          "publ": {
            "type": "string",
            "description": "Pattern that specifies how to construct a PUBL payload for the master source. The string must not contain variables defined in the dfields array but may contain variables defined in the sfields array.",
          },
          "page": {
            "type": "string",
            "description": "Pattern that specifies how to construct a PAGE payload. The payload may contain variables defined in the dfields array or the sfields array.",
          },
          "caln": {
            "type": "string",
            "description": "Pattern that specifies how to construct a CALN payload. The string must not contain variables defined in the dfields array but may contain variables defined in the sfields array.",
          },
          "scite": {
            "type": "string",
            "description": "Pattern that specifies how to construct an (HTML-format) citation string (known in EE as the \"First (Full) Reference Note\" in a QuickCheck Model), using only master source citation elements. That is, it must not contain any citation elements that would not appear in the master source (shown in EE's QuickCheck Models as the \"Source List Entry\"). The pattern must not contain variables defined in the dfields array but may contain variables defined in the sfields array.",
          },
          "dcite": {
            "type": "string",
            "description": "A pattern that specifies how to construct the rest of the (HTML-format) citation string (known in EE as the \"First (Full) Reference Note\" in a QuickCheck Model), and is appended to the string constructed with the scite pattern. If no dcite string is specified, it defaults to \", \" followed by the PAGE payload.",
          },
          "alttitl": {
            "type": "array",
            "description": "Patterns that specify an alternate way to construct a TITL payload for the master source, used only for parsing purposes when converting a FamilySearch GEDCOM 7.0 source to one using a source template. The pattern must not contain conditions or functions, and must not contain variables defined in the dfields array, but may contain variables defined in the sfields array. Any alttitl patterns are tried in order before trying the titl pattern.",
            "items": {
              "type": "string"
            }
          },
          "altpubl": {
            "type": "array",
            "description": "Patterns that specify an alternate way to construct a PUBL payload for the master source, used only for parsing purposes when converting a FamilySearch GEDCOM 7.0 source to one using a source template. The pattern must not contain conditions or functions, and must not contain variables defined in the dfields array, but may contain variables defined in the sfields array. Any altpubl patterns are tried in order before trying the publ pattern.",
            "items": {
              "type": "string"
            }
          },
          "altpage": {
            "type": "array",
            "description": "Patterns that specify an alternate way to construct a PAGE payload for the master source, used only for parsing purposes when converting a FamilySearch GEDCOM 7.0 source to one using a source template. The pattern must not contain conditions or functions, but may contain variables defined in the dfields array or the sfields array.  Any altpage patterns are tried in order before trying the page pattern.",
            "items": {
              "type": "string"
            }
          },
        },
        "required": ["name", "titl", "scite"],
        "additionalProperties": false,
      }
    }
  },
  "required": ["lang", "uri", "name", "description", "sfields", "formats"],
  "additionalProperties": false
}
```

### GEDCOM Schema

`CITATION_ELEMENT` :=
```gedstruct
   n _FIEL <Text>     {1:1}  ext:_FIEL
     +1 TEXT <Text>   {1:1}  ext:_FIEL-TEXT
```

Indicates that a specific citation element has a given text value.

**`_FIEL` (Field Name)**  `<ext:_FIEL>`

The property name of a citation element.

**`TEXT` (Field Value)**  `<ext:_FIEL-TEXT>`

The property value of a citation element.

`SOURCE_MODEL_REFERENCE` :=
```gedstruct
   n _TPLT <ShortURI>        {1:1}  ext:_TPLT
     +1 <<CITATION_ELEMENT>> {0:M}
     +1 SOUR @<XREF:SOUR>@   {0:1}  ext:_TPLT-SOUR
```

Indicates a reference to a specific source template.  Any citation element substructures contain fields specified by that source template.  A `SOUR` substructure must be present if and only if the source template indicates a collection source template.  If a `SOUR` substructure is present, it indicates the source record is the containing connection.

**`_TPLT` (Template Name)**  `<ext:_TPLT>`

The ShortURI of a source template.  It is recommended that the URI be resolvable to a YAML file containing the source template.

**`SOUR` (Collection)**  `<ext:_TPLT-SOUR>`

A source record for the collection in which the containing record is present.

And the following additions to the existing `SOURCE_RECORD` and `SOURCE_CITATION` definitions:

`SOURCE_RECORD` :=
```gedstruct
n @XREF:SOUR@ SOUR                      {1:1}  g7:record-SOUR
  ...
  +1 <<SOURCE_MODEL_REFERENCE>>         {0:1}
```

`SOURCE_CITATION` :=
```
n SOUR @<XREF:SOUR>@                    {1:1}  g7:SOUR
   ...
  +1 PAGE <Text>                        {0:1}  g7:PAGE
      ...
     +2 <<CITATION_ELEMENT>>            {0:M}
```

### Examples

#### Grave Marker Template

The Grave Marker example is taken from the example at *Evidence Explained*, pages 213-214.

##### YAML

```yaml
%YAML 1.2
---
lang: en-US
uri: https://gedcom.io/templates/GraveMarker
name: Grave Marker
description: The Grave Marker model is used for tombstones and other types of
  grave markers (see "Evidence Explained", pages 213-214).  It is not used for
  digital images found online; for those use the Website model instead.
keywords:
  - tombstone
  - cemetery
  - headstone
sfields:
  - variable: Cemetery
    label: Cemetery
    required: true
  - variable: Location
    label: Location
dfields:
  - variable: Section
    label: Section, Lot, or Row
formats:
  - name: Evidence Explained
    titl: ${Cemetery}
    publ: ${Location}
    scite: ${Cemetery}${Location:+ (${Location})}
    page: ${Section}
```

##### Example GEDCOM

```gedcom
0 @I1@ INDI
1 SOUR @C20004@
2 PAGE Samuel Witter marker
3 _FIEL Section
4 TEXT Samuel Witter marker
0 @C20004@ SOUR
1 TITL Brian Cemetery
1 _TPLT https://gedcom.io/templates/GraveMarker
2 _FIEL Cemetery
3 TEXT Brian Cemetery
2 _FIEL Location
3 TEXT Lawrence County, Illinois
1 PUBL Lawrence County, Illinois
```

##### Example Formatted Citations

**Source List Entry**

Brian Cemetery (Lawrence County, Illinois).

**First Reference Note**

Brian Cemetery (Lawrence County, Illinois), Samuel Witter marker.

#### Tax Record Templates

The Tax Roll and Tax Volume examples are taken from the microfilm examples at *Evidence Explained*, page 531.

##### Tax Roll YAML

```yaml
%YAML 1.2
---
lang: en-US
uri: https://gedcom.io/templates/TaxRoll
name: Tax Roll
description: The Tax Roll (Collection) model is used for a series of tax records
  covering a range of years, such as on a microfilm (see "Evidence Explained",
  pages 531-532).
keywords:
  - assessment
  - run
sfields:
  - variable: Jurisdiction
    label: Jurisdiction
    required: true
  - variable: Series
    label: Series
    required: true
formats:
  - name: Evidence Explained
    titl: ${Jurisdiction}, ${Series}
    publ: ${Location}
    scite: ${Jurisdiction}, ${Series}
    page: ${Section}
repository: true
quality: 3
```

##### Tax Volume YAML

```yaml
%YAML 1.2
---
lang: en-US
uri: https://gedcom.io/templates/TaxVolume
name: Tax Volume
description: The Tax Roll in Series model is used for tax records found within a
  source covering a run of years, such as on a microfilm (see "Evidence
  Explained", pages 530-531). For a single tax roll book with its own title, use
  the Tax Roll Book model instead.
collection: https://gedcom.io/templates/TaxRoll
keywords:
  - assessment
sfields:
  - variable: Volume
    label: Specific Volume
    required: true
dfields:
  - variable: Section
    label: Section (if any)
  - variable: Page
    label: Page
  - variable: Item
    label: Item of Interest
formats:
  - name: Evidence Explained
    titl: ${Jurisdiction}, ${Volume}
    scite: ${Jurisdiction}, ${Volume}
    page: |
      ${Section:+${Section}${Page:+, }}${Page:+p. ${Page}}${Item:+${Page:+, }${Item}}
altpage:
  - p. ${Page}
repository: true
quality: 3
```

##### Example GEDCOM

```gedcom
0 @I1@ INDI
1 SOUR @C20064@
0 @C20034@ SOUR
1 TITL Christian County, Kentucky, Tax Books, 1797-1875
1 _TPLT https://gedcom.io/templates/TaxRoll
2 _FIEL Jurisdiction
3 TEXT Christian County, Kentucky
2 _FIEL Series
3 TEXT Tax Books, 1797-1875
1 REPO @F10004@
2 CALN 7,926-7,929
3 MEDI FILM
1 _QUAY 3
0 @C20064@ SOUR
1 TITL Christian County, Kentucky, 1799 Tax Book
1 _TPLT https://gedcom.io/templates/TaxVolume
2 SOUR @C20034@
2 _FIEL Volume
3 TEXT 1799 Tax Book
1 REPO @F10004@
2 CALN 7,926
3 MEDI FILM
1 _QUAY 3
0 @F10004@ REPO
1 NAME Family History Library
```

##### Example Formatted Citations

**Source List Entries**

- Christian County, Kentucky, 1799 Tax Book; FHL microfilm 7,926.
- Christian County, Kentucky, Tax Books, 1797-1875; FHL microfilm 7,926-7,929.

**First Reference Notes**

- Christian County, Kentucky, 1799 Tax Book; FHL microfilm 7,926.

### Input Matching

For applications that support the `alt*` fields, such fields can be used to assist in converting a non-templated source to a templated source by providing an appropriate default for a user to confirm.
Let’s look at an example using the Web Database model from *Evidence Explained*, pages 254 and 438.

We will use the following YAML snippet as an example:

```
    altpubl: ${Type}, ${Creator}, ${Website} (${Url} : accessed {Date})
    altpubl: ${Type}, ${Creator}, ${Website} (${Url})
    altpubl: ${Type}, ${Website} (${Url})
    altpubl: ${Website} (${Url})
    altpubl: ${Website}
    publ: ${Type:+${Type}, }${Creator:+${Creator}, }${Website} (${Url}${Date:+ : accessed ${Date}})
```

Above gives a number of patterns that can be recognized.  In this case, the `publ` pattern
used for output is not particularly usable for input due to the use of conditionals, and the
fact that Type and Creator are both optional and so if only one is present, one could not
reliably tell whether it is the Type or the Creator.  The use of the `altpubl` patterns solves
this by specifying the precedence.

Consider a GEDCOM containing:

- `1 PUBL database, Library and Archives Canada (LAC), Canadian Genealogy Centre (http://www.collectionscanada.ca/genealogy/index-e.html : accessed 21 February 2007)`

	The payload above matches the first pattern.

- `1 PUBL database, Library and Archives Canada (LAC), Canadian Genealogy Centre (http://www.collectionscanada.ca/genealogy/index-e.html)`

	The payload above matches the second pattern.

- `1 PUBL database, Canadian Genealogy Centre (http://www.collectionscanada.ca/genealogy/index-e.html)`

	The payload above matches the third pattern.

- `1 PUBL Canadian Genealogy Centre (http://www.collectionscanada.ca/genealogy/index-e.html)`

	The payload above matches the fourth pattern.

- `1 PUBL Canadian Genealogy Centre`

	The payload above matches the fifth pattern.
