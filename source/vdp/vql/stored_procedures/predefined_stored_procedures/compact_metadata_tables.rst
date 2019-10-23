=========================
COMPACT_METADATA_TABLES
=========================

.. rubric:: Description

The stored procedure ``COMPACT_METADATA_TABLES`` reclaims unused space
from the database where the Virtual DataPort metadata is stored.

Virtual DataPort stores its metadata in a local database. *For each
virtual database* you create, it creates in this database a set of
tables that store the metadata: one table for the data sources, another
one for the views, another for the web services, etc. Over time, as the
users create and delete elements, the space used by the metadata
database grows because the database does not return unused space to the
operating system.

Run this procedure when the size of the directory
:file:`{<DENODO_HOME>}/metadata/db/dbmetadata` grows over a few gigabytes.

This procedure compacts the tables of the metadata database one after
the other; not all of them at once.

.. important:: Take into account the following:

   -  When compacting a table, it acquires an exclusive lock on that table.
      This lock prevents Virtual DataPort from reading or writing data from
      the table. This lock is released once the table is compacted.
   -  It is a memory-intensive process.
   -  Depending on the “mode” selected (see the parameter ``mode`` below),
      it creates temporary files. This is a problem if the disk is already
      full.

Therefore, try to run this procedure at a time the load of the server is
very low or there is no load. If the server belongs to a cluster of
Denodo servers, remove it from the cluster to avoid affecting external
applications that connect to Denodo.

.. important:: Before using this procedure for the first time, execute
   this VQL statement using an administrator account:

.. code-block:: vql

   CREATE PROCEDURE compact_metadata_tables
   CLASSNAME = 'com.denodo.vdb.contrib.storedprocedure.CompactMetatadataTablesProcedure';

.. rubric:: Syntax

.. code-block:: bnf

   COMPACT_METADATA_TABLES( 
       compaction_type : int 
   )

-  ``compaction_type``: mode used to compact the tables. This procedure compacts each
   table of the metadata database one after the other, not all of them at
   once.

   These are the possible values:
   
   -  ``0`` (no sequential): with this mode, for each table of the metadata
      database, the procedure compacts the table and *at the same time* all
      its indexes.
   -  ``1`` (sequential): with this mode, for each table of the metadata
      database, the procedure compacts the table and once it finishes
      compacting the table, it compacts its indexes one by one.
   -  ``2`` (in place): this mode does not use temporary tables and
      indexes. It just moves rows around the same space.

   The modes ``0`` (non-sequential) and ``1`` (sequential) are guaranteed
   to recover the maximum amount of free space that the mode ``2`` (in
   place), at the cost of temporarily creating new tables and indexes. This
   is because with the modes ``0`` and ``1``, the database copies active
   rows to newly allocated space and once it finishes copying these rows,
   it releases the space used by the old table. With mode ``2``, all the
   work is done in the same space.
   
   The mode ``0`` uses more memory and disk space than mode ``1``. To
   compact an index, a temporary file is created. With the mode ``0`` all
   the indexes of a table are compacted simultaneously so the temporary
   files for those indexes are created at the same time. Whereas with mode
   ``1``, the indexes of a table are compacted one by one and therefore, at
   all times there is only one temporary file for indexes.
   
   On the other hand, the mode ``0`` is much faster than mode ``1``.
   
   For the same reason, mode ``0`` is more CPU intensive than mode ``1``.
   
   The mode ``2`` cannot guarantee it will recover all available space, but
   if the hard drive is full, you make sure that the space temporary used
   by the database does not increase.

The output schema of this procedure has the following fields:

-  ``COMPACTION_TYPE_NAME``: name of the mode used to compact. That is,
   the name of the mode entered as a parameter:
-  ``METADATA_TABLES_COMPACTED``: number of tables of the metadata
   database that have been compacted.
-  ``ERRORS``: this database invokes the same procedure over each table
   of the metadata database. This field will hold the number of times
   the execution of this procedure failed.
