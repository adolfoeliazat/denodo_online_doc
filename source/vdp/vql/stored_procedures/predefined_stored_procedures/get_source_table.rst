==================
GET_SOURCE_TABLE
==================

.. rubric:: Description

The stored procedure ``GET_SOURCE_TABLE`` returns information about the source of a JDBC base view. More specifically:

-  For a JDBC base view created over a table or view, it returns the name of the table/view in the database, and its schema and catalog.
-  For a JDBC base view created over a SQL query, it returns that query.
-  For a JDBC base view created over a stored procedure, it returns the name of the procedure.

To obtain this information, the procedure does not query the database. All the information is based on the metadata of the base view. Therefore, if a table of a base view was moved to a different schema after the base view was created, this procedure will still return the old schema. To update the metadata of a base view, you have to do a :ref:`source refresh <Source Refresh>` of the view. You can also detect changes on a view programmatically with the procedure :ref:`SOURCE_CHANGES`.

If you also need to obtain information about the columns of the table/view in the database, see the procedure ``GET_SOURCE_COLUMNS``. 

.. rubric:: Syntax

.. code-block:: bnf

   GET_SOURCE_TABLE (
         input_database_name : text
       , input_view_name : text
   )

-  Both parameters are mandatory.

|

The procedure returns a single row with these fields:

-  ``database_name``: name of database where the base view belongs to.
-  ``view_name``: name of the base view.
-  ``view_type``: the value of this field is always ``BASE``.
-  ``view_wrapper_name``: name of the JDBC wrapper of the base view.
-  ``source_catalog_name``: catalog of the table/view in the source database. ``null`` if the base view was created over a SQL query or over a stored procedure, or the database does not have the concept of catalog. 
-  ``source_schema_name``: schema of the table/view in the source database. ``null`` if the base view was created over a SQL query or over a stored procedure, or the database does not have the concept of schema.
-  ``source_table_name``: name of the table, view or procedure in the database. ``null`` for base views created over a SQL query.
-  ``sqlsentence``: SQL statement of the base view. ``null`` if the base view was not created over a SQL statement.
-  ``is_procedure``: true if the view was created over a stored procedure of the database; false otherwise.
-  ``source_package_name``: name of the package that holds the stored procedure in the database. ``null`` if the procedure does not belong to any package or for base views that were not created over a stored procedure
-  ``specific_name``: unique identifier to distinguish overloaded stored procedures of the database. ``null`` if the identifier does not exist or in base views that were not created over a stored procedure.

|

The procedure returns an error if:

-  ``input_database_name`` or ``input_view_name`` do not exist.
-  If the base view is not a JDBC base view.

.. rubric:: Privileges Required

The procedure returns an error if the user does not have the METADATA privilege over the base view.

.. rubric:: Example

.. code-block:: sql

   SELECT * 
   FROM GET_SOURCE_TABLE()
   WHERE input_database_name = 'hr'
   AND input_view_name = 'employees';
