================================================
GET_JDBC_DATASOURCE_TABLES
================================================

.. rubric:: Description

The stored procedure ``GET_JDBC_DATASOURCE_TABLES`` returns the list of tables/views of the underlying database of JDBC data source that match the search criteria.

In combination with the procedure :ref:`GENERATE_VQL_TO_CREATE_JDBC_BASE_VIEW`, you can automate the process of having
a base view for all the tables/views of a source database.

.. rubric:: Syntax

.. code-block:: bnf

   GET_JDBC_DATASOURCE_TABLES(
         input_datasource_name : text,
         input_catalog_name : text,
         input_schema_name : text,
         input_table_name : text,
         input_type : text
   )

-  ``input_datasource_name``: name of the data source for which you want to get the list of tables.
-  ``input_catalog_name`` (optional): name of the catalog for which you want to get the list of tables. If the data source does not support catalogs, set to ``null``. If ``null`` and the data source does support catalogs, the procedure will return all the matching tables across all catalogs.
-  ``input_schema_name`` (optional): name of the schema for which you want to get the list of tables. If the data source does not support schemas, set to ``null``. If ``null`` and the data source does support schemas, the procedure will return all the matching tables across all schemas.
-  ``input_table_name`` (optional): name of the table.
-  ``input_type`` (optional): type of element you want to find. If ``null``, it returns all types of elements. The possible values of this parameter depend on the underlying database. To know which values you can pass to this parameter, execute this procedure passing ``null`` to this parameter and see the values of the parameter "TYPE".

|

The procedure returns one row for each table/view in the underlying database of the JDBC data source that matches the search criteria:

-  ``catalog_name``: name of the catalog of given table.
-  ``schema_name``: name of the schema of given table.
-  ``table_name``: name of the given table.
-  ``type``: type of catalog of given table.

.. rubric:: Privileges Required

The user needs the privilege Execute over the data source.