============
CATALOG_PKS
============

.. note:: This stored procedure is deprecated and it may be removed in the next
   major version of the Denodo Platform. Use the procedure :ref:`GET_PRIMARY_KEYS` instead of this one because "GET_PRIMARY_KEYS" can search on any database, not just in the one you are connected to and it returns the same information.

.. rubric:: Description

The stored procedure ``CATALOG_PKS`` returns the list of fields that
make up the primary key of a view, or of all the views of a database.

.. rubric:: Syntax

.. code-block:: bnf

   CATALOG_PKS (
       input_view_name : text
   )


-  ``input_view_name`` (optional): name of the view for which you want
   to obtain the list of primary key fields. You need to be connected to
   the database of this view.
   
   If ``null``, it returns the list of primary key fields of all the views of the database that you are currently connected to.

The procedure returns one row per field that makes up the primary key of
each view. The output schema has the following fields:

-  ``database_name``: database that the view belongs to.
-  ``view_name``: name of the view.
-  ``column_name``: field name of the primary key.
-  ``pk_name``: the name of the primary key. This is the name of the
   view followed by “\_pk”.

.. rubric:: Privileges Required

The procedure only returns information about the views over which the
user has the Metadata privilege.

.. rubric:: Example

Let us say that there is a view ``order_line`` whose primary key is made
up of the fields ``order_id`` and ``order_line_id``.

If you execute the following query:

.. code-block:: vql

   SELECT database_name, view_name, column_name, pk_name
   FROM CATALOG_PKS()
   WHERE input_view_name = 'order_line';

The result will be this:

+--------------------+--------------------+--------------------+--------------------+
| database\_name     | view\_name         | column\_name       | pk\_name           |
+====================+====================+====================+====================+
| admin              | order\_line        | order\_id          | order\_line \_pk   |
+--------------------+--------------------+--------------------+--------------------+
| admin              | order\_line        | order\_line\_id    | order\_line \_pk   |
+--------------------+--------------------+--------------------+--------------------+
