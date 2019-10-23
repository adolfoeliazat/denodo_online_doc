================================================
LIST_JDBC_DATASOURCE_TABLES
================================================

.. rubric:: Description

The stored procedure ``LIST_JDBC_DATASOURCE_TABLES`` returns the tables and views on the underlying database of a JDBC data source.

In combination with the procedure :ref:`GENERATE_VQL_TO_CREATE_JDBC_BASE_VIEW`, you can automate the process of having a base view for all the tables/views of a source database.

.. rubric:: Syntax

.. code-block:: bnf

   LIST_JDBC_DATASOURCE_TABLES (
       data_source_name : text
   )

-  ``data_source_name``: name of the data source for which you would like to get the list of tables.

|

The procedure returns one row for each table/view in the underlying database of the JDBC data source:

-  ``catalog_name``: name of catalog of given table.
-  ``schema_name``: name of schema of given table.
-  ``table_name``: name of given table.
-  ``type``: type of catalog of given table.

.. rubric:: Privileges Required

The user needs the Execute privilege over the data source.