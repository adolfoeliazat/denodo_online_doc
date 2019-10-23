================================================
GENERATE_VQL_TO_CREATE_JDBC_BASE_VIEW
================================================

.. rubric:: Description

The stored procedure ``GENERATE_VQL_TO_CREATE_JDBC_BASE_VIEW`` returns the VQL statements necessary to create a JDBC base view for a given table/view of the underlying database of a JDBC data source. Note that it does not actually creates the view, only returns the VQL statements to do so.

In combination with the procedure :ref:`GET_JDBC_DATASOURCE_TABLES`, you can automate the process of having a base view for all the tables/views of a source database.

Note that this procedure does not create the base view, it only returns the VQL statements to do so.

.. rubric:: Syntax

.. code-block:: bnf

   GENERATE_VQL_TO_CREATE_JDBC_BASE_VIEW (
         data_source_name : text
       , catalog_name : text
       , schema_name : text
       , table_name : text
       , base_view_name : text
       , folder : text
       , i18n : text
   )

-  ``data_source_name``: name of the data source.
-  ``catalog_name``: name of the catalog in the source database that contains the table from which to create the base view. Pass ``null`` if the database does not support catalogs.
-  ``schema_name``: name of the schema in the source database that contains the table from which to create the base view. Pass ``null`` if the database does not support schemas.
-  ``table_name``: name of the table/view over which to create the base view.
-  ``base_view_name``: name of the base view to be created. If ``null``, the name will be auto-generated.
-  ``folder``: folder in which to place the created base view. The result will include the VQL statements to create this folder(s). If ``null``, the VQL will not specify a folder.
-  ``i18n``: i18n of the base view to be created. If ``null``, the procedure will assign the i18n of the Virtual DataPort database to which the data source belongs. We recommend setting this to ``null``.

|

The procedure returns one row for each VQL statement necessary to create the base view:

-  ``creation_vql``: statements necessary to create the desired base view.

The procedure returns an error if the schema, catalog or table do not exist in the database.

.. rubric:: Privileges Required

The user must have execute privileges over the data source.