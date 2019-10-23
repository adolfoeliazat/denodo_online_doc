======================
COMPACT_HADOOP_CACHE
======================

.. rubric:: Description

This procedure launches the task for cache compaction. This task can only be executed on databases that use a Hadoop data source for cache, such as Impala, Presto, Spark or Hive.

.. important:: This task is performed on the whole cache data source and is not restricted to the database used as input parameter. This means that if the data source is used for global cache or is shared with other databases for cache, all of them will be affected by the procedure.

During its execution it performs two operations:

- Removes all outdated rows from control tables.
- Deletes all storage tables that back expired temporary tables in the VDP Server.

Given the nature of these operations, the invoker must ensure that the VDP Server is not performing any other task involving the affected cache data source.

.. rubric:: Syntax

.. code-block:: bnf

   COMPACT_HADOOP_CACHE( 
       database_name : text
   )

-  ``database_name``: name of the database. This is the database whose cache data source will be compacted. Note that compaction affects the data source itself, so every database using this data source for cache will be affected.

This procedure does not return any value.

.. rubric:: Privileges Required

Only global administrators or local administrators of the input database can invoke this procedure.
