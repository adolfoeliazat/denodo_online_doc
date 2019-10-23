============================
GET_CACHE_TABLE
============================

.. rubric:: Description

The stored procedure ``GET_CACHE_TABLE`` returns the name of the table in the cache database that holds the cached data for a specific view. It also returns additional information about the cache configuration of the view.

You may also find useful the procedure :ref:`GET_CACHE_COLUMNS`.

.. rubric:: Syntax

.. code-block:: bnf

   GET_CACHE_TABLE ( 
         input_database_name : text 
       , input_view_name : text
   )
   
-  ``input_database_name``: name of the database.
-  ``input_view_name``: name of the view.
   
|

The procedure returns one row with these fields:

-  ``database_name``: name of the database.
-  ``view_name``: name of the view.
-  ``view_type``: type of view. I.e. Base view, Select, Join, Interface,
   etc. When the type of the view is join, the value of this field includes
   the following information, in this order:

   -  Type of join: it can be “Inner”, “Left outer”, “Right outer” or “Full
      outer”.
   -  Join method: it can be “Any”, “Hash”, “Nested”, “Nested parallel” or
      “Merge”. “Any” means that the user did not select a method to execute
      the join when creating the view. Therefore, the Execution Engine will
      automatically select a method, unless one is selected in the
      “Execution plan” tab of the queried view.
   -  Order of the join: it can be “Any”, “Ordered” or “Reverseorder”.
      “Any” means that the user did not select an order to execute the join
      when creating the view. Therefore, the Execution Engine will
      automatically select an order, unless one is selected in the
      “Execution plan” tab of the queried view. 
-  ``cache_catalog_name``: catalog of the table that stores the cached data.
-  ``cache_schema_name``: schema of the table that stores the cached data.
-  ``cache_table_name``: table that stores the cached data.
-  ``effective_ttl``: Time To Live of the cached data. -1 if it is set to *Never expire*.
-  ``vdp_view_timezone``: time zone of the view.
-  ``cache_timezone``: time zone of the cache database.

It returns an error if the cache is not enabled on the view.

.. rubric:: Privileges Required

You need to have at least *Metadata* privilege over the view ``input_view_name``. If not, the procedure returns an error.