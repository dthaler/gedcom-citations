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

- EasyBib (<http://www.easybib.com/cite/form>) has a web entry form and can generate citations in a variety of citation formats, including APA, Chicago, MLA, Turabian, etc.
- EndNote (<https://clarivate.com/webofsciencegroup/support/endnote/>) manages source and can generate citations in a variety of formats (see for example position 6:00 in the Windows video), including APA, Chicago, Turabian, etc.
- The OCLC Citations API (<https://developer.api.oclc.org/citations-api>) can access the WorldCat database and generate citations in a variety of formats (see CitationStyle), including APA, Chicago, MLA, Turabian, etc.

[Citations: Goals and Considerations (fhiso.org)](https://fhiso.org/TR/citation-goals) also explains:

> *Property sets* and *templates* are used by BibTeX, CiteProc, CSL, EndNote, Zotero, and other
> academic document preparation toolchains. *Citation data* is provided in this format by most
> academic-oriented online repositories and archives. *Templates* in this format are offered by
> most academic publication venues.

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
or open source defacto standard.

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
or open source defacto standard, but it is very similar to standard programming languages.
On the other hand, it is much more verbose than RootsMagic's and Family Historian's template languages.

## Purpose

Many types of sources exist with a common set of elements that can be used to format a citation.
In order to facilitate formatting of consistent citations, it is desirable to allow
specifying such information with a master source.

This extension adds a tag, `_TPLT`, to allow recording the fact that a given master source
has citation elements in common with other similar sources.

This extension also allows associating citation elements with a given source, by use of
new tags that can be used together with the `_TPLT` tag to allow applications to prompt
for, store, and generate citations for various types of sources.

## Tags

### `_TPLT` (Source Template)

A substructure to identify a Source Template that contains additional information on how data should be captured for the associated source and how those data should be combined to create a citation.

#### `_TPLT`

Source Templates allow applications to give users a set of data to capture for a given source type
(such as a grave marker, obituary, will, etc.) and how such data should be formatted in a footnote citation.
Source templates might be published as standard source templates or might be specific to a given application.

The URI for this extension's `_TPLT` tag is `ext:_TPLT`.

The `_TPLT` structure is used as a substructure of the `SOUR` structure as follows:

```
n SOUR @XREF:SOUR@                          {1:1}  g7:record-SOUR
  +1 <<SOUR_DETAIL>>                        {0:1}
     +2 _TPLT <URI>                         {0:1}  ext:_TPLT
        +3 _FIEL <Text>                     {0:M}  ext:_FIEL
           +4 TEXT <Text>                   {0:1}  g7:TEXT
              +5 MIME <MediaType>           {0:1}  g7:MIME
              +5 LANG <Language>            {0:1}  g7:LANG
        +3 SOUR @<XREF:SOUR>@               {0:1}  g7:SOUR
```

The source template's payload is a [URI](https://www.rfc-editor.org/info/rfc3986) where the source template definition can be found.

The `_TPLT` substructures are:

`_FIEL` (Field)
: A field or parameter from the source template. The payload contains the machine-readable variable name from the source template. The name displayed to users may be something else, such as the human-readable label from the source template.

### `SOUR` (Source)

In addition to the `_TPLT` substructure added in this extension, use of a `_TPLT` together with a source collection requires adding support for the following
substructure of the `SOUR` structure:

```
n SOUR @XREF:SOUR@                          {1:1}  g7:record-SOUR
  +1 <<SOUR_DETAIL>>                        {0:1}
     +2 _TPLT <URI>                         {0:1}  ext:_TPLT
        +3 SOUR @<XREF:SOUR>@               {0:1}  g7:SOUR
```

The `SOUR` substructure is used to identify a collection that the `SOUR` is contained in, such as when using a source template that is for a source within a collection.

## Definitions

Master Source
: A `g7:record-SOUR`.

Source Detail
: A `g7:SOUR` substructure of an `INDI` or `FAM` record.

Citation
: Data that would be output when displaying the associated source in a source list entry, first reference note, or short subsequent note.

Citation Element
: A piece of information about a source, such as its author, title, publication date, repository, URL, etc.

Source Template
: A set of information about a class of sources, with a common set of citation elements and rules for formatting citations of sources of that class.

## Extensions to the `SOUR` structure

### Overview

This extension adds the following substructure to an existing `SOUR` structure:

- A `_TPLT` substructure whose payload is the URI of a source template.
- Multiple `_FIEL` substructures of the `_TPLT` that provide values of citation elements for the master source as specified by the source template, if any.
- `_FIEL` substructures of a `g7:SOUR` `g7:PAGE` that provide values of citation elements for the source details as specified by the source template, if any.
- A `SOUR` substructure of a `_TPLT` that provides a cross-reference to a collection that the source is contained in.

### Citation Element Values

Following GEDCOM conventions, citation element values are not empty unless explicitly stated otherwise in the source template.

### Examples

See [Examples of Source Templates](#examples-of-source-templates).

## Source Templates

### Defining Source Templates

A source template is a YAML file identified by its URI, which is preferably a URL that will resolve
to the YAML file.

Source templates associate citation elements with GEDCOM tags and contain Liquid template patterns that
say how citation elements should be formatted when generating a citation.

### Template Patterns

See [Appendix: Template Patterns](#appendix-template-patterns).

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
      "description": "IANA language tag of the language of this template."
    },
    "uri": {
      "type": "string",
      "description": "The URI of this source template."
    },
    "name": {
      "type": "string",
      "description": "Textual name which could be displayed to users in a list of source templates."
    },
    "description": {
      "type": "string",
      "description": "Textual description which could be displayed to users in a list of source templates."
    },
    "collection": {
      "type": "string",
      "description": "URI of the source template for the containing collection."
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
            "description": "Machine-readable variable name of the citation element."
          },
          "label": {
            "type": "string",
            "description": "Human-readable label associated with the citation element."
          },
          "required": {
            "type": "boolean",
            "description": "Whether the citation element is required to have a  non-empty value."
          }
        },
        "required": ["variable", "label"],
        "additionalProperties": false
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
            "description": "Machine-readable variable name of the citation element."
          },
          "label": {
            "type": "string",
            "description": "Human-readable label associated with the citation element."
          },
          "required": {
            "type": "boolean",
            "description": "Whether the citation element is required to have a  non-empty value."
          }
        },
        "required": ["variable", "label"],
        "additionalProperties": false
      }
    },
    "formats": {
      "type": "array",
      "description": "Array of style guides for formatting citations.",
      "items": {
        "type": "object",
        "properties": {
          "name": {
            "type": "string",
            "description": "The name of the style guide for formatting citations, such as 'Evidence Explained'."
          },
          "abbr": {
            "type": "string",
            "description": "Template pattern that specifies how to construct an abbreviated title for the master source. The abbreviated title constructed can be displayed in a source list when the source record does not have an ABBR structure. (An implementation might store the abbreviated title constructed in an ABBR structure, but this is not required.) The payload must not contain variables defined in the dfields array, but may contain variables defined in the sfields array, and if a collection is specified then it may also use the variable \"CollectionAbbr\" which will expand to the collection's abbreviated title."
          },
          "titl": {
            "type": "string",
            "description": "Template pattern that specifies how to construct a TITL payload. The payload must not contain variables defined in the dfields array but may contain variables defined in the sfields array."
          },
          "auth": {
            "type": "string",
            "description": "Template pattern for formatting an AUTH using the given style guide."
          },
          "publ": {
            "type": "string",
            "description": "Template pattern for formatting a PUBL using the given style guide."
          },
          "scite": {
            "type": "string",
            "description": "Template pattern for formatting the master source portion of a first reference note."
          },
          "page": {
            "type": "string",
            "description": "Template pattern for formatting a PAGE using the given style guide."
          },
          "slist": {
            "type": "string",
            "description": "Template pattern for formatting a source list entry using the given style guide."
          },
          "first": {
            "type": "string",
            "description": "Template pattern for formatting a first reference note using the given style guide."
          },
          "short": {
            "type": "string",
            "description": "Template pattern for formatting a short subsequent note using the given style guide."
          }
        },
        "required": ["name", "titl", "scite"],
        "additionalProperties": false
      }
    },
    "alttitl": {
      "type": "array",
      "description": "Array of patterns that can be matched for an existing TITL.",
      "items": {
        "type": "string"
      }
    },
    "altauth": {
      "type": "array",
      "description": "Array of patterns that can be matched for an existing AUTH.",
      "items": {
        "type": "string"
      }
    },
    "altpubl": {
      "type": "array",
      "description": "Array of patterns that can be matched for an existing PUBL.",
      "items": {
        "type": "string"
      }
    },
    "altpage": {
      "type": "array",
      "description": "Array of patterns that can be matched for an existing PAGE.",
      "items": {
        "type": "string"
      }
    },
    "repository": {
      "type": "boolean",
      "description": "Whether the master source is expected to be in a repository."
    },
    "quality": {
      "type": "integer",
      "description": "Quality (roughly 0=unreliable, 1=questionable, 2=secondary, 3=primary, 4=original)."
    }
  },
  "required": ["lang", "uri", "name"],
  "additionalProperties": false
}
```

To dereference a URI specifying a source template:

1. Look for a locally-installed file with a matching `uri` property.
2. Attempt to retrieve the file from the URI directly via HTTPS.

## Using Source Templates

An application can use a source template as follows.

Creating a source from a template:

1. The application lets the user pick or search for a source template.
2. The application prompts the user to enter values for citation elements as specified by the `sfields` array in the source template.
3. The application creates a master source with `TITL`, `AUTH`, and `PUBL` whose values are generated using the corresponding template patterns in the source template.  If a template pattern is not specified in the source template, the GEDCOM tag is omitted.
4. The application creates a `_TPLT` substructure with the URI of the source template.
5. The application creates `_FIEL` substructures of the `_TPLT` with variables and values of any citation elements that (a) are in the source template, and (b) have non-empty values.

Creating a first reference note for a citation to a source with a template:

1. The application identifies the source template (if any) from the `_TPLT` of the master source.
2. The application prompts the user to enter values for citation elements as specified by the `dfields` array in the source template.
3. The application sets the `PAGE` value by generated using the `page` template pattern in the source template.  If a template pattern is not specified in the source template, the GEDCOM tag is omitted.
4. The application generates text for the citation using the `first` template pattern.  If such a pattern is not present, the following template pattern is used instead: `{% if scite %}{{ scite }}{% else %}{{ TITL }}{% if AUTH %}, by {{ AUTH }}{% endif %}{% if PUBL %} ({{ PUBL }}){% endif %}{% endif %}{% if PAGE %}, {{ PAGE }}{% endif %}{% if URL %}; {{ URL }}{% endif %}{% if NOTE %}. {{ NOTE }}{% endif %}.`  When evaluating this template pattern, the application substitutes values as follows:
   * Any field appearing in the source template (whether in the `sfields` or `dfields` arrays) uses the value entered by the user.
   * If the pattern `scite` appears in the source template, it is expanded using the template pattern there.
   * Otherwise variables are expanded using the value of the corresponding GEDCOM tag.
5. The application creates `_FIEL` substructures of the `PAGE` with variables and values of any citation elements that (a) are in the `dfields` of the source template, and (b) have non-empty values.

## Examples of Source Templates

The following examples show the YAML text, which can be placed into `.yaml` files.

### Grave Marker Template

The Grave Marker example is taken from the monument example at *Evidence Explained*, pages 246-247.

#### Grave Marker YAML

```yaml
%YAML 1.2
---
lang: en-US
uri: https://gedcom.io/templates/GraveMarker
name: Grave Marker
description: The Grave Marker model is used for grave markers, cemetery monuments,
  and other forms of cemetery art (see "Evidence Explained", pages 246-247).
