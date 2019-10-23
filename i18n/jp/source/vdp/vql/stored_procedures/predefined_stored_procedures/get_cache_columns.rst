============================
GET_CACHE_COLUMNS
============================

.. rubric:: Description

This procedure returns information about the columns of the table in the cache database that holds the cached data for a specific view.

You may also find useful the procedure :ref:`GET_CACHE_TABLE`.

.. rubric:: Syntax

.. code-block:: bnf

   GET_CACHE_COLUMNS( 
         input_database_name : text, 
       , input_view_name : text
   )
   
-  ``input_database_name``: name of the database.
-  ``input_view_name``: name of the view.
   
|

The procedure returns one row per each column of the view, with these fields:

-  ``database_name``: database of the view.
-  ``view_name``: name of the view.
-  ``column_name``: name of the field *in the view*.
-  ``cache_catalog_name``: catalog of the table that stores the cached data.
-  ``cache_schema_name``: schema of the table that stores the cached data.
-  ``cache_table_name``: name of the table that stores the cached data.
-  ``cache_column_name``: field in the table of the cache database. Usually, its value is the same as ``column_name``, but not always. They are different if ``column_name`` is a reserved word in the cache database. In that case, the cache engine creates the table using a different name for this field. This name is something like ``field_`` followed by a number.
-  ``cache_column_type_name``: name of the type of the field in the “source type
   properties” of the field. E.g. ``VARCHAR``, ``NCHAR``, ``INTEGER``,
   ``BIGINT``, etc.
   
   These are the names of the constants defined in the class
   `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_ of the JDBC API.
   
-  ``cache_column_type_id``: number that represents the type of the field in the cache database.

   The values of this field are defined in the class `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_ of the JDBC API. E.g. ``INT`` = ``4``, ``VARCHAR`` = ``12``, etc.
   
-  ``cache_column_type_size``: its meaning depends on the type of the field in the cache database:

   -  For fields of type ``text``, it indicates the maximum length of the
      field.
   -  For numeric types, it indicates the precision. Therefore, for ``int``
      and ``long`` fields, this number is ``0``.

-  ``cache_column_type_decimals``: number of fractional digits that a value of this
   field could store.
   
   