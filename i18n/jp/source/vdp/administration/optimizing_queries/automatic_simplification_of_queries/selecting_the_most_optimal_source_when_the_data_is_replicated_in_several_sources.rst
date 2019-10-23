================================================================================
Selecting the Most Optimal Source When the Data Is Replicated in Several Sources
================================================================================

When working with a star schema, occasionally the facts table is stored
in a database and its dimensions, in others. To increase the performance
of the queries, sometimes the dimensions are replicated on all the
databases that participate on the star schema. This allows applications
to perform the joins involving any of the dimensions in the same
database.

In Virtual DataPort you can configure a base view to indicate that the
same data is stored in different sources. This feature is called
“alternative sources” and it allows to benefit from the fact that the
data is replicated in several databases.

At runtime, when this base view is involved in a query, the optimizer
will select the source of the data of this base view that maximizes the
number of operations that can be pushed down to the source.

For example, let us say that:

-  There is a JDBC base view V1 whose table is in the database DB1.
-  There is a JDBC base view V2 whose table is in the databases DB2. In
   addition, the database DB1 contains an exact copy of the same table.
-  You define an alternative source for the view V2: the database DB1

When you execute a join of V1 and V2, the optimizer will realize that
the table of V2 is also present in the data source of V1 and will push
down the join of V1 and V2 to the database V1 instead of retrieving the
data from DB1 and DB2 and perform the join in Denodo.

.. important:: In order for the optimizer to apply this optimization you
   have to define the alternative sources of the base views when possible.
   The section :ref:`Alternative Sources` explains how to do it.

This feature is only available for JDBC base views.

