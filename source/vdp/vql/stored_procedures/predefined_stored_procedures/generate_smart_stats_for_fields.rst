===================================
GENERATE_SMART_STATS_FOR_FIELDS
===================================

.. rubric:: Description

The stored procedure ``GENERATE_SMART_STATS_FOR_FIELDS`` gathers *and*
stores the statistics of the fields of a JDBC base view (minimum value,
maximum value, etc. of each field) and the number of rows of the view.

To obtain these statistics, it queries the system tables of the
database, instead of executing a ``SELECT`` query with several
aggregation functions as the procedures ``GENERATE_STATS`` and
``GENERATE_STATS_FOR_FIELDS`` do. The benefit of this approach is that
obtaining the statistics of the view is much faster.

This procedure can only be used to gather the statistics of JDBC base
views. To gather the statistics of other types of views, use
``GENERATE_STATS`` (see :ref:`GENERATE_STATS`) or
``GENERATE_STATS_FOR_FIELDS`` (see :ref:`GENERATE_STATS_FOR_FIELDS`).

Before obtaining the statistics for a JDBC base view, make sure that the
source database (Oracle, IBM DB2, etc.) has the statistics of this
table. If the statistics are not present or are outdated, the statistics
gathered will not be accurate and the execution plans selected by the
cost-based optimizer could be suboptimal.

.. rubric:: Syntax

.. code-block:: bnf

   GENERATE_SMART_STATS_FOR_FIELDS (
         mode : text
       , view_name : text
       , fields: array
       , table_stats : boolean
   )


-  ``mode``: sets how the procedure gathers the statistics:

   -  ``SMART_ONLY``: the procedure gathers the statistics from the system
      tables of the database.
   
      This mode is equivalent to clearing the check box “Complete missing
      statistics…” in the “Statistics” tab of the view’s “Options”.
   
   -  ``SMART_THEN_ATSOURCE_THROUGH_VDP``: the procedure gathers the
      statistics from the system tables of the database. Then, it executes
      a ``SELECT`` statement to gather the statistics that it cannot not
      obtain from the system tables of the database.
   
      This mode is equivalent to selecting the check box “Complete missing
      statistics…” in the “Statistics” tab of the view’s “Options”.
   
   -  ``ATSOURCE_THROUGH_VDP_ONLY``: the procedure does not query the
      system tables. Instead, it just executes a ``SELECT`` statement. This
      mode is equivalent to executing the procedures ``GENERATE_STATS`` or
      ``GENERATE_STATS_FOR_FIELDS``. This option is not available from the
      administration tool.

-  ``view_name``: the procedure will gather the statistics of this view
   (with the same case).

-  ``fields``: array of registers with the fields whose statistics you want
   to gather. Each register has these fields:

   -  name of the field.
   
   -  ``stats``: array of statistics that you want to collect:
      ``FIELD_COUNT_DISTINCT`` (Distinct values), ``FIELD_AVG_LEN``
      (Average size), ``FIELD_MAX`` (Max value), ``FIELD_MIN`` (Min value)
      and ``FIELD_NUM_NULLS`` (Null values).
    
      If ``null``, it gathers all the statistics of the field.
      
   -  ``source_stat_name``: name of the object in the database that holds
      the statistics of this view.
    
      Some sources like Microsoft SQL Server, allow the user to generate
      multiple statistics over the same field or set of fields and store
      them in different objects.
    
      If ``null``, it obtains the statistics from the default object. This
      value is ignored when is not applicable.

-  ``table_stats``: if ``true``, the procedure obtains the number of rows
   of the table. If ``false``, it does not.


.. rubric:: Examples

.. rubric:: Example 1

.. code-block:: vql

   CALL GENERATE_SMART_STATS_FOR_FIELDS('SMART_ONLY'
       , 'internet_inc'
           , { 
           ROW('iinc_id',NULL,NULL),
           ROW('specific_field1',NULL,NULL),
           ROW('taxid',NULL,NULL)
           }
       , true)

It gathers all the statistics of the fields ``iinc_id``,
``specific_field1`` and ``taxid`` of the view ``internet_inc``. To do

this, it only queries the system tables of the database.

.. rubric:: Example 2

.. code-block:: vql

   CALL GENERATE_SMART_STATS_FOR_FIELDS('SMART_ONLY'
       , 'internet_inc'
           , { 
           ROW('iinc_id',NULL,NULL),
           ROW('specific_field1',NULL,NULL),
           ROW('taxid',NULL,NULL)
           }
       , false)

It does the same as the previous example, but it does not gather the
number of rows of the table (note the value of the latest parameter).

.. rubric:: Example 3

.. code-block:: vql

   CALL GENERATE_SMART_STATS_FOR_FIELDS('SMART_ONLY'
       , 'internet_inc'
           , { ROW('summary',
               { ROW('FIELD_NUM_NULLS'),
               ROW('FIELD_COUNT_DISTINCT'),
               ROW('FIELD_AVG_LEN') }, NULL)}
       , true);

It gathers the number of rows of the view and the statistics “Null
values”, “Distinct values” and “Average size” of the field ``summary``.

Because of the parameter ``SMART_ONLY``, to do this, it only queries the
system tables of the database.

