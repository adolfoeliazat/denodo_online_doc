=========================
Queries: SELECT Statement
=========================


.. toctree::
   :hidden:

   from_clause/from_clause.rst
   with_clause/with_clause.rst
   select_clause/select_clause.rst
   where_clause/where_clause.rst
   group_by_clause/group_by_clause.rst
   having_clause/having_clause.rst
   union_clause/union_clause.rst
   order_by_clause/order_by_clause.rst
   offset_fetch_and_limit/offset_fetch_and_limit.rst
   context_clause/context_clause.rst
   trace_clause/trace_clause.rst
   case_clause/case_clause.rst

To query a view, use the SELECT statement. The following subsections describe the use of each of the clauses of the
``SELECT`` statement.

The syntax of all the VQL statements can be obtained by executing the command HELP (see section :ref:`Help Command`).

.. code-block:: bnf
   :name: Syntax of the SELECT statement
   :caption: Syntax of the SELECT statement
   
   <query> ::=
     [ WITH <common table expressions> ]
     { <select> | <complex select> }
     [ <order by> ]
     [ <result offset clause> ]
     [ <fetch first clause> ]
     [ CONTEXT ( <context information> [ , <context information> ]* ) ]
     [ TRACE ]
   
   <select> ::=
     SELECT [ DISTINCT ] <select fields>
     FROM <view> [ , <view> ]*
     [ WHERE <condition> | <subselect condition> ]
     [ GROUP BY <group by field> [ , <group by field> ]*
       [ HAVING <condition> ]
     ]
   
   <complex select> ::=
       <complex select> [ { UNION [ ALL ] | MINUS | INTERSECT } 
                            <complex select> ]+
     | SELECT [ DISTINCT ] <select fields> 
         FROM ( <complex select> [ <order by> ] )
     | ( <complex select> )
     | <select>
   
   <select fields> ::=
     <select field> [ [ AS ] <alias:identifier> ]
     [, <select field> [ [ AS ] <alias:identifier> ] ]*
   
   <select field> ::= 
       * 
     | <value>
     | <value> = <value>
   
   <common table expressions> ::=
     <common table expression> [ , <common table expression> ]*
   
   <common table expression> ::=
     <query name:view identifier>
     [ ( <field name:identifier> [ , <field name:identifier> ]* ) ]
     AS ( { <select> | <complex select> }
     [ <order by> ] )
   
   <result offset clause> ::=
     OFFSET <offset row count:number> [ ROW | ROWS ]
   
   <fetch first clause> ::=
       FETCH { FIRST | NEXT } [ <fetch first quantity:number> ] { ROW | ROWS } ONLY
     | LIMIT [ <number> ]
     
   <view> ::=
       <simple view>
     | <join view>
     | ( <select> )

   <simple view> ::=
       <view:identifier> [ [ AS ] <alias:identifier> ]
     | [ <database name:identifier> .]<procedure:identifier>
       ( [ <procedureParameter> [, <procedureParameter> ]* ] )
       [ [ AS ] <alias:identifier> ]
     | <flatten view>
   
   <join view> ::=
       <inner view1:view> [ <method type> ] [ <order type> ] [ <join type> ]
       JOIN <inner view2:view> ON <condition>
     | <inner view1:view> NESTED PARALLEL [ <order type> ] [ <join type> ]
       JOIN [ <parallel number:integer> ] <inner view2:view> ON <condition>
     | <inner view1:view> [ <method type> ] [ <order type> ]
       NATURAL [ <join type> ] JOIN <inner view2:view>
     | <inner view1:view> NESTED PARALLEL [ <order type> ]
       NATURAL [ <join type> ] JOIN [ <parallel number:integer> ]
       <inner view2:view>
     | <inner view1:view> [ <method type> ] [ <order type> ] [ <join type> ]
       JOIN <inner view2:view> USING ( <field> [ , <field> ]* )
     | <inner view1:view> NESTED PARALLEL [ <order type> ] [ <join type> ]
       JOIN [ <parallel number:integer> ] <inner view2:view>
       USING ( <field> [ , <field> ]* )
     | <inner view1:view> CROSS JOIN <inner view2:view>
   
   <inner view> ::=
       <simple view>
     | ( <view> )
   
   <join type> ::= 
       LEFT [ OUTER ] 
     | RIGHT [ OUTER ] 
     | FULL [ OUTER ] 
     | INNER
   
   <method type> ::= 
       HASH 
     | NESTED 
     | MERGE
   
   <order type> ::= 
       ORDERED 
     | REVERSEORDER
   
   <join condition> ::=
       <simple join condition> [ AND <simple join condition> ]*
     | ( <join condition> )
   
   <simple join condition> ::=
       <field1:field name> <binary operator> <field2:field name>
     | <field2:field name> <binary operator> <field1:field name>
   
  
   <subselect condition> ::=
       <condition>
     | <subselect condition> AND <subselect condition>
     | <subselect condition> OR <subselect condition>
     | NOT <subselect condition>
     | <value> <binary operator> [ ALL | ANY ] 
       ( <select> | <complex select> [ <order by> ] )
     | <value> [ NOT ] IN ( <select> | <complex select> [ <order by> ] )
     | [ NOT ] EXISTS ( <select> | <complex select> [ <order by> ] )
   
   <column names> ::= 
     <column name> [, <column name> ]*  
   
   <column name> ::=
       <identifier>
     | <literal>
   
   <group by field> ::= 
       <field name> 
     | <expression> }
   
..

   <order by> ::= (see :ref:`Syntax of the ORDER BY clause (SELECT statement)`)
   
   <flatten view> ::= (see :ref:`Syntax of the FLATTEN operation`)
   
   <view identifier> ::= (see :ref:`Basic primitives for specifying VQL statements`)

   <condition> ::= (see :ref:`Rules for forming functions`)
   
   <unary operator> ::= (see :ref:`Basic primitives for specifying VQL statements`)
   
   <binary operator> ::= (see :ref:`Basic primitives for specifying VQL statements`)
   
   <field name> ::= (see :ref:`Rules for forming functions`)
   
   <context information> ::= (see :ref:`Syntax of the CONTEXT clause`)
   
   <value> ::= (see :ref:`Rules for forming functions`)