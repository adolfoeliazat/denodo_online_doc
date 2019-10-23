===================================
Automatic Simplification of Queries
===================================

.. toctree::
   :hidden:
   
   removing_redundant_branches_of_queries_partitioned_unions.rst
   pushing_down_group_by_views_below_join_views.rst
   pushing_down_group_by_views_below_union_views.rst
   selecting_the_most_optimal_source_when_the_data_is_replicated_in_several_sources.rst

The Execution Engine tries to simplify the queries before the actual
query execution takes place. The goal is to optimize the queries by
applying optimization techniques that are always beneficial. For
example, removing redundant operations, transforming outer joins to
inner joins when possible, etc. As these optimizations are always
applied, they are called static optimizations.

Users can provide their own hints to the optimizer, overriding the
generated plan, changing, for instance, the join execution method from
the “Execution plan” editor of the view.

This section describes the most important static optimizations.

The Automatic simplification of queries is enabled by default, but it
can be disabled in the “Queries optimization” dialog of the menu
“Administration > Server configuration” (see section :doc:`/vdp/administration/server_administration_-_configuring_the_server/queries_optimization/queries_optimization`).

To enable/disable this optimization for a specific database, click
**Database management** in the **Administration** menu, select the
database and click **Edit**. Then, select **Automatic simplification of
queries on** or **off** and click **Ok**. In this dialog, if “Automatic
simplification of queries” is **Default**, the database has the
configuration defined in the “Queries optimization” dialog.

These are some of the simplifications performed by Execution Engine:

-  Remove the branches of the query that are redundant: see section
   :ref:`Removing Redundant Branches of Queries (Partitioned Unions)`.
-  Reorder the joins of the query to increase the number of operations
   that are delegated to the sources. This optimization is not performed
   when the result is a cartesian product because usually, cartesian
   products are very inefficient.
-  When possible, transform outer join into inner joins. In certain
   scenarios, some outer joins can be simplified to inner joins, without
   changing the result.
-  Push down GROUP BY operations below join views: see section :ref:`Pushing
   Down GROUP BY Views below JOIN Views`.
-  Push down GROUP BY operations below union views: see section :ref:`Pushing
   Down GROUP BY Views below UNION Views`.

