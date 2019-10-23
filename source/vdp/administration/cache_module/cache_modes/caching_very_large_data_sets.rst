============================
Caching Very Large Data Sets
============================

This section provides several recommendations for when you need to cache a large data set.

When caching the data of a view, the cache engine executes two UPDATE statements that may take a long time when you are caching a large data set (hundreds of millions of rows). These UPDATE statements are executed to:

1. Invalidate the existing data in the cache table.

#. Changing the status of the rows that have been inserted from "processing" to "valid".

Both UPDATEs are executed in the same transaction. The section :ref:`The Full Cache Mode at Runtime` explains this process in detail.

When working with large data sets, these UPDATE statements will have to update millions of rows. This may be a problem if the cache database limits the size of the transactions log or the time it takes the database to execute these statements is not acceptable. For these scenarios, the cache engine provides an option you can enable to speed up the process of loading the cache and to prevent the issues with the transaction log. If enabled, the process of loading the cache of a view changes in three ways:

1. The rows are inserted with state "valid", instead of inserting them with state "processing" and then executing an UPDATE statement to change their status to valid.
#. When ``'cache_invalidate' = 'all_rows'``, execute the statement ``TRUNCATE TRACE``, which will clear the cache table, instead of an UPDATE statement.
#. When ``'cache_invalidate' = 'matching_rows'``, execute the UPDATE statements outside a transaction.

To enable these options, add ``'cache_atomic_operation' = 'false'`` to CONTEXT clause of the query that loads the cache. For example:

.. code-block:: sql
   :emphasize-lines: 8

   SELECT *
   FROM employee
   CONTEXT (
     'cache_preload' = 'true'
   , 'cache_invalidate' = 'all_rows'
   , 'cache_wait_for_load' = 'true'
   , 'cache_return_query_results' = 'false'
   , 'cache_atomic_operation' = 'false');

We only recommend adding this parameter when is necessary for several reasons we will explain below, but first we will explain how this parameter changes the way the data is cached and invalidated:

-  As explained in the section :ref:`Full Cache Mode<Full Mode>`, the rows are inserted with the status "processing" and then, in a transaction, their status is set to "valid".

   With the parameter ``'cache_atomic_operation' = 'false'``, the rows are inserted with the status "valid". The benefit is that the cache engine does not have to execute an UPDATE to change the status of the new rows. This reduces the time of inserting new data into the cache table and avoids issues with the transaction log of the database.

-  When the query to load the cache has the parameter ``'cache_invalidate' = 'all_rows'``, after inserting all the rows, the cache engine executes an UPDATE to change the status of all the "old" valid rows to invalid.

   If the query has ``'cache_atomic_operation' = 'false'`` and ``'cache_invalidate' = 'all_rows'``, the cache engine executes ``TRUNCATE TABLE <cache table>`` before inserting cached data. This statement removes all rows from a table. In all databases, TRUNCATE is much faster than running an UPDATE to change the status of the rows from valid to invalid or execute a DELETE to delete all of them. The reason is that TRUNCATE bypasses the transaction log and UPDATE and DELETE do not.

   After the TRUNCATE TABLE, the cache engine begins inserting the cache data.

-  If the query has ``'cache_atomic_operation' = 'false'`` and ``'cache_invalidate' = 'matching_rows'``, the cache engine executes an UPDATE that changes the status of the valid rows that meet the WHERE condition of the query. Then, it begins inserting the new rows, with the status "valid".

   The UPDATE statement that switches the rows from valid to invalid is different than when you do not put ``'cache_atomic_operation' = 'false'`` in the query. With this parameter, the rows are invalidated in blocks. That is, instead of executing a single UPDATE statement to invalidate all the matching rows, the cache module executes several UPDATE statements to invalidate just a certain number of rows per UPDATE. Executing several UPDATE statements instead of one that modifies millions of rows will put less strain on the transaction log of the database, even if the number of rows to update does not change.

   By default, the number of rows invalidated with each UPDATE is 10,000 and the cache engine will keep running UPDATE statements until all the valid rows are set to invalid, or one hundred UPDATEs were executed. To change the number of rows to invalidate in each UPDATE, add the parameter ``'cache_invalidate_block_size'='<number of rows>'`` to the CONTEXT clause. This can be useful if the transaction log is small or the rows to invalidate very big. For example,

.. code-block:: sql
  :emphasize-lines: 8,9

  SELECT *
  FROM employee
  CONTEXT (
  'cache_preload' = 'true'
  , 'cache_invalidate' = 'all_rows'
  , 'cache_wait_for_load' = 'true'
  , 'cache_return_query_results' = 'false'
  , 'cache_atomic_operation' = 'false'
  , 'cache_invalidate_block_size' = '100000')

|

When you enabled the option *Use Bulk Data Load APIs* in the configuration of the cache (only available for some database adapters), the order in which these operations are executed is the same. The only difference is that instead of executing INSERT INTO statements, the cache engine uses the API of the database to load the data.

|

.. rubric:: Reasons to Add "'cache_atomic_operation' = 'false'" to the Query Only When Necessary

We only recommend adding the parameter ``'cache_atomic_operation' = 'false'`` when is necessary for several reasons:

1. If, while the query that loads the cache is running, another query hits this view, the
   second user will obtain incomplete results.

   To avoid this issue, you can set up a Scheduler job to run these queries during periods of downtime to avoid affecting clients.
#. If the cache preload query fails or is cancelled while inserting data
   in the cache database, the user has to invalidate manually the
   incomplete data.
#. With ``cache_atomic_operation' = 'false'`` and ``'cache_invalidate' = 'matching_rows'``, the data is invalidated in blocks. However, not all the databases support invalidating the
   data in blocks. E.g. Derby, Microsoft SQL Server 2000 (newer versions
   do support this option), Vertica and Netezza. If you try to do it,
   Virtual DataPort returns an error.

None of these things occur without this parameter.

Invalidating the Cache of a View in Blocks
==========================================

To invalidate the cached data of a view programmatically and not load new rows, use the statement

.. code-block:: sql

  ALTER TABLE <view name> CACHE INVALIDATE

With this command, the cache engine does the following:

1. Starts a transaction on the cache database.
2. Executes an UPDATE statement over the cache table to switch the status of the valid rows to invalid.
3. Commits the transaction.

As explained above, executing an UPDATE that modifies hundreds of millions of rows imposes a big load over the transaction log. To bypass the transaction log, add the parameter ``NOATOMIC``.

.. code-block:: sql

  ALTER TABLE <view name> CACHE INVALIDATE
  NOATOMIC [ INVALIDATEBLOCKSIZE <number of rows:integer> ]

With ``NOATOMIC``, the cache engine will not open a transaction and it will execute several UPDATE statements instead of just one. It will execute as many UPDATEs as necessary until all the valid rows have been switched to invalid, with a maximum of 100.

The parameter ``INVALIDATEBLOCKSIZE`` is optional. If present, the UPDATEs will invalidate the number of rows passed as parameter. If not, each UPDATE will invalidate 10,000 rows.

For example,

.. code-block:: sql

  ALTER VIEW employees
  CACHE INVALIDATE
  NOATOMIC INVALIDATEBLOCKSIZE 1000000;

This statement invalidates the cache of the view "employees" by executing
several ``UPDATE`` statements that change the state of the rows to
invalid. Each UPDATE modifies one million rows and the cache engine will execute.

|

To change the limit of maximum number of UPDATE statements to invalidate a view (by default, 100), execute

.. code-block:: vql

   SET 'com.denodo.vdb.util.tablemanagement.blockDelete.loopSize' = '<new value:integer>';
