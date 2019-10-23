.. _itp_gen_environment_guide_lexer_types:

===========
Lexer Types
===========

Scanners are generated from “lexer types” or “skeletons”. ITPilot
includes some options for the lexer type. Any option is valid for most
applications, but there are some situations in which a specific one is
preferred. This section describes the available options and provides
some examples. The options are:



-  **Replace tags inside extracted values with spaces**. When a DEXTL
   specification that makes use of a scanner generated from this lexer
   type, extracts any data element such that this piece of information
   contains an HTML element (or several adjacent HTML elements), this
   element (or group of adjacent elements) will be replaced by a blank
   space.
-  **Remove tags inside extracted values**. When a DEXTL specification
   that makes use of a scanner generated from this lexer type, extracts
   any data element which contains an HTML element, this element will be
   removed.
-  **Do not remove script code inside values (deprecated)**. When a
   DEXTL specification makes use of a scanner generated from this lexer
   type, extracts any data element which contains JavaScript code, this
   code will not be removed. This option is considered as deprecated now
   and **should not be used in new projects**.



The following example illustrates the difference between the two first
options. Let us suppose that an Extractor for an electronic bookstore
has been created. Among other fields, the extractor obtains the list of
authors of every book. In the HTML code of the source, authors are shown
with just a **<BR>** tag delimitation. For instance:

.. code-block:: html

   Jones, Peter <BR>
   Smith, John




If the option **Replace tags inside extracted values with spaces** is
used, the extraction process will substitute the <BR> tags by blank
spaces, and the retrieved value will be ‘Jones, Peter Smith, John’. In
case the option **Remove tags inside extracted values**, the retrieved
value will be ‘Jones, PeterSmith, John’.



The following example shows when the option **Remove tags inside
extracted values** is useful. Let us suppose that an electronic
bookstore allows searching by specifying the first letters of any word
contained in any book’s title. The results of that search show the
searched key in bold letters. For instance, the result of a search by
‘enterpr’ might contain HTML fragments such as the following:

``Advice for <B>Enterpr</B>ise leaders``

``<B>Enterpr</B>ise Information Systems``

In this case, if an Extractor based on the lexer type **Remove tags
inside extracted values** is created, it would return the values
**Advice for Enterprise leaders** and ‘Enterprise Information Systems’,
while an Extractor based on the option **Replace tags inside extracted
values with spaces** would return the values ‘Advice for Enterpr ise
leaders’ and ‘Enterpr ise Information Systems’ (notice the blank
spaces).




