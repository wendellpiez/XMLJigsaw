
# hints

## produce updated composed metaschema

This presently can't be done under SaxonJS due to no support for external parsed entities (a current requirement).

But runs fine under Saxon in Java, etc. This would be (for example, something like):

```
$ xslt3 -s:https://raw.githubusercontent.com/usnistgov/OSCAL/master/src/metaschema/oscal_catalog_metaschema.xml -xsl:https://raw.githubusercontent.com/usnistgov/metaschema/master/toolchains/xslt-M4/nist-metaschema-COMPOSE.xsl -o:generators/oscal_catalog_metaschema-COMPOSED.xml          
```

## update validator from metaschema

For example, for the OSCAL catalog:

```
$ xslt3 -s:generators/oscal_catalog_metaschema-COMPOSED.xml -xsl:generators/generate-validator.xsl -o:catalog-validate-new.xsl
```

## compile for SaxonJS

Likewise --

```
$ xslt3 -export:apply-validator.sef.json -xsl:apply-validator.xsl -nogo
```

# old

## Constrainatron

A Schematron-alike -

Borrows Schematron syntax and architecture

Updated and simplified

- Embeds it into XSLT - no longer at arm's length, this sits in XSLT's lap
- Seeks to retain the best features of the old Schematron design
- Adds new features
- Provides enhanced functionality for free (traceability/tagging)
- Easy to use and deploy - comes with its own (XSLT) shell

Details

- Takes the form of an XSLT stylesheet, with a few new elements
- Schematron `rule`, `assert` and `report` work as they did
  - Except @context and @test is XPath 3.1 with support for stylesheet functions, etc.
- Schematron `pattern` is replaced by `rule-set` (similar but a little different)
- New elements `when` and `otherwise` provide additional (targeted) rule scoping
- No more sibling precedence for rules - within a pattern, all matching rules always apply
- Compiled stylesheet, applied to an input, emits
  - Copy of input annotated with SVRL (fail-only or pass/fail)
  - Or (from this), just SVRL
  - SVRL extensions support error/id tracking
- Works with a simple (one-pass) XSLT compiler producing XSLT
  - (Did we mention?) Comes with its own XSLT shell for compiling dynamically
  - Extensible (just add to the compiler)
    - With demo extensions? (cf NIST Metatron)
- Supports @role (report level) and @id (for tracing)
- Free documentation support - helps implementations to be more explicit and traceable
- XSLT 3.0 syntax sugar (attribute and text value templates) makes everything more concise and legible
- XSLT 3.0 as the host environment gives us a Turing complete language for extensibility
- Can be validated inside your XSLT IDE (?) using an RNC (?)/Schematron/Schematroid

