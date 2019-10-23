==========================================================
Current Limitations of the Cost-Based Optimization Process
==========================================================

The cost-based optimization process can be only applied to a query when
at least one of the following two conditions is met:

-  There are statistics for all the base views involved in the query.
-  Or, there are statistics for the leaf views of the query, even if
   they are not base views. Leaf views are those views that are entirely
   pushed down to a data source.

**Example**: Let us suppose ``V1`` and ``V2`` are base views from the
same database and ``V3`` is a base view from another data source. Let us
also suppose ``DV`` is a derived view obtained by joining ``V1`` and
``V2``. ``DV`` is a leaf view because ``V1`` and ``V2`` are from the
same database and Virtual DataPort pushes down join operations to
databases.

Let us suppose we execute the following query:

.. code-block:: sql

   SELECT * 
   FROM DV JOIN V3 ON DV.a = V3.a

This query can benefit from the cost-based optimization when any of the
following conditions hold:

-  We have gathered the statistics of ``V1``, ``V2`` and ``V3``
-  Or, we have gathered the statistics of ``DV`` and ``V3``

When the cost-based optimization cannot be applied because the
statistics of a required view are lacking or are disabled, the execution
trace contains a message like “Missing statistics or not activated in
the following nodes: DV, V3”

In addition, you have to take into account the following limitations of
the cost-based optimization process:

-  This optimization is not applied in queries involving subqueries in
   the ``WHERE`` clause.
-  This optimization is not applied to queries using the ``QUERYPLAN``
   context option.
-  Although possible, we do not recommend enabling the cost-based
   optimization in views obtained from Multidimensional Databases,
   Google Search data sources and ITPilot data
   sources. The cost-based optimization process makes certain
   assumptions that are not verified in those types of data sources.
   Therefore, cost estimations may produce unexpected results.
-  The n-joins involving any left, right or full outer join will not be
   reordered. Virtual DataPort will still try to find the best execution
   plan for each 2-join in the n-join, but it will execute them in the
   order specified by the query.
