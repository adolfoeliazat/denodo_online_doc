===========
Format Tags
===========

As mentioned earlier, a *format tag* is defined by a regular expression.
Each format tag has an associated name. The format tag definitions are
of the form:



‘NAME:=Expression’



Where *NAME* is the name of the tag and *Expression* is the regular
expression.





**Example**: The definition of the tag EOL (END-OF-LINE), as a regular
expression that includes all the possible ways of representing a
carriage return or line end in HTML, could be: EOL:= ("<br"[^\\>]\*">"
\| "</p"[^\\>]\*">"\| "</tr"[^\\>]\*">").



ITPilot provides by default a set of *format tags* that cover all
the HTML tags, for correct extraction of information from HTML
documents. The set of all available tags can be checked in the Scanners
and Tagsets tool of the ITPilot Wrapper Generation Tool.



At the same time the format tags are detected, some of them define
parameters (or identifiers) to which certain values can be assigned, and
that will be passed to the user applications along with the tag types.
These parameters can be used later in the DEXTL extraction patterns.



The value assigned to a parameter can be:

-  A constant. Example: ANCHOR(URL="\http://www.denodo.com")
   
-  A variable of the context. Example: ANCHOR(URL=\@VARIABLE). In this
   case, the value assigned to the parameter will be the value of the
   VARIABLE variable in the current context at the time the tag is
   obtained from the page.
-  An HTML attribute. This is the most common case. Example:
   ANCHOR(URL=href). It will take the value assigned to the indicated
   HTML attribute (in this case href). Starting with version 4.6 of
   ITPilot, all the HTML attributes of the tags are implicitly declared,
   and there is no need for explicitly declaring them; when inspecting
   the definition of a tag, take into account that all the attributes of
   the HTML tag are available to use in the DEXTL pattern. The names of
   the attributes are not case sensitive, to account for case
   differences between browsers.

In the cases of the name of the identifier being a reserved word, this
appears prefixed with the "$" symbol.



**Example**: The definition of an ANCHOR format tag that matches the tag
‘<A …>’ of the HTML language can be the following, obtaining the value
of the attribute ‘href’ contained in said HTML tag by assigning it to an
identifier called URL: ANCHOR(URL=href):="<a" [^>]\* " >".



In this way, when a portion of text is encountered in the document that
matches the regular expression, in addition to identifying an occurrence
of the ANCHOR format tag, the portion of the text found, assigned to the
attribute href of the tag "<a" that caused the concordance, is assigned
to the identifier URL. The regular expression used to specify "<a"-type
tags is constructed in the following manner: it must begin with the
character string "<a". Any combination of different characters of ‘>’
may go after said string (this is what the portion ‘[^>]\*’ means) and,
finally, there is the character ‘>’, which closes the HTML tag. These
regular expressions use the format of `JFlex <https://www.jflex.de/>`_ regular expressions.



As discussed previously, the value of the identifiers obtained in this
way may be used in the patterns, for example, to assign the value
obtained to one of the fields of the extracted tuples. The notation used
for this is as follows: ‘NAMEFORMATTAG(:ATTRIB1=ID1,…, :ATTRIBN=IDN)’,
where ATTRIB1,…, ATTRIBN are names of attributes in the relation from
which tuples are being extracted and ID1,…,IDN are identifiers present
in the format tag definition, or in the HTML tags. We say that the
attributes like ATTRIB1, ATTRIB2…ATTRIBN are *linked* to the tag
‘NAMEFORMATTAG’.



**Example**: Consider an electronic bookshop represented by the relation
R={TITLE,AUTHOR, PRICE, LINKTOMOREINFO}, where the field LINKTOMOREINFO
contains a URL which provides direct access to the shop’s HTML page in
which detailed data on the product is found. If the graphic appearance
of the output is similar to that shown in :ref:`Results returned in an online bookshop`
and we assume that the ANCHOR tag is defined in the same way
as in the previous example, in addition to the BR tag, defined in the
usual way, we can write the following pattern to extract the tuples from
R.

.. code-block:: none
   :caption: Pattern that Extracts Data from a Format Tag
   :name: Pattern that Extracts Data from a Format Tag

   {NAME="R"
   ANCHOR(:LINKTOMOREINFO=URL) :TITLE BR
   IRRELEVANT BR
   :AUTHOR "/"IRRELEVANT BR
   "Our Price:" ¿:PRICE "," IRRELEVANT? BR
   }

