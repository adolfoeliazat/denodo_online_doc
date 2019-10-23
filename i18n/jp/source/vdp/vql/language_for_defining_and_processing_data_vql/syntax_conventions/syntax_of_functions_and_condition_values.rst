========================================
Syntax of Functions and Condition Values
========================================

As mentioned throughout this manual, there are different types of
functions in Virtual DataPort: aggregation functions and functions used
in conditions and to create derived attributes.

The listing :ref:`below <Rules for forming functions>` shows the syntax of these functions.



.. code-block:: bnf
   :caption: Rules for forming functions
   :name: Rules for forming functions

   <field name> ::= 
       <identifier>[.<identifier>]
     | <identifier>[.<identifier>]'['<integer>']' [ <compound field name> ]*
     | (<identifier>[.<identifier>])[<compound field name>]*

                  
   <compound field name> ::= 
     .<identifier> | '['<integer>']'
   
   <funcsymbol> ::= [\+\-\*\/\%]+
   
   <value> ::=
       NULL
     | <field name>
     | <number>
     | <boolean>
     | <literal>
     | <function>
     | <value> <funcsymbol> <value>
     | ( <value> )
     | <rowvalue>
     | { <rowvalue> [, <rowvalue>]* }
     | CASE <value> WHEN <compare_value:value> THEN <result:value> 
       [ WHEN <compare_value:value> THEN <result:value> ]* 
       [ ELSE <result:value>] END
     | CASE WHEN <condition> THEN <result:value> 
       [ WHEN <condition> THEN <result:value>]*
       [ ELSE <result:value> ] END
        
   <condition> ::= 
       <condition> AND <condition>
     | <condition> OR <condition>
     | NOT <condition>
     | ( <condition> )
     | <boolean>
     | <value> <unary operator>
     | <value> <binary operator> <value> [ , <value> ]*
     | <value> <binary operator> ( <value> [ , <value> ]* )
     | <value> BETWEEN <value> AND <value>
     | <value> CONTAINS <value>
     | <value> CONTAINSAND ( <value> [ , <value> ]* )
     | <value> CONTAINSOR ( <value> [ , <value> ]* )
     | <value> IN ( <value> [ , <value> ]* )
     | <value> LIKE <value> [ ESCAPE <escape character:literal> ]
     | <value> NOT BETWEEN <value> AND <value>
     | <value> NOT IN ( <value> [ , <value> ]* )
     | <value> NOT LIKE <value> [ ESCAPE <escape character:literal> ]
     | <value> REGEXP_LIKE <value>
     | <value> REGEXP_ILIKE <value>
     | <value> XMLEXISTS ( <XQuery expression:text>, <value:xml> )
     | XMLExists ( <XQuery expression:text>, 
       <ReadXQueryExpressionFromFile:boolean>, <value:xml>)
     | <value>
    
   <rowvalue> ::=
     ROW( <value> [, <value>]* )
                   
   <function> ::=
     <identifier> ( [ [ <function modifier>] <function parameter> 
     [, <function parameter>]* ] )

   <function parameter> ::=
       *
     | <value>
     | '[' [ <value>, [ <value> ]* ] ']'
   
   <function modifier> ::=
       ALL
     | DISTINCT

..

   <unary operator> ::= (see :ref:`Basic primitives for specifying VQL statements`)

   <binary operator> ::= (see :ref:`Basic primitives for specifying VQL statements`)


To define the syntax of a function we use the following elements:

-  The element ``<field name>`` defines the syntax for specifying an
   attribute of a view or base view. Note that attributes can be of
   compound types (see section :ref:`Management of Compound Values` for a
   detailed description of compound types).
-  The ``<value>`` element defines the syntax for any parameter of a
   function. They can be the name of an attribute, a number, a boolean
   or a literal constant. It is also possible to create a compound value
   using the ``ROW`` constructor (see section :ref:`Conditions with Compound
   Values`). As you can see, the parameter of a function can also be a
   new function. In addition, a ``<value>`` allows infix notations to be
   specified for a function (see the ``<value> <funcsymbol> <value>``
   rule).

A function element is defined as an identifier followed by a list of
parameters in brackets and separated by commas. The parameters of a
function can be ``*``, single valued (``<value>`` elements) or
multivalued (``<value>`` elements in square brackets and separated by
commas).

The syntax explained earlier is common for all types of functions
existing in Virtual DataPort. However, some peculiarities may exist for
a particular function type. These peculiarities, when they exist, are
mentioned in the section of the manual corresponding to each function
type.

Finally, it is important to remember that the format to be used to
represent date-type constants and other fields whose data type shows
internationalization characteristics when querying a view or base
relation is set by the internationalization configuration being used for
same. See section :ref:`Managing Internationalization Configurations`
for more information on the different internationalization configuration
parameters and section :ref:`Describing Catalog Elements` to find out how to
consult the parameters assigned to a specific internationalization
configuration.

