==========================
Defining an Interface View
==========================

Interface views are a special type of derived views that consist only of
a definition of fields and a reference to another view. You can use them
to do a top-down design where you first define the fields of the
interface and later, you set the “implementation view” of the interface.

When the Server executes a query that involves the interface, the query
is delegated to the implementation view.

Interface views are created with the statement ``CREATE INTERFACE VIEW``.

.. code-block:: bnf
   :caption: Syntax of the CREATE INTERFACE VIEW statement
   :name: Syntax of the CREATE INTERFACE VIEW statement

   CREATE [ OR REPLACE ] INTERFACE VIEW <name:identifier>
       ( <field name:identifier> : <field type> 
         [ ,<field name:identifier> : <field type> ]* )
       [ SET IMPLEMENTATION <view identifier> 
           [ ( <field name:identifier> = <expression:value> 
               [, <field name:identifier> = <expression:value> ]* ) 
           ] 
       ]
       [ FOLDER = <literal> ]
       [ DESCRIPTION = <literal> ]

..

   <view identifier> ::= (see :ref:`Basic primitives for specifying VQL statements`)


For example,

.. code-block:: vql

   CREATE INTERFACE VIEW i_incidences (
       summary:text,
       ttime:date,
       taxid:text,
       inc_type:long,
   )
   SET IMPLEMENTATION incidences
   FOLDER = '/Interface views';


This statement creates an interface view with the fields ``summary``,
``ttime``, ``taxid`` and ``inc_type`` and sets ``incidences`` as the
implementation view (``SET IMPLEMENTATION`` clause). With this syntax,
the interface view (``i_incidents``) and the implementation view
(``incidents``) must have the same number of fields and with the same
name.

To create an interface view whose implementation view does not match the
definition of the interface, you can define an expression for each field
of the interface. For example,

.. code-block:: vql

   CREATE OR REPLACE INTERFACE VIEW i_incidents_test_delete (
       summary:text,
       ttime:date,
       taxid:text
       )
       SET IMPLEMENTATION incidents(
           summary = coalesce(summary, '<EMPTY>'),
           ttime = ttime,
           taxid = taxid    
   );

In this example, the ``summary`` field of the interface is mapped to an
expression instead of just a field. Also, the view ``incidents`` could
have more fields than ``summary``, ``ttime`` and ``taxid`` and they are
simply not taken into account in the definition of the interface.

After defining an interface, you can modify it to change its definition
and its implementation view. To do this, use the
``ALTER INTERFACE VIEW`` statement.

.. code-block:: bnf
   :caption: Syntax of the ALTER INTERFACE VIEW statement
   :name: Syntax of the ALTER INTERFACE VIEW statement

   ALTER INTERFACE VIEW <name:identifier>
       [ ( <field name:identifier> : <field type> 
           [, <field name:identifier> : <field type> ]* ) ]
       [ SET IMPLEMENTATION <view identifier> 
           [ ( <field name:identifier> = <expression:value> 
               [, <field name:identifier> = <expression:value> ]* ) 
           ] 
       ]

..

   <view identifier> ::= (see :ref:`Basic primitives for specifying VQL statements`)

For example,

.. code-block:: vql

   ALTER INTERFACE VIEW i_incidences (
         summary:text
       , ttime:date
       , taxid:text
   )
   SET IMPLEMENTATION another_view;

This statement removes the field ``inc_type`` from the definition of the
interface and changes the implementation view of the interface.

To delete an interface view use the statement ``DROP INTERFACE VIEW``
(see :ref:`Syntax of the DROP statement`).
