================
INSERT Statement
================

The INSERT statement inserts one or more tuples in a view, updating
the underlying data source.


.. code-block:: bnf
   :caption: Syntax of the INSERT statement
   :name: Syntax of the INSERT statement

   INSERT INTO <view identifier> ( <field name> [, <field name> ]* ) 
       VALUES ( <value> [, <value> ]* )
       [ RETURNING <field name> [ , <field name> ]* ]
       [ CONTEXT ( <context information> [, <context information> ]* ) ]
       [ TRACE ]
       
   INSERT INTO <view identifier>
       VALUES ( <value> [, <value> ]* )
       [ RETURNING <field name> [, <field name> ]* ]
       [ CONTEXT ( <context information> [, <context information> ]* ) ]
       [ TRACE ]
       
   INSERT INTO <view identifier> [ ( <field name> [, <field name> ]* ) ]
       [ RETURNING <field name> [, <field name>]* ]
       VALUES ( <value> [, <value> ]* ) [, ( <value> [, <value> ]* ) ]*
       [ TRACE ]
       
   INSERT INTO <view identifier> 
       SET <field name> = <value> [, <field name> = <value> ]*
       [ RETURNING <field name> [, <field name>]* ]
       [ CONTEXT ( <context information> [, <context information> ]* ) ]
       [ TRACE ]
  
   <field name> ::= 
     <identifier>[.<identifier>]
   
   <value> ::=
       NULL
     | <number>
     | <boolean>
     | <literal>
     | <function>

..
   <view identifier> ::= (see :ref:`Basic primitives for specifying VQL
   statements`)

The section :ref:`Return Modified Rows` explains how the RETURNING clause works.

.. rubric:: Examples:

.. rubric:: *Add a row to the view internet_inc*

.. code-block:: sql

   INSERT INTO internet_inc (iinc_id, summary, taxid, specific_field1, specific_field2)
   VALUES (6, 'Error in ADSL Router', 'B78596015', '5', '6')

A row will be added in the source database to the table associated with the ``internet_inc``
view.


.. rubric:: *Alternative syntax to add a row to the view internet_inc*

.. code-block:: sql

   INSERT INTO internet_inc
   SET 
       iinc_id = 6
     , summary = 'Error in ADSL router'
     , taxid = 'B78596015'
     , specific_field1 = '5'
     , specific_field2 = '6'
   
.. rubric:: *Add several rows to a view*

.. code-block:: sql

   INSERT INTO employee (last_name, first_name, title) VALUES
       ('Callahan', 'Laura', 'IT Staff')
     , ('Edwards', 'Nancy', 'Sales Manager')
     , ('King', 'Robert', 'IT Staff');


