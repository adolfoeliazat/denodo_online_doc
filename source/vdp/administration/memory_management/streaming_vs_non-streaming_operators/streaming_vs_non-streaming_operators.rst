====================================
Streaming Vs Non-Streaming Operators
====================================

In general, data sources are accessed in “streaming” mode. This means
that Denodo retrieves data in blocks from the data sources as they are
needed, instead of loading in memory the entire result set returned by
the data source before start processing them. Many of the data
transformation and combination operations, such as some types of joins,
unions, selections and projections, are also performed in “streaming”
mode, considering only a few rows at a time.

When the data sources are accessed in streaming mode and their data is
combined using exclusively streaming operators, the memory consumption
of the query is minimal, no matter how many rows are processed. Even the
execution of a join between very large tables from different data
sources where Denodo processes many millions of rows consumes a minimal
amount of memory, when the join is executed using a streaming operator.

.. note:: The section :ref:`Edge Cases in Streaming Operation` describes
   several exceptions to this rule, where even streaming operations can
   consume a significant amount of memory.

There are some data transformation/combination operators that cannot be
performed in streaming mode. These non-streaming operators can cause a
query to consume a significant amount of memory when the number of rows
they need to process is large.

The non-streaming operators are:

#. *Hash joins*. Joins that use the hash execution method, load in
   memory all the rows from the view of the right side of the join.
   Notice that, if there are applicable selection conditions on that
   view, they will be applied before executing the join. However, if
   these conditions do not exist or are not very selective, it may still
   be needed to load a large number of rows in memory.
   
   The left view of the join is accessed in streaming mode, so its size does not significantly increase memory consumption. Notice that joins using other execution methods such as merge and nested can be run in streaming mode and, therefore, do not consume large amounts of memory. 
   
   The section :ref:`Optimizing Join Operations` explains how each join execution method works.
   
#. *Minus* and *Intersect* operations. These operations are executed by
   Denodo in a way similar to *hash* joins. Therefore, the same memory
   considerations apply to them.
#. Subqueries in the WHERE clause of the query that use the hash method.
   These subqueries are executed using the internal operator “semijoin”.
   This operator behaves similarly to joins and it admits three
   execution methods: hash, merge and nested.
   
   You can see the method used to execute a subquery in the execution trace of the query. 
   
   The section :ref:`Subqueries in the WHERE Clause of the Query` of the VQL Guide provides more information about each semijoin method and how to select the one you want.

   
#. *Order By*. Sorting is an inherently “non-streamable” operation: you
   need to load all data before producing the first row at the output.
   If the data to sort is large, this operation can consume a
   significant amount of memory.
#. *Group By*. Group by operations are executed in streaming mode when
   the data is sorted by the group by fields. In this case, the memory
   consumption is usually minimal, especially when the ``SELECT`` clause
   of the query only uses “cumulative” aggregation functions (i.e.:
   ``AVG``, ``COUNT``, ``SUM``, ``MAX`` or ``MIN``). Otherwise, Virtual
   DataPort uses the execution method “hash group by”, which may consume
   a significant amount of memory.
#. *SELECT DISTINCT*. The SELECT DISTINCT operation is executed in a way
   very similar to *Group By* operations. Therefore, the same
   considerations that apply to *Group By* operations also apply to them

The section :ref:`Limit the Maximum Amount of Memory of a Query` explains
how to limit the memory occupied by a query that uses these operators.

When a query uses non-streaming operators and in addition, it processes
a large number of rows, then the memory settings comes into play. The
next section explains the memory swapping parameters, which are used to
avoid that non-streaming operations exhaust the available memory.