keywords:
  - monument
  - headstone
  - cemetery
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
    titl: {{ Cemetery }}
    publ: {{ Location }}
    scite: {{ Cemetery }}{% if Location %} ({{ Location }}){% endif %}
    page: {{ Section }}
```

#### Example GEDCOM

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

#### Example Formatted Citations

**Source List Entry**

Brian Cemetery (Lawrence County, Illinois).

**First Reference Note**

Brian Cemetery (Lawrence County, Illinois), Samuel Witter marker.

### Tax Record Templates

The Tax Roll and Tax Volume examples are taken from the microfilm examples at *Evidence Explained*, page 531.

#### Tax Roll YAML

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
    titl: {{ Jurisdiction }}, {{ Series }}
    publ: {{ Location }}
    scite: {{ Jurisdiction }}, {{ Series }}
    page: {{ Section }}
repository: true
quality: 3
```

#### Tax Volume YAML

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
    titl: {{ Jurisdiction }}, {{ Volume }}
    scite: {{ Jurisdiction }}, {{ Volume }}
    page: |
      {% if Section %}{{ Section }}{% if Page %}, {% endif %}{% endif %}
      {%- if Page %}p. {{ Page }}{% endif %}
      {%- if Item %}{% if Page %}, {% endif %}{{ Item }}{% endif %}
