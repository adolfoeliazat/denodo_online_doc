================
UPDATE Statement
================

The UPDATE statement allows modifying the value of certain attributes in
all tuples of a view that verify a certain condition, directly updating
the underlying data source.

.. code-block:: bnf
   :caption: Syntax of the UPDATE statement
   :name: Syntax of the UPDATE statement

   UPDATE <view identifier> 
       SET ( <field name> [, <field name> ]* ) = ( <value> [, <value> ]* ) 
       [ WHERE <condition> ]
       [ RETURNING <field name>[, <field name> ]* ]
       [ CONTEXT ( <context information> [, <context information> ]* ) ]
       [ TRACE ]

   UPDATE <view identifier> 
       SET <field name> = <value> [, <field name> = <value> ]* 
       [ WHERE <condition> ]
       [ RETURNING <field name> [, <field name> ]* ]
       [ CONTEXT ( <context information> [, <context information> ]* ) ]
       [ TRACE ]
   
   <field name> ::= <identifier>[.<identifier>]
   
   <value> ::=
       NULL
     | <number>
     | <boolean>
     | <literal>
     | <field name>
     | <function>
   
   <condition> ::=
       <condition> AND <condition>
     | <condition> OR <condition>
     | NOT <condition>
     | ( <condition> )
     | <value> <binary operator> <value> [ , <value> ]*
     | <value> <unary operator>

..

   <view identifier> ::= (see :ref:`Basic primitives for specifying VQL statements`)

The section :ref:`Return Modified Rows` explains how the RETURNING clause works.

.. rubric:: Examples:

.. rubric:: Example #1

The following statement updates the tuples of the
``internet_inc`` view whose value of ``iinc_id`` is ``6``, setting to
``10`` its value for the attributes ``specific_field1`` and
``specific_field2``:

.. code-block:: sql

   UPDATE internet_inc
   SET (specific_field1, specific_field2) = ('10', '10')
   WHERE iinc_id = 6
   
As a result of executing this statement, the corresponding tuples in the
source database will be altered in the table associated with the

``internet_inc`` view.

.. rubric:: Example #2

It is also possible to use the alternative syntax:

.. code-block:: sql

   UPDATE internet_inc 
   SET specific_field1 = '10', specific_field2 = '10' 
   WHERE iinc_id = 6

