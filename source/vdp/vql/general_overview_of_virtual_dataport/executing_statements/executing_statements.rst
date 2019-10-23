====================
Executing Statements
====================

Once the creation stage has been completed, Virtual DataPort is ready to
accept queries and/or updates (updates can only be executed on
“updateable views” as defined by the SQL 92 standard). Details on how
external applications can send and execute statements on the Virtual
Database are provided in the :doc:`Developer Guide </vdp/developer/index>`. This section provides
only a general overview of the statement execution process from an
internal point of view.

When a VQL query is received, the Virtual DataPort query interpreter
starts by checking if the query capabilities of the views involved allow
obtaining an answer to the query. If the query cannot be answered to,
the user is informed of this. If it can, the process continues.

Subsequently, the *Plan Generator* creates the possible execution plans
for the query. The plans normally differ in aspects such as the
algorithm used to execute the joins or the specific search methods
selected on the sources.

The *optimizer* module is responsible for obtaining the cost of each of
the plans, according to different parameters, so that the best can be
selected. This process, among other tasks, is responsible for optimally
distributing processing between the Virtual DataPort server and the sources,
delegating operations such as groupby, selection conditions, joins or
unions, where possible. Hence, data transfer on the network can be
minimized. This stage is also responsible for tasks such as choosing the
most suitable method for implementing join operators, for establishing
the swapping to disk strategy for very large results or for managing use
of the *cache* module. See the section :ref:`Optimizing Queries` of the
Administration Guide for more details.

Once the optimum plan has been selected, the *Execution Engine* puts it
into practice. Execution of a plan assumes execution of a series of
sub-queries expressed in terms of the *base relations* only. These
sub-queries will be executed by the *wrapper* of the corresponding
source. It is remarkable how Virtual DataPort is capable of making
maximum use of parallelism, whereby sub-queries are normally executed in
parallel.

Finally, the execution engine combines the results returned by the
sources in accordance with that specified for each plan, thus obtaining
the final response to the query.

It is important to highlight that the system operates in an
*asynchronous* manner. This means that as the results of the sources
become available, the system begins to process them even if the sources
have not yet issued a complete response. This considerably speeds up the
times for obtaining the first tuples of the final result. Another
important aspect is that the system is capable of processing *partial
results*, i.e. it can process the query even if some of the sources are
temporarily inaccessible, providing the results that can be obtained
with the remaining sources while still informing the client application
of the errors.

Another fundamental aspect is that, optionally, as mentioned previously,
all or part of the source data can be pre-loaded in the server *cache*.
In this case, the system will check if the sub-query received can be
resolved with the data contained in the cache. If this is the case, the
response is obtained directly from same instead of querying the source.

Virtual DataPort also supports the executing of updating statements
(INSERT / UPDATE / DELETE) on views, provided these can be updated
according to the standard definition in SQL-92. See section :ref:`Inserts,
Updates and Deletes over Views` for further details.

