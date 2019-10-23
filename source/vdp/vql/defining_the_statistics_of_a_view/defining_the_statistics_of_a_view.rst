=================================
Defining the Statistics of a View
=================================

The Cost-based optimization of Virtual DataPort uses the statistics of
the views and information about the indexes in the data source to
estimate the cost of alternative execution plans for a query and select
the one with the lower estimated cost. See more about this in the
section :ref:`Cost-based Optimization` of the Administration Guide.

We recommend using the Administration Tool to gather and store the
statistics of the views. But you can also enter these statistics
manually with the ``CREATE VIEWSTATSUMMARY`` statement.

This section lists the statements to manage the statistics of base views
*and* derived views.

To store the statistics of a view, use the command
``CREATE VIEWSTATSUMMARY``.


.. code-block:: bnf
   :caption: Cost-based optimization: syntax of CREATE VIEWSTATSUMMARY
   :name: Cost-based optimization: syntax of CREATE VIEWSTATSUMMARY

   CREATE [ OR REPLACE ] VIEWSTATSUMMARY <view name:identifier> (
       [ SET ENABLED = <boolean> ]
       [ SET NUMOFROWS = <number of rows of the view: long> ]
       [ SET STATS <statistics table> ]
   )
   
   <statistics table> ::= 
       <statistics header> VALUES <field statistics> [, <field statistics> ]*
   
   <statistics header> ::= ( FIELDNAME, <property name> [, <property name> ]* )
   
   <property name> ::= 
       AVGSIZE 
     | MAXVALUE 
     | MINVALUE 
     | NUMOFDISTINCTVALUES 
     | NUMOFNULLVALUES
   
   <field statistics> ::= 
       ( <field name : identifier>, <property value> [, <property value> ]* )
   
   <property value> ::= 
       NULL 
     | <value : number> 
     | <value : literal>
   

For example:

.. code-block:: vql
   :caption: Cost-based optimization: example of CREATE VIEWSTATSUMMARY
   :name: Cost-based optimization: example of CREATE VIEWSTATSUMMARY

   CREATE OR REPLACE VIEWSTATSUMMARY internet_inc (
       SET ENABLED = true
       SET NUMOFROWS = 4125
       SET STATS (fieldname, avgsize, maxvalue, minvalue, numofdistinctvalues, numofnullvalues) VALUES 
         (iinc_id, 8.0, 4, 1, 4, 0),
         (summary, 21.0, 'Install aditional line', 'Bandwidth increase', null, 40),
         (ttime, 186.0, null, '2005-06-30T04:19:41+0200', null, 0),
         (taxid, 9.0, null, 'B78596011', null, 1238),
         (specific_field1, 1.0, '3', '1', 3, 2133),
         (specific_field2, 1.0, '3', '1', 3, 998)
   );
      
If ``ENABLED`` is ``true``, the “Execution Engine” will use the
statistics of this view to estimate the cost of executing this query. If
``false``, these statistics will not be used.

The ``STATS`` clause indicates the statistics of each field provided in
the ``VALUES`` clause:

-  ``AVGSIZE``: for fields of type ``text``, this is the average length
   of this field’s values. For fields of other types, it is the average
   size in bytes.
-  ``MAXVALUE``: maximum value of the field.
-  ``MINVALUE``: minimum value of the field.
-  ``NUMOFDISTINCTVALUES``: number of distinct values in this field.
-  ``NUMOFNULLVALUES``: number of ``NULL`` values in this field.

If you do not want/cannot provide a statistic for a field, enter
``NULL``. E.g. it may not be possible to provide the size of ``blob``
values.

To modify the existing statistics of a view, use the
``ALTER VIEWSTATSUMMARY`` statement:



.. code-block:: vql
   :caption: Cost-based optimization: ALTER VIEWSTATSUMMARY statement
   :name: Cost-based optimization: ALTER VIEWSTATSUMMARY statement

   ALTER VIEWSTATSUMMARY <view name:identifier> (
       [ SET ENABLED = <enabled : boolean> ]
       [ SET NUMOFROWS = <num of rows : long> ]
       [ SET STATS <statistics table> ]
   )

..

   <statistics table> ::= (see :ref:`Cost-based optimization: syntax of
   CREATE VIEWSTATSUMMARY`)


Use the following commands to manage the statistics of views:

-  ``LIST VIEWSTATSUMMARIES``: returns a list of views for which there
   are statistics.
-  ``DESC VQL VIEWSTATSUMMARY <view name>``: returns the statistics of a
   view.
-  ``DROP VIEWSTATSUMMARY <view name>``: deletes the statistics of a
   view.



