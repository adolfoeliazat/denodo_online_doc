==========================
Optimizing Join Operations
==========================

A key aspect of query optimization in Virtual DataPort is choosing the
most suitable method for join operations. Virtual DataPort will select a
join method based on internal cost data. However, you can force a
specific join method that you consider is most appropriate.

An execution strategy for a join consists of two elements: the *method*
used to implement the join operation and the *order* in which the join
input views are considered. Virtual DataPort supports the following
execution methods:

-  Merge join: see section `Merge Join`_
-  Nested join: see section `Nested Join`_
-  Nested parallel join: see section `Nested Parallel Join`_
-  Hash join: see section `Hash Join`_

If a query or a view forces the use of this strategy in a scenario where
it is not applicable, the Server will return an error message.

Merge Join
==========

The merge method is very often the fastest way and with the lowest
memory footprint of joining two views that are sorted by the fields of the join condition.

When the execution engine executes a merge join, it reads a row from each input view and evaluates the join condition.
If the condition is met, the join returns the row. Otherwise, the row with the smaller value for the fields 
of the join condition is discarded because, as both views are sorted, the discarded row will not match any of the next rows of the other view.

The execution engine repeats this process until all the rows of one of the tables have been processed. Even if the other view still has rows, there is no need to process them because they will not match with any other row.

The merge join can only be used when the input views of the join are
sorted by the join attributes. The execution engine obtains the data sorted in the following cases so it can do a merge join: 

-  The underlying views of the JOIN have an ORDER BY that sorts the data by the fields of the join conditions.

-  If the data comes from a JDBC or ODBC base view created from a table/view and it meets *all* these conditions:

   i. The Source Configuration property “Supports binary ORDER BY collation” of the views is "yes".
   
      .. important:: If the value of this property is not "yes" by default, do not set it 
         to "yes". If the value of this property is "no" by default, it means that
         the source is not capable of sorting the data using a binary collation 
         so the data will not be ordered as expected by the Server even if the query sent to 
         the database has the ORDER BY. This could lead to returning incorrect results.
      
      The section :ref:`ORDER BY Properties of the Source Configuration` explains 
      in more detail the meaning of this property.

   #. The views have the same i18n.
  
   If all these conditions are met, the Execution Engine adds the clause ORDER BY 
   to the query sent to the database.
   
-  The execution engine cannot select merge if the data comes from a JDBC or ODBC base view created from a query *and* for some reason you had to set to "false" the option "Delegate SQL sentence as subquery" of the base view's "wrapper source configuration". However, it can select merge if the SQL query of the base view has an ORDER BY clause that sorts the data by the attributes of the join condition. If this is the case, add the attributes of the ORDER BY clause to the property “Fields by which the data is sorted in the source” of the view. After doing this, the execution engine will be able to select merge.
  
-  If the data is obtained from non-relational data sources and they
   return the data sorted by the fields involved in the join condition.
   If this is the case, set the value of the property “Fields by which
   the data is sorted in the source” of the base view. This property
   indicates that the data of the base view is sorted by these fields.

The section :ref:`View Configuration Properties` explains how to set the property “Fields by which
the data is sorted in the source” of a view.

If the execution engine cannot select the method merge, it will use hash, unless the cost-based optimization is enabled (see section
:ref:`Cost-Based Optimization`) and it considers that executing the join with nested join method is more efficient.

During the execution of a merge join, the execution engine will stop obtaining tuples from a branch when it knows that the remaining rows of that branch will not match with any row of the other branch.

Nested Join
===========

The Nested join method, first, queries the input view of the left side
of the join, to obtain the tuples that verify the join condition. Then,
it executes a query to the view of the right side, for each combination
of values obtained for the field that takes part in the join.

If the view of the right side of the join retrieves the data from a
database (from a JDBC or an ODBC data source), Virtual DataPort will
retrieve the data from this view “in blocks”, instead of executing a
query for each row obtained from the first view.

To retrieve the data “in blocks” it executes a query with the syntax
selected in the property “Nested join optimization syntax” of the data
sources’ Source configuration. Its values can be:

-  Use OR clause
-  Use IN clause
-  Use WITH clause
-  Use subquery

The values available for this property depend on the database. For
example, for data sources that connect to Oracle, you can only select
“Use Or clause” or “Use IN clause”.

This property is changed in the “Source Configuration” tab of the “Edit
Data Source” dialog.

**Example**:

Let us say that we have defined the view ``internet_inc_join_phone_inc``
as:

.. code-block:: sql

   CREATE VIEW internet_inc_join_phone_inc
   AS
   SELECT ...
   FROM internet_inc 
   NESTED ORDERED INNER JOIN 
   phone_inc 
   ON internet_inc.iinc_id = phone_inc.pinc_id


And that the values of the field ``iinc_id`` of the view
``internet_inc`` are: ``1``, ``2``, ``3`` and ``4``.

|

If the value of the property “Nested join optimization syntax” is “Use
OR clause”, the Server executes the following query in the database to
retrieve the data of the view ``phone_inc``:

.. code-block:: sql

   SELECT ...
   FROM phone_inc t0
   WHERE ((t0.pinc_id = 1) OR (t0.pinc_id = 2) OR (t0.pinc_id = 3) OR
   (t0.pinc_id = 4))

|

If the value of this property is “Use IN clause”, the Server executes
the following query to retrieve the data of the view ``phone_inc``:

.. code-block:: sql

   SELECT *
   FROM phone_inc t0
   WHERE t0.pinc_id IN (1, 2, 3, 4)

|

If the value of this property is “Use WITH clause”, the Server executes
the following query to retrieve the data of the view ``phone_inc``:

.. code-block:: sql

   WITH a0 AS ( SELECT a0.c0 FROM TABLE (VALUES 1, 2, 3, 4) AS a0 (c0) )
   SELECT *
   FROM phone_inc t0 INNER JOIN a0 ON (t0.PINC_ID = a0.c0)

|

If the value of this property is “Use subquery”, the Server executes the
following query to retrieve the data of the view ``phone_inc``:

.. code-block:: sql

   SELECT *
   FROM phone_inc t0
   INNER JOIN TABLE (
       SELECT a0.c0
       FROM TABLE ( VALUES ( 1, 2, 3, 4 ) ) AS a0(c0)
       ) a0 ON (t0.PINC_ID = a0.c0)

If the amount of values obtained from the left-side view of the join (in
this example ``internet_inc``) exceeds a certain value, the Server will
execute several queries to obtain the data from the right-side view (in
this example ``phone_inc``).

|

If you have created the right side input view using a SQL statement (see
section :ref:`Creating Base Views from SQL Queries`), the SQL statement must
have the interpolation variable ``WHEREEXPRESSION`` (see section :ref:`Using
the WHEREEXPRESSION Variable`) so that Virtual DataPort can use this
optimization option.

Nested Parallel Join
====================

The Nested parallel join method is similar to the Nested method. The
difference is that the subqueries issued to the view of the right side
of the join are issued in parallel instead of one after the other. It
accepts an additional parameter that specifies the maximum number of
subqueries issued in parallel.

If the data from the right-side view is obtained from a database, Nested
parallel is usually less efficient than Nested because with Nested, the
Server will optimize the process by generating a single subquery that
retrieves all required data.

Hash Join
=========

The Hash join method is often the most efficient when the data in the
input views is not ordered and is large. It is also often the most
effective when the query latency times for the data sources are high
(e.g. Web sources), as this type of join minimizes the number of
sub-queries made to the sources.
