========================
Information on the Cache
========================

There are two types of MBeans that provide information about the status
of the cache engine of the Server:

-  ``DatabaseCache``: provides information about the settings of a
   database’s cache.
   See section :ref:`DatabaseCache MBean`.
-  ``ViewCache``: provides information about the settings of a view’s
   cache.
   See section :ref:`ViewCache MBean`.

DatabaseCache MBean
=================================================================================

The ``DatabaseCache`` MBeans provide information about the settings of a
database’s cache. There is an MBean of this type for each database. They
are located in ``com.denodo.vdb.management.mbeans`` > ``Cache`` >
*database name* and they have the following attributes:


-  ``ActiveRefreshCacheProcesses``: number of processes that are currently
   updating the cache of the views of the database.


-  ``CacheStatus``: status of the cache: ``ON`` or ``OFF``.


-  ``CustomDataSource``: ``true`` if this database is not using the cache
   settings established for the entire Server and is using a specific one.
   ``false`` otherwise.


-  ``DBURI``: URI of the database used as cache.


-  ``DatabaseName``: name of the database.


-  ``DriverClassName``: class of the JDBC driver used to connect to the
   database that stores the cached data.


-  ``LastCacheAccessStatus``: indicates whether the last access to the
   cache database was successful (``true``) or not (``false``).


-  ``MaxRefreshCacheProcesses`` (editable value): maximum number of
   concurrent processes that the Server may start to update the cache of
   the views of this database.


-  ``RefreshCacheProcess<i>`` (compound value): contains information about
   the process #i that was started to update the cache of a view. These
   MBeans are added as children of the ``DatabaseCache`` MBean. To see
   them, expand the ``DatabaseCache`` node in the MBeans browser. If there
   are no ``RefreshCacheProcess`` nodes, it means that no view has been
   refreshed yet.
   These MBeans have the following properties:


   -  ``CacheStatus``: mode of the cache that is being refreshed. Its value
      can be:

      -  ``OFF``
      -  ``PARTIAL``
      -  ``PARTIAL EXACT``
      -  ``PARTIAL PRELOAD``
      -  ``PARTIAL EXACT PRELOAD``
      -  ``FULL``.


   -  ``DatabaseName``: the name of the database.


   -  ``Exception``: if there was a problem while inserting data into the
      cache, this attribute contains the cause of the problem.


   -  ``Identifier``: unique identifier of the refresh cache process.


   -  ``NumConditions``: number of conditions of the executed query.


   -  ``NumOfInsertedRows``: number of rows that have been inserted in the
      cache so far.

      If the cache database is configured to use the “Bulk Data Load API”
      of the cache database, this is the number of rows that have been written
      into the data files that are then transferred to the database using its API.
      This is *not* the number of rows inserted on the database because the bulk data
      load APIs of the databases do not provide information about how many rows have
      been inserted, until all the data is stored.

   -  ``NumOfReceivedRows``: number of rows returned by the query and that
      have to be inserted in the cache.

      Once all the data has been inserted in the cache, this attribute and
      ``NumOfInsertedRows`` have the same value.


   -  ``ProjectedFields``: list of fields projected by the query.


   -  ``QueryPatternId``: ID of the query pattern.


   -  ``QueryPatternState``: state of the query pattern: ``PROCESSING``,
      ``VALID`` or ``INVALID``.


   -  ``SqlViewName``: name of the table in the cache data source where the
      cached data is stored.


   -  ``StartTime`` and ``EndTime``: instant at which the process that updates
      the cache started/finished.


   -  ``StartCachedResultMetadataStorageTime`` and
      ``EndCachedResultMetadataStorageTime``: instant at which the Server
      started/finished storing the metadata of the cached data.


   -  ``StartDataStorageTime`` and ``EndDataStorageTime``: instant at which
      the Server started/finished storing the data in the cache.


   -  ``StartQueryPatternStorageTime`` and ``EndQueryPatternStorageTime``:
      instant at which the Server started/finished storing the query pattern.


   -  ``TTLInCache``: time to live (in seconds) of the data stored by this
      “refresh cache process”. When this time is reached, the contents
      inserted by this process will be invalid.
      
      .. note:: This value is only relevant when the value of the attribute
         ``TTLStatusInCache`` is ``CUSTOM``.


   -  ``TTLStatusInCache``: status of the ``TTLInCache`` configuration. It has
      one of the following values:

      -  ``CUSTOM``: The view’s cache is using the TTL set by the attribute
         ``TTLInCache``.
      -  ``DEFAULT``: The view is using the TTL set by the cache settings of
         the database.
      -  ``NOEXPIRE``: The cached data never expires.


   -  ``VDPConditionList``: list of conditions in the query.


   -  ``ViewId``: internal name of the view.


   -  ``ViewName``: name of the view.



-  ``TotalRefreshCacheProcesses``: total number of refresh cache processes
   executed since the start of the Server.


-  ``UserName``: username used to connect to the database that stores the
   cached data.


-  ``UserPwd``: password used to connect to the database that stores the
   cached data.


This MBean has these operations:

-  ``getActiveRefreshCacheProcesses( <maxActiveCacheProcesses:integer> )``:
   operation that returns information about the processes that are
   currently inserting data in the cache of views of this database. The
   parameter limits the number of processes that we retrieve information
   of.



ViewCache MBean
=================================================================================

The ``ViewCache`` MBeans provide information about a view’s cache. There
is an MBean of this type for each view. They are located in
``com.denodo.vdb.management.mbeans`` > ``Cache`` > *database name* >
``ViewCache`` > *view name* and it has the following attributes:


-  ``BatchSizeInCache``: the Server inserts rows into the cache, in
   batches. This attribute controls the number of rows inserted, per batch.


-  ``CacheStatus``: status of the view’s cache. Its value can be:

   -  ``OFF``
   -  ``PARTIAL``
   -  ``PARTIAL EXACT``
   -  ``PARTIAL PRELOAD``
   -  ``PARTIAL EXACT PRELOAD``
   -  ``FULL``


-  ``DatabaseName``: name of the database that this view belongs to.


-  ``LastAccess``: last access to the view’s cache.


-  ``LastRefresh``: last time the content of this view’s cache was updated.


-  ``RefreshFailuresCount``: number of failures when writing new tuples in
   the view cache.


-  ``TTLInCache``: time to live (in seconds) of the data in the view’s
   cache. When this time is reached, the contents of the cache are
   considered invalid.
   
   .. note:: This value is only relevant when the value of the attribute
      ``TTLStatusInCache`` is ``CUSTOM``.


-  ``TTLStatusInCache``: status of the ``TTLInCache`` configuration. It has
   one of the following values:

   -  ``CUSTOM``: In this case, the view’s cache uses the TTL set by the
      attribute ``TTLInCache``.
   -  ``DEFAULT``: The view uses the TTL set by the cache settings of the
      database.
   -  ``NOEXPIRE``: The cached data never expires.


-  ``ViewName``: name of the view.




