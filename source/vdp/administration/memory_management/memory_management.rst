=================
Memory Management
=================

Virtual DataPort includes a series of memory settings that allow
customizing certain aspects of the Execution Engine. The following
sections explain how they work and includes some tips to use them
effectively.

Usually, you do not need to change the default memory settings, with the
exception of the “Limit Query Memory Usage” parameters (see section
:ref:`Limit the Maximum Amount of Memory of a Query`),

Memory usage considerations are only of interest for queries in which
the Virtual DataPort server processes a large number of rows and perform
several operations over them. In many cases this can be avoided by
maximizing push down of queries to the data sources, so rows are
filtered and/or aggregated at the data sources and Virtual DataPort only
has to combine a smaller number of rows. To maximize push-down, Denodo
applies many automatic optimizations that in some cases, involve
redesigning queries.


.. toctree::
   :maxdepth: 1
   
   streaming_vs_non-streaming_operators/streaming_vs_non-streaming_operators.rst
   swapping_parameters/swapping_parameters.rst
   limit_the_maximum_amount_of_memory_of_a_query/limit_the_maximum_amount_of_memory_of_a_query.rst
   edge_cases_in_streaming_operation/edge_cases_in_streaming_operation.rst
   cache_load_processes/cache_load_processes.rst