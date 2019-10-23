===================
GET_VIEW_STATISTICS
===================

.. rubric:: Description

The stored procedure ``GET_VIEW_STATISTICS`` returns the statistics of a view. Each row of the result represents the statistics of the field of the view.

If the statistics of the view have not been gathered yet, this procedure returns 0 rows. You can gather the statistics of a view using the stored procedure :ref:`GENERATE_SMART_STATS_FOR_FIELDS` or from the administration tool, in the dialog *Options* > *Statistics* of the view.

.. rubric:: Syntax

.. code-block:: bnf

   GET_VIEW_STATISTICS (
         input_database_name : text
       , input_name : text       
   )      

-  ``input_database_name`` (mandatory): database of the view.
-  ``input_name`` (mandatory): name of the view.

The procedure returns one row per field of the view, with these fields:

-  ``database_name``: name of database of the view.
-  ``name``: name of the view.
-  ``field_name``: name of the field. 
-  ``rows_number``: number of rows of the view. This value is the same for all the rows of the view.
-  ``average_size``: for text fields, the average length; for fields of other type, average size in bytes.
-  ``min_value``: minimum value of the field.
-  ``max_value``: maximum value of the field.
-  ``distinct_values``: number of distinct values for the field.
-  ``null_values``: number of null values for the field.

.. rubric:: Remarks

This procedure returns 0 rows in three situations:

1. If the user does not have the privilege EXECUTE over the view.
#. If the view does not exist.
#. If the statistics of the view have not been gathered.

.. rubric:: Privileges Required

This procedure only returns information about a view if the user has the EXECUTE privilege over the view. Otherwise, it does not return any row.

.. rubric:: Examples

.. rubric:: Example 1

.. code-block:: sql

    SELECT field_name, rows_number, average_size, min_value, max_value, distinct_values, null_values
    FROM GET_VIEW_STATISTICS()
    WHERE input_database_name = 'customer_report'
        AND input_name = 'invoice'        

This query obtains the statistics of all the fields of the view "invoice" (database "customer_report").