====================
RESTful Architecture
====================

This section explains how to:

#. Define associations between views.
#. With ``SELECT_NAVIGATIONAL``, execute queries that allow “navigating”
   through the associations defined between views.

These two elements, along with the primary key support of views, are
part of the REST architecture of the Denodo Platform.

The main feature of this architecture is the RESTful Web service, which
is described in the section :ref:`RESTful Web service` of the Administration
Guide. This service allows clients to browse through the contents of
Virtual DataPort using an HTTP interface.

Associations
============

An association represents a relationship between two views in a similar
way a join view links one view with another one. The section
:doc:`/vdp/administration/restful_architecture/associations/associations` of the Administration Guide explains this concept in more
detail.

This section explains the VQL statements to create, modify and delete
associations.



.. code-block:: bnf
   :caption: Syntax of the CREATE ASSOCIATION statement
   :name: Syntax of the CREATE ASSOCIATION statement

   CREATE [ OR REPLACE ] ASSOCIATION <identifier with database> [ REFERENTIAL CONSTRAINT ]
       [ FOLDER = <folder:literal> ]
       [ DESCRIPTION = <association_description:literal> ]
       ENDPOINT <role_name:identifier> <view_name:identifier>   
                [ <mult:multiplicity> ]
       [ PRECONDITION <condition> ]
       [ DESCRIPTION = <endpoint_description:literal> ]
   
       ENDPOINT <role_name:identifier> <view_name:identifier> 
                [<mult:multiplicity>]
       [ PRECONDITION <condition> ]
       [ DESCRIPTION = <endpoint_description:literal> ]
       [ ADD MAPPING <val1:mapping_value> = <val1:mapping_value> ]+ 
   
   <mapping_value> ::=
         <field name>
       | <mapping_value> <funcsymbol> <value>
       | <value> <funcsymbol> <mapping_value>
       | CASE <mapping_value> 
             WHEN <compare_value:value> THEN <result:value> 
             [ WHEN <compare_value:value> THEN <result:value> ]* 
             [ ELSE <result:value> ] END
       | CASE WHEN <condition> THEN <result:value> 
              [ WHEN <condition> THEN <result:value> ]* 
              [ ELSE <result:value> ] END
   <multiplicity> ::= 
         ( 0 , 1 )
       | ( 1 )
       | ( * )
       | ( + )

..

   <identifier with database> ::= (see :ref:`Basic primitives for specifying VQL
   statements`)

   <value> ::= (see :ref:`Rules for forming functions`)

   <condition> ::= (see :ref:`Rules for forming functions`)


The clause ``REFERENTIAL CONSTRAINT`` marks the association as a
referential constraint (see the section :ref:`Referential Integrity in
Associations` of the Administration Guide)

The first ``DESCRIPTION`` clause is the description of the association
and the second and the third ones are the descriptions of the two
endpoints of the association.

The clause ``PRECONDITIONS`` represent the *Role preconditions* of each
end point (see the section :ref:`Role Preconditions` of the Administration
Guide).



.. code-block:: bnf
   :caption: Syntax of the ALTER ASSOCIATION statement
   :name: Syntax of the ALTER ASSOCIATION statement

   ALTER ASSOCIATION <name:identifier>
       [ RENAME <new_name:identifier> ]
       [ DESCRIPTION = <desc:literal> ]


Use the ``ALTER ASSOCIATION`` statement to rename the association and/or
change its description.

To delete an association, execute the statement ``DROP ASSOCATION`` (see
:ref:`Syntax of the DROP statement`)



Navigational Queries
====================

With ``SELECT_NAVIGATIONAL`` you can execute queries that allow you to
“navigate” through the associations defined between views.

If you want to identify a particular row of a view with its primary key
and then, obtain the rows of another view that are linked with the rows
of the other view, you can do it in two ways:

-  With a join of two views.
-  Or defining an association between these views and at runtime, use
   ``SELECT_NAVIGATIONAL`` to traverse the association.

Defining an association provides several benefits over doing a join:

-  The queries are much simpler.
-  The client does not need to know the name of the view at the other
   side of the association.
-  The client does not need to know how to build a valid join condition
   that links the two views.
-  Any change on the view at the other side of the association or the
   condition of the association does not affect clients.

For example, if there is an association between the views “customer” and
“order”, an application can execute the statement
``SELECT_NAVIGATIONAL`` to obtain all the orders of a particular
customer without having to know the names of the fields of the join
condition or the name of the view that holds the customers’ orders.

The drawback of doing this with an association instead of a join is that
joins allow you to link two views in a more complex way.

