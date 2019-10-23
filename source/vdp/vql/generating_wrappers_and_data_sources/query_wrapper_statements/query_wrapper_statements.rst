========================
QUERY WRAPPER Statements
========================

Virtual DataPort allows directly querying wrappers (without having to
define base relations on them). To do it, use the statement ``QUERY WRAPPER``. 
You have to provide the type and name of the
name and optionally, a list of conditions in the format
``<value>, binary operator, <value>`` (see the syntax of condition
values in the section :ref:`Syntax of Functions and Condition Values`). Unary
operators and multivalued binary operators are not allowed.

.. code-block:: bnf
   :caption: Syntax of the QUERY WRAPPER statement
   :name: Syntax of the QUERY WRAPPER statement

   QUERY WRAPPER { ARN | CUSTOM | DF | ESSBASE | GS | ITP | JDBC | JSON | LDAP | 
                   ODBC | OLAP | SALESFORCE | SAPBWBAPI | SAPERP | WS | XML } <name:identifier>
       [ 
         ( <value> <binary operator> <value>
        [, <value> <binary operator> <value> ]* 
         ) 
       ]

.. 

   <value> ::= (see section :ref:`Syntax Conventions`)


See :ref:`below <Syntax of the QUERY WRAPPER ITP (WWW) statement>` the syntax to query WWW wrappers.
This syntax is slightly different than for the other types of wrappers. Only a list of ``key = value`` pairs can be indicated,
which will be directly received by the wrapper as input parameters.



.. code-block:: bnf
   :caption: Syntax of the QUERY WRAPPER ITP (WWW) statement
   :name: Syntax of the QUERY WRAPPER ITP (WWW) statement

   QUERY WRAPPER ITP <name:identifier>
   [
       (
           <name:identifier> = <value:literal>
           [, <name:identifier> = <value:literal>]*
       )
   ]
