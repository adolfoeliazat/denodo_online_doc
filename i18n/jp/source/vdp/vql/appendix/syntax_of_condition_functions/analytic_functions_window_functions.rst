=====================================
Analytic Functions (Window Functions)
=====================================

Analytic functions (also known as window functions) are functions whose
result for a given row is derived from the window frame of that row.


.. important:: Virtual DataPort can delegate these functions to a
   database, but cannot execute them. Therefore, if a query uses one of
   these functions and it cannot be delegated to a database, the query will
   fail.
   
   See :ref:`Workaround to Execute Analytic Functions`.


.. contents:: Analytic functions supported by Virtual DataPort
   :depth: 1
   :local:
   :backlinks: none
   :class: threecols


All the analytic functions have some syntax in common:

.. code-block:: bnf
   :caption: Analytic functions: common syntax
   :name: Analytic functions: common syntax

   <over clause> ::= 
     OVER ( [ <window partition clause> ] 
     [ <window order by clause> [ <windowing_frame_clause> ] ] )

   <window partition clause> ::= 
     PARTITION BY <expression>

   <window order by clause> ::= 
     ORDER BY <expression> [ ASC | DESC ]

   <window_frame_clause> ::=
       <window frame units>
       { <window_frame_preceding> | <window_frame_between> }

   <window frame units> ::=
       ROWS
     | RANGE

   <window_frame_between> ::=
       BETWEEN <window_frame_bound> AND <window_frame_bound>

   <window frame bound> ::=
       <window_frame_preceding>
     | <window_frame_following>

   <window frame preceding> ::=
       UNBOUNDED PRECEDING
     | <unsigned_value_specification> PRECEDING
     | CURRENT ROW

   <window frame following> ::=
       UNBOUNDED FOLLOWING
     | <unsigned_value_specification> FOLLOWING
     | CURRENT ROW

   <unsigned value specification> ::=
     <unsigned integer literal>


AVG
=================================================================================

.. rubric:: Description

The ``AVG`` function returns the average of an expression in a
partition.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

.. rubric:: Syntax

.. code-block:: bnf

   AVG ( <expression> ) OVER ( 
       [ <window partition clause> ] 
       [ <window order by clause> ] 
       [ <window_frame_clause> ] 
   ) : number

-  ``<expression>``: expression to calculate the average.
-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window_frame_clause>``: see `Analytic functions: common syntax`_.

COUNT
=====

.. rubric:: Description

The ``COUNT`` function returns the number of rows returned by the query.

.. important:: Virtual DataPort can push down this function to Oracle, Snowflake and SQL Server - **not to other databases** - 
   but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

.. rubric:: Syntax

.. code-block:: bnf

   COUNT ( 
       { 
           * 
         | <expression>
       } 
   ) OVER ( 
       [ <window partition clause> ]
       <window order by clause>
   ) : number

-  ``<expression>``: if you indicate an expression instead of ``*``, the function returns the number of rows where the expressions is not null.
-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.

CUME\_DIST
=================================================================================

.. rubric:: Description

The ``CUME_DIST`` function returns the cumulative distribution of the
current row with regard to other rows of the same partition.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

.. rubric:: Syntax

.. code-block:: bnf

   CUME_DIST() OVER ( 
       [ <window partition clause> ]
       <window order by clause>
   ) : number


-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.


DENSE\_RANK
=================================================================================

.. rubric:: Description

The ``DENSE_RANK`` function returns the rank of the row within the
window partition of the row, based on the order set by the
``<window order by clause>``.

The rows within a group are sorted by the ``ORDER BY`` clause and then,
the function returns a number for each row starting with 1 and going up.
The function returns the same value for the rows with equal values (null
values are considered equal).

The functions ``DENSE_RANK``, ``RANK`` (`RANK`_) and ``ROW_NUMBER``
(`ROW\_NUMBER`_) have some similarities. The differences are:

-  ``DENSE_RANK`` never skips a ranking and ``RANK`` does.
-  The values returned by ``ROW_NUMBER`` are always unique. ``RANK`` and
   ``DENSE_RANK`` return the same value to equal rows.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

.. rubric:: Syntax

.. code-block:: bnf
   
   DENSE_RANK( ) OVER ( 
       [ <window partition clause> ]
       <window order by clause>
    ) : numeric

-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.


FIRST_VALUE
=================================================================================

.. rubric:: Description

The ``FIRST_VALUE`` function returns the first value of a table or
partition.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

.. rubric:: Syntax

.. code-block:: bnf

   FIRST_VALUE( <expression> ) OVER ( 
       [ <window partition clause> ]
       [ <window order by clause> ]
       [ <window_frame_clause> ]
   ) : <type of the input expression>


