============
Cache Module
============

.. toctree::
   :hidden:

   generic_support_for_other_databases/generic_support_for_other_databases.rst
   cache_modes/cache_modes.rst
   cache_maintenance_task/cache_maintenance_task.rst


Virtual DataPort incorporates a Cache Engine that can store a local copy
of the data retrieved from the data sources, in a JDBC database. This
may reduce the impact of repeated queries hitting the data source and
speed up data retrieval, especially with certain type of sources.

This is the list of databases that you can use for this:

.. table:: List of databases supported by the Cache Engine
   :name: List of databases supported by the Cache Engine

   +--------------------------------------+--------------------------------------+
   | Database                             | Driver is included in the Denodo     |
   |                                      | Platform                             |
   +======================================+======================================+
   | Amazon Redshift                      | Yes                                  |
   +--------------------------------------+--------------------------------------+
   | Azure SQL Data Warehouse             | Yes (Microsoft SQL Server driver)    |
   +--------------------------------------+--------------------------------------+
   | Hive 2.0.0 (HiveServer2)             | No                                   |
   +--------------------------------------+--------------------------------------+
   | HP Vertica 7                         | No                                   |
   +--------------------------------------+--------------------------------------+
   | IBM DB2 8, 9, 9 for z/OS, 10,        | No                                   |
   | 10 for z/OS, 11 and 11 for z/OS      |                                      |
   +--------------------------------------+--------------------------------------+
   | Impala 2.3                           | No                                   |
   +--------------------------------------+--------------------------------------+
   | Microsoft SQL Server 2000, 2005,     | Two drivers are available and both   |
   | 2008, 2008 R2, 2012, 2014, 2016      | are included                         |
   |                                      |                                      |
   |                                      | `jTDS                                |
   |                                      | <http://jtds.sourceforge.net/>`_:    |
   |                                      | included.                            |
   |                                      |                                      |
   |                                      | Microsoft driver (MS driver)         |
   +--------------------------------------+--------------------------------------+
   | MySQL 4 and 5                        | No                                   |
   +--------------------------------------+--------------------------------------+
   | Netezza 6.0 and 7.0                  | No                                   |
   +--------------------------------------+--------------------------------------+
   | Oracle 8i, 9i, 10g, 11g, 12c and     | Yes                                  |
   | 12c In-Memory                        |                                      |
   +--------------------------------------+--------------------------------------+
   | Oracle TimesTen 11g                  | No                                   |
   +--------------------------------------+--------------------------------------+
   | PostgreSQL 9 and 10                  | Yes                                  |
   +--------------------------------------+--------------------------------------+
   | Presto 0.1x                          | Yes                                  |
   +--------------------------------------+--------------------------------------+
   | SAP HANA 1                           | No                                   |
   +--------------------------------------+--------------------------------------+
   | Snowflake                            | Yes                                  |
   +--------------------------------------+--------------------------------------+
   | Spark SQL 2.x                        | Yes                                  |
   +--------------------------------------+--------------------------------------+
   | Teradata 12, 13, 14, 15 and 16       | No                                   |
   +--------------------------------------+--------------------------------------+
   | Vertica 7 and 9                      | No                                   |
   +--------------------------------------+--------------------------------------+


The drivers that are not included in the Denodo Platform have to be downloaded from the vendor’s website.

In addition, you can configure the cache module to store the cache data in the
`Apache Derby <http://db.apache.org/derby/>`_
database embedded in Virtual DataPort. However, we strongly recommend using an
external Database Management System (DBMSs), especially in production
environments.

In addition to the adapters listed above, Virtual DataPort provides a generic adapter
that can be used with any other JDBC database to store cached data. The section
:ref:`Generic support for other databases` explains how to configure it. However,
we strongly recommend that you use one of the databases of the list above because
the performance when querying the cache database will be better. The reason is that
Virtual DataPort will push down more operations to the supported databases than the generic one.

|

To use the cache module you have to enable it in the Server (see section
:ref:`Configuring the Cache`). Alternatively, you can enable the cache
module only for some databases (see section :ref:`Configuring and Deleting
Databases`).

After enabling the cache module in the Server or in a database, enable
the cache in each view that requires it (see section
:ref:`Configuring the Cache of a View`).

When the Server initializes the cache, it creates a set of tables in the
database that will have information about the data stored in the cache.
For each query whose data is cached, the Server stores a **Query
Pattern** that later is used to know if a query can be “solved” with the
data stored in the cache. A Query Pattern is formed by:

