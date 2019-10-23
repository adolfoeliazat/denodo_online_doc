==================
Syntax Conventions
==================


.. toctree::
   :hidden:

   syntax_of_functions_and_condition_values.rst

The following sections of this document describe the different
operations that can be executed using VQL. The notation and syntax
conventions used for this description are provided below.

-  The language is not case-sensitive. I.e. “CREATE VIEW” is equivalent
   to “create view”
-  The text in lower case and specified between the symbols ``“<``
   and ``>`` (e.g. ``<name>``) indicates an element whose specific
   syntax will be specified later. If the separator ``:`` appears
   (e.g. ``<name:element-definition>``) | this indicates a name of
   a representative element followed by the name of the element that
   defines it.
-  The symbols ``::=`` declare the definition of an element.
-  The square brackets (``[]``) indicate optional elements. When they
   must appear in a statement | they are specified in inverted commas to
   explicitly indicate that they should appear and that they do not
   indicate optional elements.
-  The asterisk (``*``) indicates that an element can be specified zero or
   more times. Example: ``[<search_method_clause>]*`` indicates that the
   element ``[<search_method_clause>]`` can be repeated as many times as
   necessary.
-  The plus sign (``+``) indicates that an element can be specified one
   or more times. Example: ``[<field>]+`` indicates that the element
   ``[<field>]`` should appear at least once and can be repeated as many
   times as required.
-  Elements separated by the character ``|`` and possibly grouped
   between braces (``{}``) indicate alternative elements. For example:
   ``{element1 | element2}`` indicates that ``element1`` or
   ``element2`` have to be written in this position.
-  The commas (``,``) are used in syntax constructions to separate the
   elements of a list.
-  The brackets (``()``) normally serve to group expressions and
   increase priority. In some cases, they are required as part of the
   specific syntax of a statement.
-  The full stop (``.``) is used in numeric constants and to separate
   names of tables | columns and fields.
-  The whitespace character can be a space | a tab | a carriage return or
   a line jump.
-  Identifiers (``<identifier>``). Identifiers are the names of the
   elements of the catalog. If at least one of the characters of the
   identifier is not a number or in is not in the range of “a” to “z”,
   it is a ``<quoted identifier>``. Therefore, | it has to be surrounded
   with double quotes (``"``).
   Certain words cannot be used as identifiers. See the definition of
   ``<reserved VQL words>`` in `Basic primitives for specifying VQL
   statements`_.
-  Numbers (``<number>``). A number is a combination of digits that can
   be preceded by a ``-`` symbol and can include a full stop as a
   decimal separator point and optionally an exponent (if they are real
   numbers).
-  Logical values (``<boolean>``). Representation of the ``true``
   and ``false`` logical values.
-  Literals (``<literal>``). They represent any string that is not an
   identifier nor a number nor a logical value. This may be any string
   that is found in inverted commas (single or double commas). If a
   literal contains single or double comma characters (depending on the
   case) | they should be escaped (``\'`` and ``\"`` respectively).
-  Operators (``<operator>``). Represent operators in the system.

.. vql_guide_figure_basic_primitives_for_specifying_vql_statements:

.. code-block:: bnf
   :caption: Basic primitives for specifying VQL statements
   :name: Basic primitives for specifying VQL statements

   <identifier> ::= 
       <basic identifier> 
     | <quoted identifier>
   
   <basic identifier> ::= 
     [A-Za-z\200-\377][A-Za-z\200-\377_0-9\$]*
   
   <quoted identifier> ::= 
     ".*" (a double quote in an identifier has to be escaped with another double quote)
   
   <view identifier> :: = 
     <identifier with database>
   
   <identifier with database> ::=
       <element name:identifier> (assumes that the element exists in the current database)
     | <database name:identifier>.<view name:identifier>
   
   <integer> ::=
     [-][0-9]+
   
   <number>  ::= 
       <integer> 
     | (([0-9]*\.[0-9]+)|([0-9]+\.[0-9]*)) ((([0-9]*\.[0-9]+)|([0-9]+\.[0-9]*)|([0-9]+))([Ee][-+][0-9]+))
                
   <boolean> ::= 
       TRUE 
     | FALSE
   
   <literal> ::= '.*' (a single quote in a literal has to be escaped with another single quote. E.g. 'literal''with a quote')
   
   <operator> ::= 
       <unary operator> 
     | <binary operator>
   
   <opsymbol> ::= 
     [~!@#^&|?<>=]+
   
   <unary operator> ::=
       is null
     | is not null
     | is true
     | is false
   
   <binary operator> ::=
       =
     | <identifier>
     | <opsymbol>
     | IN
                  
   <reserved VQL words> ::=
       ADD | ALL | ALTER | AND | ANY | ARN | AS | ASC | BASE | BOTH | CALL 
     | CASE | CONNECT | CONTEXT | CREATE | CROSS | CURRENT_DATE | CURRENT_TIMESTAMP 
     | CUSTOM | DATABASE | DEFAULT | DESC | DF | DISTINCT | DROP | EXISTS | FALSE 
     | FETCH | FLATTEN | FROM | FULL | GRANT | GROUP BY | GS | HASH | HAVING 
     | HTML | IF | INNER | INTERSECT,INTO | IS | JDBC | JOIN | LDAP | LEADING 
     | LEFT | LIMIT | MERGE | MINUS | MY | NATURAL | NESTED | NOS | NOT | NULL 
     | OBL | ODBC | OF | OFF | OFFSET | ON | ONE | OPT | OR | ORDER BY | ORDERED 
     | PRIVILEGES | READ | REVERSEORDER | REVOKE | RIGHT | ROW | SELECT | SWAP 
     | TABLE | TO | TRACE | TRAILING | TRUE | UNION | USER | USING | VIEW | WHEN 
     | WHERE | WITH | WRITE | WS | ZERO
