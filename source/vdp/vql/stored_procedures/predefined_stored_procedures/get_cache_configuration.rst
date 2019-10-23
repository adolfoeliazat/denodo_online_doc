=========================
GET_CACHE_CONFIGURATION
=========================

.. rubric:: Description

The stored procedure ``GET_CACHE_CONFIGURATION`` returns the cache configuration of the server and the cache configuration of each Virtual DataPort database.

.. rubric:: Syntax

.. code-block:: bnf

   GET_CACHE_CONFIGURATION (
       database_name : text
   )

-  ``database_name``: database of which you want to obtain its cache configuration. If ``null``, the procedure returns the cache configuration of all the databases and the global cache configuration.

The procedure returns one row containing the global cache configuration (this row has the field ``database_name`` set to ``null``) and one row per database. Each row has these fields, which are the same ones you can configure in the administration tool:

-  ``database_name``: name of the database. For the row that represents the global cache configuration, this field is ``null``.
-  ``database_datasource_name``: database that contains the cache data source. If the database does not have a specific cache configuration, this value is ``admin``. 
-  ``datasource_name``: name of the cache data source. If the database does not have a specific cache configuration, this value is ``vdpcachedatasource``. 
-  ``adapter_database_name``: adapter of the cache data source.
-  ``adapter_database_version``: version of the adapter of the cache data source.
-  ``adapter_driver_name``: driver class name of the adapter of the cache data source.
-  ``status``: ``ON`` if the cache is enabled on this database. ``OFF`` if it is disabled.
-  ``batch_size``: batch insert size (number of rows).
-  ``time_to_live``: *default time to live* of the database, in seconds. ``0`` if the database has its own cache configuration but the value of this field is *Default*, which means that it uses the time to live set in the global cache configuration.
-  ``maintenance``: ``true`` if the :ref:`cache maintenance task <Cache Maintenance Task>` is enabled. Otherwise, ``false``.
-  ``maintainer_period``: how often the server runs the cache maintenance task, in seconds.

.. rubric:: Privileges Required

This procedure does not require any privilege.

.. rubric:: Examples

.. rubric:: Example 1

To obtain the global cache configuration and the cache configuration of all the databases, execute this:

.. code-block:: vql

   SELECT database_name, status, batch_size, time_to_live, maintenance, maintainer_period
   FROM GET_CACHE_CONFIGURATION()

The result will be something like this:

.. csv-table:: 
   :header: "database_name", "status", "batch_size", "time_to_live", "maintenance", "maintainer_period"
   
   "null", "ON", "200", "120000", "true", "21600"
   "customer360", "ON", "200", "120000", "true", "21600"

The first row represents the global cache configuration because ``database_name`` is ``null``.

.. rubric:: Example 2
 
Execute this to obtain only the global cache configuration:
 
.. code-block:: vql
 
   SELECT *
   FROM GET_CACHE_CONFIGURATION()
   WHERE database_name is null;

.. rubric:: Example 3
 
Execute this to obtain the cache configuration of a specific database:
 
.. code-block:: vql
 
   SELECT *
   FROM GET_CACHE_CONFIGURATION()
   WHERE database_name = 'customer360';