-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window_frame_clause>``: see `Analytic functions: common syntax`_.


LAG
=================================================================================

.. rubric:: Description

The ``LAG`` function returns the value of an expression at a given
offset *before* the current row of a window.

Use the ``LEAD`` function (section :ref:`LEAD`) to obtain the value *after*
a given offset.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

.. rubric:: Syntax

.. code-block:: bnf

   LAG ( 
         <expression> 
       [ , <offset:number> 
       [, <default value:type of the input expression> ] ] 
   ) OVER ( 
       [ <window partition clause> ]
       <window order by clause>
   ): <type of the input expression>


-  ``expression``: expression to evaluate.
-  ``offset``: Number greater than 0 that indicates the lag. If not
   present, the offset is 1 (the previous row).
-  ``default value``: value returned if ``offset`` is outside the bounds
   of the partition. For example, if ``offset`` is 1 this function will
   return the default value for the first row of the partition.
-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.


LAST_VALUE
=================================================================================

.. rubric:: Description

The ``LAST_VALUE`` function returns the last value of a table or
partition.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

.. rubric:: Syntax

.. code-block:: bnf
   
   LAST_VALUE( <expression> ) OVER ( 
       [ <window partition clause> ]
       [ <window order by clause> ]
       [ <window_frame_clause> ]
   ) : <type of the input expression>


-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window_frame_clause>``: see `Analytic functions: common syntax`_.


LEAD
=================================================================================

.. rubric:: Description

The ``LEAD`` function returns the value of an expression at a given
offset *after* the current row of a window.

Use the ``LAG`` function (section :ref:`LAG`) to obtain the value *before* a
given offset.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.


.. rubric:: Syntax

.. code-block:: bnf
   
   LEAD( 
         <expression> 
       [ , <offset:number> 
       [, <default value:type of the input expression> ] ] 
   ) OVER ( 
       [ <window partition clause> ]
       <window order by clause>
   ): <type of the input expression>


-  ``expression``: expression to evaluate.
-  ``offset``: Number greater than 0 that indicates the lead. If not
   present, the offset is 1 (the following row).
-  ``default value``: value returned if ``offset`` is outside the bounds
   of the partition. For example, if ``offset`` is 1 this function will
   return the default value for the last row of the partition.
-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.


LISTAGG
=======

.. rubric:: Description

The ``LISTAGG`` function orders data within each group specified in the clause ``ORDER BY`` and then, concatenates the values of the measure column. It separates each value with the "delimiter expression".

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

.. rubric:: Syntax
   
.. code-block:: bnf
      
   LISTAGG ( 
         <measure expression> 
       , <delimiter expression:text> 
   ) WITHIN GROUP ( <window order by clause> ) 
     OVER ( [ <window partition clause> ] ) : text


-  ``measure expression``: expression.
-  ``delimiter expression``: string that will separate the measure values. If you do not want any separator between values, put an empty string (``''``).
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.

Returns a text value.

MAX
=================================================================================

.. rubric:: Description

The ``MAX`` function returns the maximum value of an expression within a
window.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

.. rubric:: Syntax

.. code-block:: bnf

   MAX ( <expression> ) OVER (
       [ <window partition clause> ]
       [ <window order by clause> ]
       [ <window_frame_clause> ] 
   ) : <type of the input expression>

-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window_frame_clause>``: see `Analytic functions: common syntax`_.


MIN
=================================================================================

.. rubric:: Description

The ``MIN`` function returns the minimum value of an expression within a
window.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

.. rubric:: Syntax

.. code-block:: bnf

   MIN ( <expression> ) OVER (
       [ <window partition clause> ]
       [ <window order by clause> ]
       [ <window_frame_clause> ]
   ) : <type of the input expression>


-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window_frame_clause>``: see `Analytic functions: common syntax`_.

NTILE
=================================================================================

.. rubric:: Description

The ``NTILE`` function divides an ordered data set (partition) into a
number of subsets within a window, with buckets (subsets) numbered 1
through <value>. For example, if <value> is 4, then each row in the
partition is assigned a number from 1 to 4. If the partition contains 40
rows, the first 10 would be assigned 1, the next 10 would be assigned 2,
and so on.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

.. rubric:: Syntax

