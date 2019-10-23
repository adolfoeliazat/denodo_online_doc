=====================
DROP_REMOTE_TABLE
=====================

.. rubric:: Description

The ``DROP_REMOTE_TABLE`` stored procedure deletes a base view and the remote table accessed by the base 
view of a JDBC data source (Oracle, PostgreSQL, DB2, Hive, Impala, Redshift, etc.).

.. note:: This stored procedure only allows dropping base views created with the :ref:`CREATE_REMOTE_TABLE` stored procedure.

.. rubric:: Syntax

.. code-block:: bnf

   DROP_REMOTE_TABLE(
         base_view_database : text
       , base_view_name : text
       , drop_base_view_on_cascade : boolean
   )

-  ``base_view_database_name`` (optional): VDP database where is the base view that will be deleted. If the value
   of this parameter is ``null`` then the procedure will use the VDP database to which the user is connected.
-  ``base_view_name``: Name of the base view that will be deleted.
-  ``drop_base_view_on_cascade`` (optional): If the value is ``true``, then the base view dependent elements will be deleted. The default value is ``false``.


.. rubric:: Stored procedure result

Two or three rows with the result of each step of the procedure. Example of execution with ``drop_base_view_on_cascade = false``:

+----------------------------------------------------------------------------------------------------------+
|Step 1 of 2: Remote table '\<remote table name\>' dropped successfully.                                   |
+----------------------------------------------------------------------------------------------------------+
|Step 2 of 2: Deleted base view '\<base view name\>' successfully in the '\<vdp database name\>' database. |
+----------------------------------------------------------------------------------------------------------+

Example of execution with ``drop_base_view_on_cascade = true``:

+----------------------------------------------------------------------------------------------------------+
|Step 1 of 3: Remote table '\<remote table name\>' dropped successfully.                                   |
+----------------------------------------------------------------------------------------------------------+
|Step 2 of 3: Deleted base view '\<base view name\>' successfully in the '\<vdp database name\>' database. |
+----------------------------------------------------------------------------------------------------------+
|Step 3 of 3: Deleted base view dependent elements.                                                        |
+----------------------------------------------------------------------------------------------------------+


.. rubric:: Privileges Required
        
The user must have the following privileges to be able to execute the `DROP_REMOTE_TABLE` procedure:

- ``Connect`` privilege in the VDP database where is the base view.
- ``Write`` privilege in the base view.
- ``Write`` privilege in the base view dependent elements.
- ``Connect`` privilege in the VDP database where is the data source of the base view.
- ``Execute`` privilege in the data source of the base view.

Furthermore, the user used in the VDP data source to connect to the database must have ``DROP TABLE`` privileges.


.. rubric:: Example

This example uses a VDP database that contains a base view called ``vdp_base_view`` that was created with the :ref:`CREATE_REMOTE_TABLE` stored procedure.

.. code-block:: sql

   SELECT *
   FROM DROP_REMOTE_TABLE()
   WHERE base_view_database = 'customer360_db'
       AND base_view_name = 'customer'
       AND drop_base_view_on_cascade = true;

In this example, the procedure will perform these steps:

1. Deletes the remote table accessed by the base view ``customer``.
2. Deletes the base view ``customer``.
3. Deletes the base view dependent elements.

