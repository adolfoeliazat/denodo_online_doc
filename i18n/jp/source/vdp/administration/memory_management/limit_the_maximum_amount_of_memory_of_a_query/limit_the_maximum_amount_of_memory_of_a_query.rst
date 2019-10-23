=============================================
Limit the Maximum Amount of Memory of a Query
=============================================

The execution of queries is multi-threaded. That is, for each query,
Virtual DataPort uses one thread for each data source involved in the
query, and one additional thread for combining the data obtained from
the sources.

As Virtual DataPort can process several queries concurrently, the memory
used by the queries can rise to significant levels, even when using
swapping. Also, because of the use of parallelism, several non-streaming
operations of the same query can be executed at the same time. For
instance, a query joining views from four different data sources, where
all joins are performed using the hash execution method, may execute up
to 3 non-streaming operations in parallel, which increases the
overall memory consumption.

The “Limit Query Memory Usage” parameter solves this potential problem
by setting a memory limit to the amount of memory a single query can
use. To check if it is enabled, open the **Server administration**, click 
the **Memory usage** tab and see if **Limit query memory usage on** is selected.

When this feature is enabled, the Execution Engine analyzes the
execution plan of the query to find its maximum number of non-streaming
operations that will be executed in parallel given the query topology
(this number is usually lower than the total number of non-streaming
operations in the plan). If the sum of the swap sizes assigned to those
operators - plus some minimum memory every query needs, depending on its
complexity - is higher than the value specified in the “Maximum Query
Size”, the Execution Engine automatically decreases the values of the
“Maximum size in memory of each node” and the “Maximum size in memory of
intermediate results” to ensure that the query execution will not
surpass the limit. The value of this property is only changed for the
execution of this query. It does not change the settings of the views,
the database or the Server.

In general, limiting the memory usage of queries is good because it
ensures no single query monopolizes the resources of the system.
Nevertheless, keep in mind that it may force a particular query to run
with less resources, even when the Server has available free memory.
Therefore, in scenarios where queries are predefined (so the amount of
memory they need will not vary abruptly) and they are scheduled to
ensure the level of concurrency does not cause memory problems,
deactivating this feature may improve the performance of the queries.

Regarding the value assigned to the “Maximum Query Size” parameter, it
can be adjusted in function of:

#. The maximum expected number of concurrent queries, that you estimate
   can use a significant amount of memory (recall that queries which use
   only streaming operations typically run on a very small memory
   footprint)
#. The available memory (consider always only 50% - 60% of the size of
   your real heap size to let space for other queries and for unexpected
   peaks).

For example, if you estimate Virtual DataPort will execute up to 20
concurrent queries that are complex, involve several non-streaming
operators (hash joins, hash group bys) that cannot be pushed down to the
data sources, and you have a real and reserved heap size of 8 gigabytes
for the JVM, then 200 megabytes is a reasonable value for this
parameter.

However, increasing the memory available for a query does not always
result in better performance. For instance, queries that use much memory
(even if they fit into the available memory space) can cause large
garbage collection cycles in the JVM. Also, when the host operating
system is running on a Virtual Machine whose memory is not actually
reserved (a bad practice you should avoid in any case), the
theoretically “available” memory may “not be there”, and the process to
acquire it may be costly.

In general, ensuring queries have a small memory footprint will
considerably increase the performance and stability of your application,
and you should only avoid this rule when benefits are clear and have
been tested. That is also why, no matter the size of the JVM’s heap, we
do not recommend setting this limit to a very high value (e.g. more than
1 GB).

Virtual DataPort does not guarantee that the memory limit per query will
be honored if the query meets one of the following conditions:

#. There is a subquery in the ``WHERE`` clause of the query.
#. The query has an aggregation function with the ``DISTINCT`` modifier.
   E.g.
   ``SELECT COUNT ( DISTINCT field1 ) ) FROM ...``
   This condition does not refer to queries where ``DISTINCT`` is used
   outside the aggregation function. E.g.
   ``SELECT DISTINCT field1 FROM ...``
