=========================================
Memory Usage and Swapping Policy of Views
=========================================

Virtual DataPort provides two methods to avoid memory overflows when
dealing with huge datasets:

#. Stop retrieving data from a source when the size of these data that
   have not been processed, exceeds a certain limit (“Maximum size in
   memory of intermediate results (MB)”).
#. Enable the swapping mechanism, which stores in secondary storage the
   intermediate results of the execution of a query, if their size
   exceeds a certain limit. It also swaps the intermediate results of
   the sorting operations (queries with an ``ORDER BY`` involved).
   
   This is configured with “Swapping status” and “Maximum size in memory
   of each node (MB)”.

The section :ref:`Configuring the Memory Usage and Swapping Policy` explains
in more detail these methods and the parameters that control them:

-  Maximum size in memory of intermediate results
-  Swapping status
-  Maximum size in memory of each node

By default, the memory usage and swapping settings of a view are the
same as the database ones and the settings of the database may be the
ones of the Server. You can see the settings of the database in the
“Memory usage” wizard of the “Database management” (see section
:ref:`Configuring and Deleting Databases`).

To change them for a specific view, open the “Options” dialog of the
view and click the **Memory** tab.

Besides setting a different swapping configuration for a view, you can
change the swapping settings at runtime for a specific query, by adding
the parameters ``swap`` and/or ``swapsize`` to the ``CONTEXT`` VQL
clause.

The parameters added to the ``CONTEXT`` clause of the query override the
swapping configuration of the view. For example, it can be used to
change the value of the “Maximum size in memory of each node” or disable
swapping when querying a view that has swapping enabled. For example:

 
.. code-block:: sql

   SELECT * FROM INTERNET_INC CONTEXT ('swap' = 'OFF');
   SELECT * FROM INCIDENTS CONTEXT('swap' = 'ON', 'swapsize' = '500');

See more about the parameters ``swap`` and ``swapsize`` in the section
:ref:`Configuring Swapping Policies` of the VQL Guide.

