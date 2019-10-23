====================
Cache Load Processes
====================

Loading the cache of a view can also result in significant memory
consumption. The reason is that, in order to maximize throughput, rows
are inserted in the cache database using a different thread than the one
that combines the data and returns it to the client. Since the read
process is usually faster than the load process, rows can accumulate and
memory usage can grow.

To avoid this problem, Denodo uses the “Maximum in size of memory of
each node” view parameter described in the section :ref:`Streaming Vs
Non-Streaming Operators`: if Denodo estimates the cache load process of
a view will use more memory than specified in the aforementioned
parameter, then it will swap part of the rows to disk to avoid
surpassing the limit.

There is an exception to this: when you add the parameter
``'cache_return_query_results' = 'false'`` to the ``CONTEXT`` clause of
the query the Server does not return the results to the client. In this
case, the data to be cached does not have to be swapped to disk.