-  An id that uniquely identifies every Query Pattern.
-  The name of the view that these data belongs to.
-  The conditions of the WHERE clause of the query used to retrieve the
   data from the data source.
-  The expiration time of the data set that the Query Pattern
   represents. Once this time is reached, the data set associated with
   this Query Pattern is marked as invalid and no longer used. At
   regular intervals (Maintenance Period), the Server deletes the data
   sets that are marked as invalid. Note that this applies to cache partial
   only. For cache full, the expiration date represents the time of the latest cache
   refresh.
-  Status of the Query Pattern. This can be valid, processing (while the
   data is being stored in the cache) or invalid (when the expiration
   time of the Query Pattern has been reached).

|

When enabling the cache on a view, the cache module creates a table in
the cache’s database that will store the cached data of this view. The
data types of the fields of this table are equivalent to the data types
of the fields of the view in Virtual DataPort.

When a user executes a query that involves a view with cache enabled,
the Server checks if it can retrieve the data from the cache. If not, it
retrieves the data from the data source, stores it in the cache and
creates a Query Pattern that represents these data.

The cache can also be by-passed for specific queries if you need to
access the actual data.

When querying a cache with required fields (e.g. a Web service base view
with mandatory input parameters) and the view is cached, you do not have
to provide a value for the input parameters. However, you have to do it
if the data has to be retrieved from the data source.

.. warning:: Users should be careful when enabling the cache for views
   that involve data sources with pass-through credentials enabled. The
   section :ref:`Considerations When Configuring Data Sources with Pass-Through
   Credentials` explains this in more detail.

.. warning:: The tables of the database that store cached data should be
   monitored for excessive fragmentation and compacted periodically if
   needed. In particular, with tables whose rows are invalidated very
   often.

You can see information about the Query Pattern of the database by
executing the Denodo stored procedure ``CACHE_CONTENT``. The section
:ref:`Displaying the Contents of the Cache` of the VQL Guide
explains how to invoke this procedure.

==========================================
Specific Information about Cache Databases
==========================================

This section contains information you have to take into account depending on the database you use to store cached data.

Azure SQL Data Warehouse
=============================

The Azure SQL Data Warehouse data source as cache have some limitations:

- Azure SQL Data Warehouse does not support unique indexes in tables.
- Data movements and creating materialized tables cannot be performed within a transaction.
- Binaries and complex type columns (arrays and registers) are limited to 8,000 bytes. If this size
  is exceeded during a cache load process, it will end with the error "String or binary data would be truncated".
- The only available isolation level is READ UNCOMMITTED.

Because of an issue in the Microsoft SQL Server Driver, you need a driver 
version 7.1.3 or newer to use Azure SQL Data Warehouse as cache. 
See `Bug <https://github.com/Microsoft/mssql-jdbc/issues/872/>`_ for more details.

Presto
=============================

When using Presto to store cached data, take the following into account:

-  You can only use it when the data is stored in the Hadoop Distributed File System (HDFS).
-  In the tab, **Read & Write**, select **Use bulk data load APIs** and fill the fields **HDFS URI** and **Hadoop executable location**. Otherwise, Virtual DataPort will not be able to cache data.
-  When using Presto to store cached data, you must not select the cache mode *Partial* on a view. The data will not be cached.

Spark
=============================

During the process of invalidating the cache, some queries may fail if they are 
executed when the Parquet file changes.

Databases with HDFS Storage
=============================

Databases that use HDFS storage (Hive, Impala, Presto and Spark) only support full cache mode. Any view with partial cache will be ignored and behave as if it has no cache.

The temporary files are generated with the Parquet file format, without compressing them. If you want these files to be generated compressed, execute this command from the VQL Shell:

.. code-block:: vql

   SET 'com.denodo.vdb.util.tablemanagement.sql.insertion.HdfsInsertWorker.parquet.compression'
       = '<compression mechanism>';

The value of "<compression mechanism>" can be ``off`` (no compression), ``snappy``, ``gzip`` or ``lzo``.

You do not need to restart after changing this property.

The database needs to support the selected compression algorithm.

Generating these files compressed may speed up the process loading the cache of a view or a data movement. However, it may not be the case if the database is in the same local area network as the Denodo server because the transfer speed is high. It is possible that there is not any reduction in time and an increase of CPU usage by the Denodo server because it has to generate the files compressed.

Besides, these databases do only support **Full Cache** invalidation.
