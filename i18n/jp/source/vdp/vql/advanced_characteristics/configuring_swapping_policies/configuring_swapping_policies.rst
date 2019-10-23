=============================
Configuring Swapping Policies
=============================

Virtual DataPort provides two ways to avoid memory overflows when
dealing with huge datasets:

-  Stop retrieving data from a source when the size of these data that
   have not been processed, exceeds a certain limit.
-  Enable the swapping mechanism, which stores in secondary storage the
   intermediate results of the execution of a query, if their size
   exceeds a certain limit. It also swaps the intermediate results of
   the sorting operations (queries with an ``ORDER BY`` involved).

The section :doc:`/vdp/administration/server_administration_-_configuring_the_server/configuring_the_memory_usage_and_swapping_policy/configuring_the_memory_usage_and_swapping_policy`
of the
Administration Guide explain these methods in detail and how to change
the default values of the settings that control them.

You can also change these values in the following elements, by executing
certain VQL statements:

-  A specific database, with the statement ``ALTER DATABASE`` (see
   section :ref:`Modifying and Deleting Databases`)
-  A specific base view or derived view with the statements
   ``ALTER TABLE`` (see section :ref:`Query Capabilities: Search Methods and
   Wrappers`) and (``ALTER VIEW`` see section :ref:`Modifying a Derived
   View`) respectively.
-  A query, by adding the appropriate parameters to the ``CONTEXT``
   clause of the statement.

**Examples**

**Example 1**: Modifying the memory usage parameters of a database to
set the following parameters:

-  The “Maximum size in memory of each node” to 100
-  The “Maximum size of blocks stored in swap” to 150
-  The “Maximum size in memory of each intermediate result” to 50

.. code-block:: vql

   ALTER DATABASE testing  
     MEMORYCONFIG (
       SWAP ON (
         SWAPSIZE 100
         SWAPBLOCKSIZE 150
       )
       MAXRESULTSIZE 50
     )


**Example 2**: Disabling swapping in a view:

.. code-block:: vql

   ALTER VIEW V SWAP OFF;

**Example 3**: Enabling swapping in a view, setting the “Max size” to
100 Mb and the “Result maximum size” to 25 Mb:

.. code-block:: vql

   ALTER VIEW V SWAP ON SWAPSIZE 100 MAXRESULTSIZE 25;

**Example 4**: Running a query and disabling swapping. By adding the
attribute ``SWAP`` to ``CONTEXT`` clause of the query, the swapping will
be disabled, even if it is enabled in the view. That is because the
value of the parameters ``SWAP`` and ``SWAPSIZE`` of the ``CONTEXT``
clause override the settings of the view.

.. code-block:: vql

   SELECT … CONTEXT ('SWAP' = 'OFF')

**Example 5**: Running a query with swapping enabled and a swap “Max
size” of 100 Mb:

.. code-block:: vql

   SELECT … CONTEXT ('SWAP' = 'ON', 'SWAPSIZE' = '100' )