.. code-block:: bnf
   
   NTILE ( <value> OVER ( 
       [ <window partition clause> ]
       <window order by clause>
   ): <number>


-  ``<value>``: number of subsets.
-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.


PERCENTILE_CONT
=================================================================================

.. rubric:: Description

The ``PERCENTILE_CONT`` function returns for each row, the value that
would fall into the specified percentile among the values in each
partition within a window.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.


.. rubric:: Syntax

.. code-block:: bnf
   
   PERCENTILE_CONT ( <percentile:number> ) 
       WITHIN GROUP ( <window order by clause> ) 
       OVER ( [ <window partition clause> ] )

-  ``percentile``: number between 0 and 1.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.


PERCENTILE_DISC
=================================================================================

.. rubric:: Description

The ``PERCENTILE_DISC`` function returns for each row, the value that
would fall into the specified percentile among the values in each
partition within a window.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

.. rubric:: Syntax

.. code-block:: bnf
   
   PERCENTILE_DIST ( <percentile:number> ) 
       WITHIN GROUP ( <window order by clause> ) 
       OVER ( [ <window partition clause> ] )

-  ``percentile``: number between 0 and 1.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.


PERCENT_RANK
=================================================================================

.. rubric:: Description

The ``PERCENT_RANK`` function returns the rank of a row relative to a
group of values within a window. It is similar to ``CUME_DIST``, but it
uses rank values rather than row counts in its numerator.

It returns a value between 0 and 1.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

.. rubric:: Syntax

.. code-block:: bnf
   
   PERCENT_RANK ( ) OVER (
       [ <window partition clause> ]
       <window order by clause> 
   ) : number


-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.


RANK
=================================================================================

.. rubric:: Description

The ``RANK`` function returns the rank of the row within the window
partition of the row, based on the order set by the
``<window order by clause>``.

The rows within a group are sorted by the ``ORDER BY`` clause and then,
the function returns a number for each row starting with 1 and going up.
The function returns the same value for the rows with equal values
(nulls are considered equal in this comparison).

The functions ``DENSE_RANK`` (`DENSE\_RANK`_), ``RANK`` and
``ROW_NUMBER`` (`ROW\_NUMBER`_) have some similarities. The differences
are:

-  ``DENSE_RANK`` never skips a ranking and ``RANK`` does.
-  The values returned by ``ROW_NUMBER`` are always unique. ``RANK`` and
   ``DENSE_RANK`` return the same value to equal rows.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.


.. rubric:: Syntax

.. code-block:: bnf
   
   RANK( ) OVER ( 
       [ <window partition clause> ]
       <window order by clause>
   ) : numeric

-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.


ROW_NUMBER
=================================================================================

.. rubric:: Description

The ``ROW_NUMBER`` function returns the rank of the row within the
window partition of the row, based on the order set by the
``<window order by clause>``.

The rows within a group are sorted by the ``ORDER BY`` clause and then,
the function returns a number for each row starting with 1 and going up.
The function returns the same value for the rows with equal values
(nulls are considered equal in this comparison).

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.

The functions ``DENSE_RANK`` (`DENSE\_RANK`_), ``RANK`` (`RANK`_) and
``ROW_NUMBER`` have some similarities. The differences are:

-  ``DENSE_RANK`` never skips a ranking and ``RANK`` does.
-  The values returned by ``ROW_NUMBER`` are always unique. ``RANK`` and
   ``DENSE_RANK`` return the same value to equal rows.


.. rubric:: Syntax

.. code-block:: bnf
   
   ROW_NUMBER ( ) OVER (
        [ <window partition clause> ] <window order by clause> 
   ) : numeric


-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.

**Example**



.. code-block:: sql

   SELECT key
       , amount
       , block
       , ROW_NUMBER( OVER ( ORDER BY key ) row_number
       , ROW_NUMBER() OVER ( PARTITION BY block ORDER BY key ) row_number_with_partition
       , RANK() OVER ( PARTITION BY block ORDER BY amount ) rank
       , DENSE_RANK() OVER ( PARTITION BY block ORDER BY amount ) dense_rank
   FROM VIEW
   ORDER BY block, key;
   

+-----+--------+-------+-------------+------------+--------+------------+-------------+
| key | amount | block | row\_number | row\_number\_with\_ | rank       | dense\_rank |
|     |        |       |             | partition           |            |             |
+=====+========+=======+=============+=====================+============+=============+
| 1   | 1      | 1     | 1           | 1                   | 1          | 1           |
+-----+--------+-------+-------------+---------------------+------------+-------------+
| 2   | 1      | 1     | 2           | 2                   | 1          | 1           |
+-----+--------+-------+-------------+---------------------+------------+-------------+
| 3   | 2      | 1     | 3           | 3                   | 3          | 2           |
+-----+--------+-------+-------------+---------------------+------------+-------------+
| 4   | 2      | 1     | 4           | 4                   | 3          | 2           |
+-----+--------+-------+-------------+---------------------+------------+-------------+
| 5   | 2      | 1     | 5           | 5                   | 3          | 2           |
+-----+--------+-------+-------------+---------------------+------------+-------------+
| 6   | 4      | 1     | 6           | 6                   | 6          | 3           |
+-----+--------+-------+-------------+---------------------+------------+-------------+
| 7   | 5      | 2     | 7           | 1                   | 1          | 1           |
+-----+--------+-------+-------------+---------------------+------------+-------------+
| 9   | 7      | 2     | 8           | 2                   | 2          | 2           |
+-----+--------+-------+-------------+---------------------+------------+-------------+
| 10  | 8      | 2     | 9           | 3                   | 3          | 3           |
+-----+--------+-------+-------------+---------------------+------------+-------------+
| 11  | 9      | 2     | 10          | 4                   | 4          | 4           |
+-----+--------+-------+-------------+---------------------+------------+-------------+


SUM
=================================================================================

.. rubric:: Description

The ``SUM`` function returns the sum of an expression in a group within
a window.

.. important:: Virtual DataPort can push down this function to a
   database, but cannot execute it. If a query uses this function and it
   cannot be pushed down to a database, the query will fail.


.. rubric:: Syntax

.. code-block:: bnf
   
   SUM ( <expression> ) OVER ( 
       [ <window partition clause> ] 
       [ <window order by clause> ] 
       [ <window_frame_clause> ] 
   ) : number

-  ``<expression>``: numeric expression.
-  ``<window partition clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window order by clause>``: see `Analytic functions: common
   syntax`_.
-  ``<window_frame_clause>``: see `Analytic functions: common syntax`_.

Workaround to Execute Analytic Functions
========================================

Virtual DataPort can delegate analytic functions to a database but cannot execute them. Therefore, if a query includes an analytic function and the execution engine cannot delegate it to a database that supports it, the query will fail immediately.

The most common scenarios where this occurs are:

#. When the underlying data sources of the query do not support analytic functions. For example, the data source is a REST API, an Excel file, etc.

#. Or if the execution engine cannot delegate these functions to a database that supports them. For example, if the query applies an analytic function over data coming from different data sources.

In these scenarios, use the :ref:`Data Movement` optimization to transfer the data to a database that supports analytic functions with the parameter ``DATAMOVEMENTPLAN`` of the ``CONTEXT`` clause. 

.. code-block:: vql 
   :caption: Example

   SELECT region.r_name, nation.n_name, sum(order.o_totalprice), rank() OVER (PARTITION BY region.r_name ORDER BY sum(order.o_totalprice) DESC)
   FROM order INNER JOIN customer ON order.o_custkey = customer.c_custkey
   INNER JOIN nation ON customer.c_nationkey = nation.n_nationkey
   INNER JOIN region ON nation.n_regionkey = region.r_regionkey
   WHERE region.r_regionkey > 0
   GROUP BY region.r_name, nation.n_name 
   CONTEXT(DATAMOVEMENTPLAN = 
       order : JDBC admin.vdpcachedatasource
       customer : JDBC admin.vdpcachedatasource
       nation : JDBC admin.vdpcachedatasource 
       region : JDBC admin.vdpcachedatasource
   ); 

In this example, the clause ``DATAMOVEMENTPLAN`` instructs the execution engine to move the data of the views involved in the query to the cache database (``admin.vdpcachedatasource``) and execute there the analytic function. The optimizer will try to delegate the WHERE conditions and other functions to the source databases to reduce the data that has to be transferred between the sources and the cache database. In this example, the SQL query executed in the source to obtain the data of ``region`` will include the condition ``r_regionkey > 0`` if the data source supports this condition.

If you cannot add the ``CONTEXT`` clause to the query because you are executing it from a third-party tool (e.g. a business intelligence tool), create a view with this clause and then, query the new view. For example:

.. code-block:: vql 

    CREATE VIEW rank_best_selling_nation_within_region AS 
    SELECT region.r_name, nation.n_name, sum(order.o_totalprice), rank() OVER (PARTITION BY region.r_name ORDER BY sum(order.o_totalprice) DESC)
    FROM order INNER JOIN customer ON order.o_custkey = customer.c_custkey
    INNER JOIN nation ON customer.c_nationkey = nation.n_nationkey
    INNER JOIN region ON nation.n_regionkey = region.r_regionkey
    WHERE region.r_regionkey > 0
    GROUP BY region.r_name, nation.n_name 
    CONTEXT(DATAMOVEMENTPLAN = 
        order : JDBC admin.vdpcachedatasource
        customer : JDBC admin.vdpcachedatasource
        nation : JDBC admin.vdpcachedatasource 
        region : JDBC admin.vdpcachedatasource
    ); 
