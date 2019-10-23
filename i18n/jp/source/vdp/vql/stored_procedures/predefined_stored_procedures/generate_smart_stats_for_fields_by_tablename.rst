==================================================
GENERATE_SMART_STATS_FOR_FIELDS_BY_TABLENAME
==================================================

.. rubric:: Description

The stored procedure
``GENERATE_SMART_STATS_FOR_FIELDS_BY_TABLENAME`` behaves as the
procedure ``GENERATE_SMART_STATS_FOR_FIELDS`` with the difference that
with ``GENERATE_SMART_STATS_FOR_FIELDS_BY_TABLENAME`` you can gather the
statistics of a base view from a table/view of the database that is not
the one that is linked to the base view. Use this procedure
to gather the statistics of a base view whose table in the database is
called ``customer_final`` but you want to obtain the statistics from the
table ``customer``.

This is the only difference between this procedure and
``GENERATE_SMART_STATS_FOR_FIELDS``.

.. rubric:: Syntax

.. code-block:: bnf

   GENERATE_SMART_STATS_FOR_FIELDS_BY_TABLENAME ( 
         mode : text
       , view_name : text
       , fields : array
       , table_stats : boolean
       , source_table_name : text
       , source_schema_name : text
       , source_catalog_name : text
   )

-  ``source_table_name``: name of the table from which you want to gather the
   statistics.
-  ``source_schema_name``: schema of the table from which you want to gather
   the statistics. Enter ``null`` if the database does not have schemas.
-  ``source_catalog_name``: catalog of the table from which you want to gather
   the statistics. Enter ``null`` if the database does not have
   catalogs.
