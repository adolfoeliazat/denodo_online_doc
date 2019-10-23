======================
Cache Maintenance Task
======================

The “Cache Maintenance Task” does the following:

-  Deletes the invalid or expired rows from the tables that hold the
   cached data of each view.
   
   Virtual DataPort marks a row as invalid when you execute a statement 
   that invalidates it. I.e. 
   ``ALTER TABLE CACHE INVALIDATE…`` or ``SELECT … CONTEXT('cache_invalidate'…)``

   A row expires when it reaches is time to live.

   When a row is marked as invalid or reaches its time to live is not 
   actually removed from the database. The row is in the cache, but when 
   the view is queried, the Execution Engine does not retrieve it from the 
   cache. However, is important to execute the cache maintenance task regularly because having too many invalid or expired rows may affect the performance of the queries that hit the cache.

-  Deletes the tables created as a result of the statement ``CREATE TEMPORARY TABLE`` and that expired. The default expiration
   time for temporary tables is 48 hours. The section :doc:`/vdp/vql/temporary_tables/temporary_tables` of the VQL Guide
   explains what temporary tables are.
   
-  Deletes the “query patterns” that have expired. The query patterns
   are stored in the table ``vdb_cache_querypattern`` of the cache
   database.


.. note:: In production environments, we strongly recommend setting the
   “Maintenance” option to “Off” to avoid executing the Cache Maintenance
   task periodically. Instead, use the Denodo Scheduler to program the
   execution of the ``CLEAN_CACHE_DATABASE`` procedure at a period where
   the Server and the cache’s database are not expected to be under heavy
   load.

By default, every time this task is executed, it deletes up to ten
million rows from each table of the cache database. Instead of deleting
them with a single ``DELETE`` statement, it executes several ``DELETE``
statements in a loop. For each cache table, it executes a ``DELETE``
statement that removes up to 100,000 rows. If there are more rows, it
does the same again and again for a maximum of a hundred times.

It does not invalidate all the rows with a single ``DELETE`` statement
to avoid that the transaction log of the database is filled up. This
could occur when the task has to delete a lot of rows from a table.

If you want this task to delete more rows of each table in each
execution, increase the number of times the ``DELETE`` statement is
executed per table. To do this, open the VQL Shell and execute:

.. code-block:: sql 

   SET 'com.denodo.vdb.util.tablemanagement.blockDelete.loopSize' = '1000';

This command will make the task to delete up to a hundred million rows
per table: 100,000 rows \* 1,000 iterations.

If you want to change the number of rows deleted in each iteration,
execute the following:

.. code-block:: sql 

   SET 'com.denodo.vdb.util.tablemanagement.blockDelete.blockSize' = '2000';

|

By default, this task is scheduled to run periodically (if the option
“Maintenance” is set to “On” in the “Cache” dialog of the “Server
configuration” or the cache configuration of the database).

In addition, this task also is executed when an administrator invokes
the procedure ``CLEAN_CACHE_DATABASE``. The section :ref:`CLEAN_CACHE_DATABASE` of the VQL Guide explains how to
invoke this procedure and what it returns.

 
|

When you disable the cache of a view (set the *Cache Mode* of the view to *Off*), the rows of the table in the cache database are marked as invalid. The next time the cache maintenance task runs, it will delete the invalid rows, but it will not remove the table. The table is only deleted when you remove the view.  


Maintaining the Underlying Database
===================================

In addition to running the "Cache Maintenance Task" from the Virtual DataPort server, you or the database administrator will have to do certain tasks on the cache database to avoid performance degradation. For example:

-  Prevent index fragmentation.
-  For databases that do not automatically reclaim space that is freed when deleting rows or tables, reclaim that space.
-  Make sure there is enough space available in the database to keep up with the current usage.
-  ...