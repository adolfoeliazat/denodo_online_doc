===============
Using the Cache
===============


.. toctree::
   :hidden:

   cache_invalidation.rst
   displaying_the_contents_of_the_cache.rst

You can enable the cache system (``CACHE`` option) for a view by using
the commands for modifying a base view (``ALTER TABLE`` - section :ref:`Query
Capabilities: Search Methods and Wrappers`) and modifying a view
(``ALTER VIEW`` - see section :ref:`Modifying a Derived View`). When it is
enabled, the tuples obtained as a result of executing queries on the
view will be stored in the cache.

Use the ``ALTER DATABASE`` command (see section :ref:`Modifying and Deleting
Databases`) to establish the default configuration of the cache for the
views that belong to a certain database.

Note that if the cache is activated in a view, you can run periodic
preloads of source data by executing a query that obtains the data that
you want to preload.

Virtual DataPort has the following cache modes:


-  ``PARTIAL``: when a query is executed against the view, the Server
   checks if the cache contains the data required to answer the query. If
   not, it queries the data source. This mode has additional clauses:

   -  ``EXACT``: with this type of cache, the cache stores the result of
      each query. Then, if the same query is executed and the entries of
      this query in cache have not expired (the ``TTL`` has not been
      reached), the data returned to the client is retrieved from the cache
      and not from the data source.
      

      .. important:: The ``PARTIAL`` cache without the ``EXACT`` clause may
         not be appropriate for certain types of *non-relational* sources.

   -  ``PRELOAD``: the cached data has to be reloaded “manually” by
      executing a special query instead of being automatically reloaded by
      the Server.
   
-  ``FULL``: when a view uses this cache mode, you have to *explicitly
   preload* the cache with a special query. Once it is preloaded, the data
   is always retrieved from the cache without checking if the query used to
   load the cache retrieved all the data from the source, or not. This
   means that the Server only retrieves data from the source while
   “preloading” data from the source.

   -  ``INCREMENTAL``: The incremental mode is a subtype of the full cache
      mode. With this mode enabled, the queries to this view are
      “incremental queries”. That is, the queries to this view merge the
      results obtained from the cache with the most recent data retrieved
      from the source. The main benefit of this mode is that the queries
      will always return fully up to date results without retrieving the
      full result set from the data source, just the rows that are
      added/changed since the last cache refresh.

The section :doc:`/vdp/administration/cache_module/cache_modes/cache_modes` of the Administration Guide explains in more
detail how these cache modes work.

To disable the cache of a view, use the parameter ``CACHE OFF`` of the
commands ``ALTER TABLE`` (for base views) or ``ALTER VIEW`` (for derived
views).

If the cache is enabled (``PARTIAL`` or ``FULL``), set the value of the
parameter ``TIMETOLIVEINCACHE``. Its values can be:

-  The number of seconds after the data will expire.
-  ``DEFAULT``. The time to live of the data is defined in the database
   that this view belongs to.
-  Or, ``NOEXPIRE``. The data in the cache will never expire.

The “Cache maintenance task” removes the expired or invalid rows from
the cache’s database.

The section :doc:`/vdp/administration/cache_module/cache_maintenance_task/cache_maintenance_task` of the administration guide
explains in more detail what this task does.

The behavior of this thread is controlled by the parameters
``MAINTENANCE`` and ``MAINTAINERPERIOD``:

-  If ``MAINTENANCE`` is ``OFF``, the Server never spawns the “Cache
   maintenance” thread. With this option, the expired rows of the cache
   are never actually deleted from the database. However, as they are
   marked as expired, they are never returned as results of the query.
-  If ``MAINTENANCE`` is ``ON``, the Server will spawn the “Cache
   maintenance” thread every ``MAINTAINERPERIOD`` seconds.