.. code-block:: bnf
   :caption: Syntax of the SELECT_NAVIGATIONAL statement (navigational queries)
   :name: Syntax of the SELECT_NAVIGATIONAL statement (navigational queries)

   SELECT_NAVIGATIONAL <navigation fields>
       FROM <view:identifier> [ <select navigations> ]
       [ EXPAND <expand elements> ]  
       [ WHERE <extended condition> ]
       [ GROUP BY <group by expression> [ , <group by expression> ]* ]
       [ HAVING <condition> ]
       [ ORDER BY <order by expression> [ ASC | DESC ] [, <order by expression> [ ASC | DESC ] ]* ]
       [ WITH CONDITION MAPPINGS EVALUATION ]
       [ FLATTEN FIRST LEVEL ROLES ]
       [ OFFSET <number> [ ROW | ROWS ] ]
       [ {
             FETCH { FIRST | NEXT } [ <number> ] { ROW | ROWS } ONLY
           | LIMIT [ <number> ]
         }
       ]
       [ CONTEXT ( <context information> [ , <context information> ]* ) ]
       [ TRACE ]

   <navigation fields> ::= 
     <navigation field> [, <navigation field> ]*

   <navigation field> ::= 
       * 
     | <field name:identifier> [ AS <alias:identifier> ]
     | <role name:identifier> / *
     | <role name:identifier> / <field name:identifier>
     | <aggregation function name:identifier> ( <function parameter> ) [ AS <alias:identifier> ] 
     | <expression>
   
   <function parameter> ::=
         *
     | <field name:identifier>
     | <role name:identifier> / <field name:identifier>
   
   <select navigations> ::= 
         / <primary key values>  
     | / [ <condition> ] 
     | { / <primary key values> / <association name:identifier> }* 
              [ / <primary key values> | / LBRACE <condition> RBRACE ]
   
   <primary key values> ::= 
       <primary key value:value> [, <primary key value:value> ]*
   
   <expand elements> ::= 
     <expand element> [ , <expand element> ]*
   
   <expand element> ::= 
     <role name:identifier> [ / <role name:identifier> ]*
   
   <group by expression> ::= 
       <field name:identifier>
     | <role name:identifier> / <field name:identifier> 
     | <expression>
   
   <order by expression> ::= 
       <expression>
     | <field name>
     | <field position:integer> 
     | <role name:identifier> / <field name:identifier>

   <extended condition> ::=
       <regular condition:condition>
     | (condition with references to <navigation field>s)


The most important part of this statement is the navigational expression
of the ``FROM`` clause. These expressions identify a collection of one
or more elements on which to apply the query. A collection may be a
view, a row of a view or the result of traversing an association from
one view to another one.

The clauses ``WHERE``, ``HAVING`` and ``ORDER BY`` support the use of navigation fields (e.g. role_customer/customer_name) 
provided that they navigate to an endpoint with cardinality 1.

To use the division operator in a "navigation field" or expression, use ``DIV``. ``/`` is not allowed in this context other than to traverse a role.

.. note:: An association can only be traversed if the view has a primary
   key defined. The section :ref:`Primary Keys of Views`
   of the Administration Guide explains how to define the primary key of a
   view using the Administration Tool. The :ref:`Syntax of the statement
   CREATE TABLE` and the :ref:`Syntax of the CREATE VIEW statement`
   contain the syntax of the VQL statements to define the primary key when
   creating base and derived views.

Let us say we have this:

#. The view ``customer``, whose primary key is the field ``cid``.
#. The view ``order``, whose primary key is the field ``oid``.
#. An association between them called ``customer_orders``, whose role
   name on the view ``customer`` is called ``final_orders``.

Consider the following examples of valid navigational expressions:

+--------------------------------------+--------------------------------------+
| Query                                | Explanation                          |
+======================================+======================================+
| SELECT\_NAVIGATIONAL \*              | Returns all the rows of              |
| FROM customer                        | ``customer``.                        |
+--------------------------------------+--------------------------------------+
| SELECT\_NAVIGATIONAL \*              | Returns the rows with                |
| FROM customer / 1234                 | ``cid = 1234``.                      |
|                                      |                                      |
|                                      | Note that the client does not need   |
|                                      | to know the name of the fields that  |
|                                      | form the primary key.                |
+--------------------------------------+--------------------------------------+
| SELECT\_NAVIGATIONAL \*              | Returns all orders by customer with  |
| FROM customer / 1234 / final\_orders | ``cid = 1234``.                      |
|                                      |                                      |
+--------------------------------------+--------------------------------------+
| SELECT\_NAVIGATIONAL \*              | Returns all the products sold in     |
| FROM customer / 1234 / final\_orders | the order ``oid = 123`` made by the  |
| / 123 / products                     | the customer with ``cid = 1234``.    |
+--------------------------------------+--------------------------------------+
| SELECT\_NAVIGATIONAL \*              | Returns all orders that match the    |
| FROM customer / 1234 / final\_orders | condition ``field <> 'value'``       |
| / [ field <> 'value' ]               | and that were made by the            |
|                                      | customer with ``cid = 1234``.        |
+--------------------------------------+--------------------------------------+

The remaining clauses (``SELECT``, ``WHERE``, ``ORDER BY``, ``FETCH``
and ``OFFSET``) are used in the same way as in the ``SELECT`` statement.

**Example**

Consider a view ``customer`` whose primary key is the field ``cid``.
This view participates in an association whose role on the endpoint of
the ``customer`` view is called ``orders``.

.. code-block:: vql

   SELECT_NAVIGATIONAL *
   FROM customer / 2 / orders
   WHERE o_description like '%Bandages%'

This query returns the orders of the customer with ``cid = 2`` and whose
description starts with ``Bandage``.

The equivalent ``SELECT`` query is more complex. E.g.:

.. code-block:: sql

   SELECT orders.*
   FROM customer INNER JOIN orders
   ON customer.cid = orders.cid
   WHERE customer.cid = 2 AND orders.o_description like '%Bandages%'

|

``SELECT_NAVIGATIONAL`` queries return some extra fields in addition to
the ones specified in the ``SELECT`` clause:


-  One additional field for each association of the view. The value of this
   field is a register containing:

   -  Name and description of the involved role.
   -  A path expression relative to the one used in the ``FROM`` clause
      that allows to traverse the association for that row.

-  If the view has the primary key defined, the result will have the field
   ``_primary_key``.

   It provides a path expression that uniquely identifies each row of the
   result. The values of this field are registers with the same structure
   as the association fields. In this case the value of the ``rel``
   attribute is always ``self`` and the path expression is absolute.