Another important note in respect of these identifiers is that a tag
that has them defined will match a specific portion of text in the
document analyzed only if the portions referring to said identifiers
also appear.



To indicate that the tag to match regardless of whether or not the
portion that matches the identifier appears in the document, this should
be signaled using the symbols "¿" and "?" (or "/?" and "?/"). For
example, if a tag ANCHOR(:REFMOREINFO=URL) is defined in the pattern
specification and DEXTL receives from the document a tag ANCHOR() with
no value for URL, it will not match. However, if it had been put in the
specification ANCHOR(¿:REFMOREINFO=URL?) or
ANCHOR(/?:REFMOREINFO=URL?/), it would have.



Thus, in summary:


-  If the tag <a…> does not have a href attribute, then the scanner returns
   an ANCHOR token without URL.

-  If the tag <a…> has a href attribute, the scanner returns an ANCHOR
   token with URL=value\_href

-  Now, if the specification contains ANCHOR(:REFMOREINFO=URL)

   -  If an ANCHOR() token is received by the DEXTL interpreter, then the
      pattern will not match.
   -  If an ANCHOR(URL=value\_href) token is received by the DEXTL
      interpreter, then the pattern will match and the tag :REFMOREINFO
      will be assigned the value\_href.

-  If the specification contains ANCHOR(¿:REFMOREINFO=URL?) or
   ANCHOR(/?:REFMOREINFO=URL?/)

   -  If an ANCHOR() token without URL is received by the DEXTL
      interpreter, then the pattern matches, but no value is assigned to
      REFMOREINFO (the attribute is not even created).
   -  If an ANCHOR(URL=value\_href) token is received, then the pattern
      matches and :REFMOREINFO is assigned the value\_href.

-  If the DEXTL specification contains ANCHOR() (or ANCHOR, they are the
   same)

   -  If an ANCHOR() token is received by the DEXTL interpreter, it
      matches.
   -  If an ANCHOR(URL=value\_href) token is received by the DEXTL
      interpreter, then the pattern matches and no variables are assigned.



Finally, when the tags are defined, the value "true" can be assigned to
a tag attribute depending on whether an attribute is present in the HTML
tag to which the tag refers (regardless of the value or even if it has
no assigned value).



For example, if a tag is defined for the tag OPTION, we may want to know
if it is the selected one or not (i.e. if the SELECTED attribute is
present or not):



OPTION(VALUE=value,SELECTED=¿selected?):="<option" [^>]\* ">"



The attribute "VALUE" of the tag is assigned the value of the attribute
"value" from the HTML tag OPTION. If the "selected" attribute is present
in the HTML tag, then the value "true" is assigned to the "SELECTED"
attribute of the tag (otherwise a value is not assigned).



Tagsets
=======

*Format tags* are grouped into *tagsets*. A *tagset* is a list of format
tag definitions grouped under a common name. Tagsets are created
graphically from the Scanners and Tagsets tool of the ITPilot Wrapper
Generation tool and internally have the following syntax:


.. code-block:: none
   :name: Tagset
   :caption: Tagset

   SET "NAMESET"
   NAMETAG1:=Expr1
   ...
   NAMETAGn:=Exprn
   ENDSET

Where *NAMESET* is the name of the tagset, *NAMETAG1, …, NAMETAGn* are
names of format tags and *Expr1,…,Exprn* are expressions of format tag
definitions.



To facilitate its reuse the tagsets are saved in separate files that can
be included later for the specifications that wish to use them. Existing
tagsets can be checked in the Scanners and Tagsets tool, and also are
found in the directory :file:`{<DENODO_HOME>}/metadata/parsers/wgt/tagsets`.
Each file corresponds to the definition of a tagset.



By default, ITPilot includes the following built-in tagsets:



-  All4\_6. Used by default by the process which generates DEXTL
   programs using examples. It defines a tag
   for every HTML tag.
-  All4\_6\_xx. A set of pre-generated tagsets that represents the most
   commonly used tagsets. They define a tag for every HTML tag except
   for those that appear in their name. For example, all4\_6\_anchor
   recognizes all the HTML tags except the anchor tag <a>.



Due to backwards compatibility reasons all the tagsets of earlier
versions of ITPilot are included.



For a new tagset defined by the user to be available in DEXTL programs,
a "scanner" must first be generated for said tagset. The graphical
creation process is described in detail in the :doc:`ITPilot Generation Environment Guide <../../../generation_environment/index>`.



Finally, it is very important to highlight that various tagsets can be
used in the same specification for the different relations and
subrelations involved in the specification.
