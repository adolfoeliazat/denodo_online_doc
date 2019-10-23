======================================================
Syntax of Search Expressions for the Contains Operator
======================================================


This section describes the syntax of search expressions of the
``contains`` operator.

.. contents::
   :depth: 1
   :local:
   :backlinks: none

Exact Terms and Phrases
=======================

A query is made up of terms and operators. There are two types of terms:
Individual Terms and Exact Phrases.

An Individual Term is a single word. A phrase is a group of words
between double quotes. Terms may be combined using Boolean operators to
form complex queries (see below).

.. note:: The whole expression must be surrounded by single quotes so
   when using a phrase as expression both single and double quotes are
   required. E.g.

.. code-block:: vql

   '"Denodo Technologies"'

Term Modifiers
==============

The use of the following modifiers is accepted:

Search Wildcards
--------------------------------------------------------------------------------

The symbol ``?`` replaces ``?`` for a single character in the
word. The symbol ``*`` replaces ``*`` for 0 or more characters.
For example, if you want to search for “information” or “informative”,
the following term could be entered:



.. code-block:: vql

   'inform*'


Fuzzy Searches
--------------------------------------------------------------------------------

Fuzzy searches are allowed (sources may implement this function using
string editing distance techniques, for example). To make fuzzy
searches, add the operator "``~``" at the end of a simple term (the
operator has to be placed right next to the term without any space in
between).

For example, to search for terms similar to “card”, use the following
fuzzy search:



.. code-block:: vql

   'card~'

This would find terms such as “cad”.

A parameter (optional) can be added to specify the minimum similarity
required. For example:



.. code-block:: vql

   'card~0.8'
   
To reference another field from a condition with the ``contains``
operator, add the full name of the field (the name of the view and the field) prefixed by the character ``$``.

For example,



.. code-block:: vql

   SELECT *
   FROM bv_aracne INNER JOIN bv_test_json_view
   ON bv_aracne.summary contains '$bv_test_json_view.field1~0.7'

If you add the minimum similarity parameter (in this example: ``~0.7``),
do not leave any space between the name of the field and the operator, nor between the operator and the value.

There are several available similarity algorithms (find the available algorithms in the documentation of the function 
:ref:`SIMILARITY`). The default similarity strategy used in fuzzy 
searches is "JaroWinklerTFIDF". To change the similarity algorithm used by this operator, 
execute this command from the VQL Shell:

.. code-block:: vql

   SET 'com.denodo.vdb.util.TextualSimilarity.distanceMetric' = '<strategy>';

Restart the Virtual DataPort server to apply the change.


Proximity Searches
--------------------------------------------------------------------------------

Searches for terms among which there is a certain spatial proximity are
allowed. To implement these, use the symbol "``~``" at the end of an
exact phrase. The maximum number of words to separate the terms can also
be specified. For example, to search for “denodo” and “technologies”
with a distance of up to 8 words in the same document, the following
search would be used:



.. code-block:: vql

   '"denodo technologies"~8'


Range Searches
--------------------------------------------------------------------------------

Range searches allow for documents with values within a certain range to
be retrieved. The range specified may or may not include the upper and
lower limits. Inclusive ranges are specified using square brackets and
exclusive ranges using curly brackets. The classification follows the
lexicographic order. For example:



.. code-block:: vql

   ' [20020101 TO 20030101] '



This query finds documents with a value of between 20020101 and
20030101, inclusively. The range search is not limited to the fields
containing dates as the value:



.. code-block:: vql

   '{Aida TO Carmen}'

This query retrieves all documents with titles found between Aida and
Carmen, not inclusively.


Boosting the Relevance Level of a Term
--------------------------------------------------------------------------------

It is possible to boost the weight of a term in the search when
calculating the level of relevance using the symbol “^” with a boosting
factor (a number) at the end of the search term. The higher the factor,
the more relevant the term in the search.

This allows for the relevance of a document to be controlled by boosting
the relevance level of its terms. For example, if you want to search for
the terms “denodo” and “technologies”, and the term “denodo” is the most
relevant, use the operator ``^`` with a relevance level boosting factor
alongside the term:


.. code-block:: vql

   'denodo^4 and technologies'

This ensures that the documents containing the term “denodo” are most
relevant for the search. This technique can also be used with phrases.

The default relevance factor is 1. This must be a positive number,
although it may be less than 1 (for example, 0.2).

Boolean Operators
=================

Boolean operators allow combining terms using logic operators. The
following Boolean operators are accepted: AND, OR, and NOT (
Boolean operators must be written in upper-case letters.).

Groups
======

The use of brackets is allowed. For example, to search for “Corp” or
“Inc” and “Denodo”, the following query would be used:



.. code-block:: vql

   ' (Corp OR Inc) AND denodo '

Escaping Special Characters
===========================

The list of special characters is the following:

.. code-block:: vql

   ( ) { } [ ] ^ " ~ * ? : \

To escape these characters, use ``\`` before the character.
