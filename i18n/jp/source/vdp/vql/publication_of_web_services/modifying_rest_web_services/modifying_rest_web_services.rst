==============================
Modifying REST Web Services
==============================

To modify a REST web service use command ALTER REST WEBSERVICE. With this statement, you can add, replace or drop views from a web service.


.. code-block:: bnf
   :caption: Syntax of the ALTER REST WEBSERVICE statement
   :name: Syntax of the ALTER REST WEBSERVICE statement

   ALTER REST WEBSERVICE <name:identifier>
       SET VIEW <view identifier>
           FIELDS ( <fields list> [ <fields' mappings> ] )

   ALTER REST WEBSERVICE <name:identifier>
       SET ASSOCIATIONS ( <association name:identifier with database>
                      [, <association name:identifier with database> ]* ) ]

   ALTER REST WEBSERVICE <name:identifier>
       DROP VIEW [ IF EXISTS ] <view identifier>

   <fields list> ::= <field> [, <field> ]*

..

   <view identifier> ::= (see :ref:`Basic primitives for specifying VQL statements`)

   <identifier with database> ::= (see :ref:`Basic primitives for specifying VQL statements`)

   <field> :::= (see :ref:`Creating REST Web Services`)

To add or replace a view, use the ``SET VIEW`` clause. In the ``FIELDS`` clause, put the identifier of the fields of the view. You do not have to publish all the fields of the view.

To remove a view, use the ``DROP VIEW`` clause.

To add one or more association, use clause ``SET ASSOCIATIONS`` followed by a comma-separated list of the identifiers of the associations you want the service to publish.
