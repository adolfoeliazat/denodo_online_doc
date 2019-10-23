====================
GET_SOURCE_COLUMNS
====================

.. rubric:: Description

The stored procedure ``GET_SOURCE_COLUMNS`` returns information about the columns of the source of a JDBC base view. More specifically:

-  For a JDBC base view created over a table or view, it returns one row per column of the table/view of the database.
-  For a JDBC base view created over a SQL query, it returns one row per column projected by that query.
-  For a JDBC base view created over a stored procedure, it returns one column per parameter of the procedure.


To obtain this information, the procedure does not query the database. All the information is based on the metadata of the base view. Therefore, if a field was added to the table of a base view after the base view was created, this procedure will not return the new column. To update the metadata of a base view, you have to do a :ref:`source refresh <Source Refresh>` of the view. You can also detect changes on a view programmatically with the procedure :ref:`SOURCE_CHANGES`.

See also the procedure :ref:`GET_SOURCE_TABLE`.

.. rubric:: Syntax

.. code-block:: bnf

   GET_SOURCE_COLUMNS (
         input_database_name : text
       , input_view_name : text
   )

-  Both parameters are mandatory.

|

The procedure returns one row per column of the view:

-  ``database_name``: name of database where the base view belongs to.
-  ``view_name``: name of the view.
-  ``columns_name``: name of the column in the base view (see the field "source_column_name")
-  ``column_depth``: 1 for columns of simple types. For compound types, the depth of the field.
-  ``view_wrapper_name``: name of the JDBC wrapper of the base view.
-  ``source_catalog_name``: catalog of the table/view in the source database. ``null`` if the base view was created over a SQL query or over a stored procedure, or the database does not have the concept of catalog. 
-  ``source_schema_name``: schema of the table/view in the source database. ``null`` if the base view was created over a SQL query or over a stored procedure, or the database does not have the concept of schema.
-  ``source_table_name``: name of the table, view or procedure in the database. ``null`` for base views created over a SQL query.
-  ``sqlsentence``: SQL statement of the base view. ``null`` if the base view was not created over a SQL statement.
-  ``source_column_name``: if the base view was created over a table/view, name of the column in the source table. If the base view was created over a stored procedure, name of the parameter. If the base view was created over a SQL statement, alias of the column in the query. For SQL statements, if a column does not have an alias, the result depends on the database.
-  ``source_column_type_name``: name of the type of the column. E.g. ``VARCHAR``, ``NCHAR``, ``INTEGER``,
   ``BIGINT``, etc.

   These are the names of the constants defined by the JDBC API (class `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_).

-  ``source_column_type_id``: integer that represents the type of the field in the
   “source type properties” of the field. E.g. ``INT`` = ``4``, ``VARCHAR`` = ``12``, etc.

   The values of this field are defined by the JDBC API (class `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_). 

-  ``source_column_type_size``: its meaning depends on the type of the field:

   -  For columns that store text values, it indicates the maximum length of the
      field.
   -  For numeric types, the precision. For columns that store integers, 0.
      
-  ``source_column_type_decimals``: for columns that store numbers with decimals, it is the maximum number of decimals. 0, for other types of fields.

-  ``source_package_name``: name of the package that holds the stored procedure in the database. ``null`` if the procedure does not belong to any package or for base views that were not created over a stored procedure.
-  ``source_column_parameter_type``: the possible values are:

   -  1: the parameter's mode is IN.
   -  2: the parameter's mode is INOUT.
   -  4: the parameter's mode is OUT.
   -  0: the mode of the parameter is unknown.

|

The procedure returns an error if:

-  ``input_database_name`` or ``input_view_name`` do not exist.
-  The base view is not a JDBC base view.

.. rubric:: Privileges Required

The procedure returns an error if the user does not have the METADATA privilege over the base view.

.. rubric:: Example

.. code-block:: sql

   SELECT * 
   FROM GET_SOURCE_COLUMNS()
   WHERE input_database_name = 'hr'
   AND input_view_name = 'employees';
