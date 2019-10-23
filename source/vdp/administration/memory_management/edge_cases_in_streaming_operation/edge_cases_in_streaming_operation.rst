=================================
Edge Cases in Streaming Operation
=================================

As explained in the section :ref:`Streaming Vs Non-Streaming Operators`,
when the data is combined using exclusively streaming operators (such as
merge and nested joins, unions, selections, projections, etc.), the
memory consumption of the query should be minimal, no matter how many
rows need to be processed by Denodo. Nevertheless, there are some edge
cases where memory consumption can be significant:


#. Non-streaming data sources. Some data sources do not support returning
   data in “streaming mode”. In these cases, Denodo has to store in memory
   all the results returned by the data source. If the returned number of
   rows is high, this can result in significant memory consumption. Most
   data sources that can return large amounts of data (e.g. databases)
   support streaming in a way or another to avoid forcing their client
   applications to store large result sets. To avoid memory overflows,
   ensure you use streaming always when possible.


#. Large rows. In streaming operators, Denodo processes result sets row by
   row, so typically only a few rows need to be in memory at the same time.
   Nevertheless, if rows are extremely large, then the query still consumes
   a significant amount of memory. This may be the case when a row contains
   complex hierarchical data (remind that Denodo has the capability to
   represent complex hierarchical data using the array and the register
   data types).

   For instance, by default, the Denodo XML data source creates one
   single row containing the full result of a query (you can latter use
   the flatten operation to divide it into several rows). If you think a
   certain XML data service will return large documents, use the “Tuple
   Root” option in the view creation wizard to divide the document in
   several rows, avoiding this problem. The section :ref:`XML Sources`
   explains how to do this.

   There are similar options in other data sources where it is common to
   return complex hierarchical data, such as JSON web services and SOAP
   Web Services


#. Slow data sources (or slow client applications). As explained in the
   section :ref:`Streaming Vs Non-Streaming Operators`, Denodo retrieves data
   from the data sources involved in a query in parallel. Suppose Denodo is
   joining two views A and B, and the data comes from the data sources
   already sorted by the join attributes (so the join can be performed
   using a streaming operator). Suppose also that the data source A is much
   faster than data source B. Then, the thread of the data source A will
   tend to accumulate rows in memory, since they cannot be processed until
   the matching rows of B arrive. To avoid the number of accumulated rows
   to grow excessively, Denodo limits the maximum amount of memory that can
   be used to accumulate rows waiting to be processed, through the “Maximum
   size in memory of intermediate results” parameter. This ensures that, no
   matter how slow is the other data source, memory usage will not grow out
   of control.

   By default, Denodo uses a relatively small value for the “Maximum size
   in memory of intermediate results” parameter, (the mechanism to limit
   the maximum memory consumed by a query explained in the section :ref:`Limit
   the Maximum Amount of Memory of a Query` can also automatically
   decrease it even more, if it thinks it is needed). In general, you do
   not need to modify this value, but increasing it can improve performance
   in very specific cases. Here is an example:
   
   a. Suppose we have a join view V between two views A (from data source
      DS1, which is a SaaS application like Salesforce) and B (from data
      source DS2, which is a database in the same network as the Virtual
      DataPort server).
   b. B pushes down a very complex aggregation query on the DS2 database
      and A executes a relatively simple query on DS1.
   c. Since B pushes down a complex aggregation query to DS2, suppose that
      DS2 does not begin returning results until 30 seconds after the query
      was executed (the time required by the database to compute the
      results). After the first results are returned, the remaining results
      will come very fast, since the database is in the same network as the
      Virtual DataPort server.
   d. In turn, the results from DS1 will start to come almost immediately
      (since the query executed on DS1 is simple) but they will arrive at a
      relatively slow pace because they come from an external network.
   e. In these circumstances, increasing the value of the “Maximum size in
      memory of intermediate results” will result in a better execution
      time: the extra space will allow Denodo to retrieve results from DS1
      during the 30 seconds that DS2 is busy computing the query, even if
      they are still not needed, so they are ready when data from DS2
      finally arrives. In this case, if the “Maximum size in memory of
      intermediate results” parameter has a small value, then the available
      space would fill soon, and Denodo would spend most of the 30 first
      seconds just waiting for DS2, without doing actual work.
   