altpage:
  - p. {{ Page }}
repository: true
quality: 3
```

#### Example GEDCOM

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

#### Example Formatted Citations

**Source List Entries**

- Christian County, Kentucky, 1799 Tax Book; FHL microfilm 7,926.
- Christian County, Kentucky, Tax Books, 1797-1875; FHL microfilm 7,926-7,929.

**First Reference Notes**

- Christian County, Kentucky, 1799 Tax Book; FHL microfilm 7,926.

## Input Matching

For applications that support the `alt*` fields, such fields can be used to assist in converting a non-templated source to a templated source by providing an appropriate default for a user to confirm.
Let's look at an example using the Web Database model from *Evidence Explained*, pages 254 and 438.

We will use the following YAML snippet as an example:

```
    altpubl:
      - "{{ Type }}, {{ Creator }}, {{ Website }} ({{ Url }} : accessed {{ Date }})"
      - "{{ Type }}, {{ Creator }}, {{ Website }} ({{ Url }})"
      - "{{ Type }}, {{ Website }} ({{ Url }})"
      - "{{ Website }} ({{ Url }})"
      - "{{ Website }}"
    publ: {% if Type %}{{ Type }}, {% endif %}{% if Creator %}{{ Creator }}, {% endif %}{{ Website }} ({{ Url }}{% if Date %} : accessed {{ Date }}{% endif %})

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

