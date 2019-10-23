================
DELETE Statement
================

The DELETE statement deletes the tuples of a view that verify a certain
condition by updating the underlying data source.

.. code-block:: bnf
   :caption: Syntax of the DELETE statement
   :name: Syntax of the DELETE statement

   DELETE FROM <view identifier> [ WHERE <condition> ]
       [ CONTEXT ( <context information> [, <context information>]* ) ]
       [ TRACE ]

   <condition> ::=
         <condition> AND <condition>
       | <condition> OR <condition>
       | NOT <condition>
       | ( <condition> )
       | <value> <binary operator> <value> [ , <value> ]*
       | <value> <unary operator>

..
   <view identifier> ::= (see :ref:`Basic primitives for specifying VQL
   statements`)

For example, the following statement deletes the tuples of the
``internet_inc`` view where the value for the ``iinc_id`` attribute is
greater than 4:

.. code-block:: sql

   DELETE FROM internet_inc WHERE iinc_id > 4

As a result of executing this statement, the corresponding tuples in the
source database will be deleted in the table associated with the
``internet_inc`` view.

.. note:: This statement does not work with Microsoft Excel sources
   because of limitations in the Excel ODBC Driver provided by Microsoft
   Windows.