## Appendix: Template Patterns

For programmatically generating GEDCOM structures from a given source template, this document
recommends template patterns using [Liquid template syntax](https://shopify.github.io/liquid/).

### Why Liquid?

Liquid was created by Shopify specifically for safe template evaluation with untrusted input. It's battle-tested at scale:
- Used by GitHub Pages (millions of templates daily)
- Used by Jekyll static site generator
- Used by Shopify (billions of template renders)

### Safety Guarantees

Liquid is designed to be safe by default:
- **No infinite loops**: Iteration is limited (default 100, configurable)
- **No arbitrary code execution**: Only pre-registered filters and tags
- **No file system access**: Cannot read/write files
- **No network access**: Cannot make HTTP requests
- **Resource limits**: Memory and CPU limits in implementations

### Parser Availability

Mature, well-maintained parsers exist for all major languages:

| Language | Library | GitHub Stars | URL |
|----------|---------|--------------|-----|
| Ruby | liquid | 10.8k | <https://github.com/Shopify/liquid> |
| Python | python-liquid | 300+ | <https://github.com/jg-rp/liquid> |
| JavaScript | liquidjs | 1.5k | <https://github.com/harttle/liquidjs> |
| Java | Liqp | 240+ | <https://github.com/bkiers/Liqp> |
| Go | liquid | 270+ | <https://github.com/osteele/liquid> |
| PHP | liquid-php | 40+ | <https://github.com/kalimah-apps/liquid-php> |
| C# | DotLiquid | 1k | <https://github.com/dotliquid/dotliquid> |
| Rust | liquid-rust | 430+ | <https://github.com/cobalt-org/liquid-rust> |

### Liquid Patterns

`{{ variable }}`
: Replace with the value of the specified variable.

`{{ variable | default: "text" }}`
: If variable is non-empty, replace with its value. Otherwise replace with the specified text.

`{% if variable %}text{% endif %}`
: If variable is empty, do nothing. Otherwise replace with the specified text.

`{{ variable | escape }}`
: Replace with HTML-escaped value of the variable.

### Common Filters

Liquid provides filters for common text transformations:

```liquid
{{ Author | split: ", " | reverse | join: " " }}  # "Smith, John" → "John Smith"
{{ Date | date: "%Y" }}                           # Extract year from date
{{ Title | capitalize }}                          # Proper case
{{ Name | upcase }}                               # Convert to uppercase
```

### Lists

For multiple authors or other repeated elements:

```liquid
{% for author in Authors limit:10 -%}
  {{ author }}{% unless forloop.last %}, {% endunless %}
{%- endfor %}
```

### Complex Conditionals

More readable than nested patterns:

```liquid
{% if Type == "database" %}
  database
{% elsif Type == "digital" %}
  digital image
{% else %}
  {{ Type }}
{% endif %}
```

### Common Template Patterns

Here are common Liquid patterns used in source templates:

| Pattern | Description |
|---------|-------------|
| `{{ variable }}` | Replace with the value of the specified variable |
| `{{ variable \| default: "default" }}` | Use default value if variable is empty |
| `{% if variable %}text{% endif %}` | Conditional text inclusion |
| `{{ variable \| escape }}` | HTML-escape the variable value |

Note: Liquid rarely needs escaping. When needed, use `{{ "{{" }}` to produce `{{`.

### Example Template Pattern

```liquid
{{ Cemetery }}{% if Location %} ({{ Location }}){% endif %}
```

This pattern outputs the cemetery name, and if a location is provided, adds it in parentheses.

```liquid
{% if Type %}{{ Type }}, {% endif %}{% if Creator %}{{ Creator }}, {% endif %}{{ Website }} ({{ Url }}{% if Date %} : accessed {{ Date }}{% endif %})
```

This pattern conditionally includes type and creator if provided, always includes the website and URL, and optionally adds an access date.