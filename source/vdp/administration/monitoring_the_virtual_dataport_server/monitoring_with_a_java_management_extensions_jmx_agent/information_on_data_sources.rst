===========================
Information on Data Sources
===========================

The following MBeans provide information about the data sources defined on the Server:

- ``GlobalDataSourceManagementInfo``: provides information about all the data
  sources of the Server. See section :ref:`GlobalDataSourceManagementInfo MBean`.

- ``LocalDataSourceManagementInfo``: provides information about all the data
  sources of a specific database. See section :ref:`LocalDataSourceManagementInfo MBean`.

- ``DataSourceManagementInfo``: provides information about a specific data
  source. See section :ref:`DataSourceManagementInfo MBean`.

GlobalDataSourceManagementInfo MBean
====================================

The ``GlobalDataSourceManagementInfo`` MBean aggregates information about the
number of data sources on the Virtual DataPort Server and how many active
requests they are processing. This MBean is located at
``com.denodo.vdb.management.mbeans`` > ``DataSourceManagementInfo``.

Attributes of the GlobalDataSourceManagementInfo MBean
------------------------------------------------------

The ``GlobalDataSourceManagementInfo`` MBean has these attributes:

-  ``TotalDataSourceCount``: number of data sources of any type on the Virtual
   DataPort Server.

-  ``TotalActiveRequestCount``: number of current requests to any data
   source on the Virtual DataPort Server.

-  For every type of data source there is an attribute with the number
   of data sources and the number of active requests to that type of
   source.

-  ``ITPMaintenanceDataSourceCount``: number of Web (WWW) sources that
   have the maintenance option enabled.

-  ``ITPMaintenanceActiveRequestsCount``: number of current requests to
   Web sources that have the maintenance option enabled.

LocalDataSourceManagementInfo MBean
===================================

Each Virtual DataPort database has a ``LocalDataSourceManagementInfo`` MBean,
which has the same attributes as ``GlobalDataSourceManagementInfo``, but only
considers the data sources that belong to that database. This MBean is located at
``com.denodo.vdb.management.mbeans`` > ``DataSourceManagementInfo`` > *database name*.

DataSourceManagementInfo MBean
==============================

Each data source has its own MBean in ``com.denodo.vdb.management.mbeans`` >
``DataSourceManagementInfo`` > *database name* > *data source type* > *data source name*.
It provides information about the number of requests to that specific data source, about
its pool of connections and about the last ping invocation.

Attributes of the DataSourceManagementInfo MBean
------------------------------------------------

Each ``DataSourceManagementInfo`` MBean has the following attributes:

-  ``DatabaseName``: name of the data source database.

-  ``DataSourceName``: name of the data source.

-  ``DataSourceType``: type of the data source, which can take one of the
   following values:

   + ``CUSTOM``. Note that internally, Excel files are actually ``CUSTOM`` data
     sources.

   + ``DF`` (delimited files)

   + ``ESSBASE`` (Oracle Essbase Multidimensional DB)

   + ``GS`` (Google Search)

   + ``ITP`` (Web - ITPilot)

   + ``JDBC``

   + ``JSON``

   + ``LDAP``

   + ``ODBC``

   + ``OLAP`` (OLAP multidimensional DB)

   + ``SALESFORCE``

   + ``SAPBWBAPI`` (SAP BI 7.x BAPI and SAP BW 3.x BAPI Multidimensional DB)

   + ``SAPERP`` (BAPI)

   + ``WS`` (SOAP Web services)
   
   + ``XML``

-  ``NumRequests``: Number of requests made on the data source since the
   start of the Server.

-  ``ActiveRequests``: Number of active requests to the data source.

-  ``MaxActive``, ``NumActive`` and ``NumIdle`` (only for JDBC and ODBC
   sources). Provide information about the pool of connections with the
   source:

   +  ``MaxActive``: maximum number of connections in the pool.

   +  ``NumActive``: number of connections established with the source that
      are being used to execute a query.

   +  ``NumIdle``: number of idle connections established with the source.

-  ``isMaintenanceEnabled`` (only for ITPilot - Web sources): ``true`` if
   the maintenance option is enabled.

-  ``PingStatus``, ``PingExecutionTime``, ``PingDuration`` and ``PingDownCause``
   (only for JDBC, ODBC, LDAP, OLAP, SAPBWBAPI, SAPERP and SALESFORCE sources).
   Provide information about the last ping invocation to the data source, both
   from the stored procedure :ref:`PING_DATA_SOURCE` or the ``ping()`` operation of
   this MBean explained below:

   + ``PingStatus``: result of the last ping invocation, which can take the value
     ``UP``, ``DOWN`` or ``TIMEOUT``.
   
   + ``PingExecutionTime``: instant when the last ping to the data source actually started.
   
   + ``PingDuration``: if ``PingStatus`` is ``UP`` or ``DOWN``, the time in
     milliseconds that the data source took to respond; ``NULL`` otherwise.
   
   + ``PingDownCause``: if ``PingStatus`` is ``DOWN``, a message explaining the
     cause of the problem; ``NULL`` otherwise.

Operations of the DataSourceManagementInfo MBean
------------------------------------------------

Those data sources that support the ping invocation (JDBC, ODBC, LDAP, OLAP,
SAPBWBAPI, SAPERP and SALESFORCE sources) also provide the following operation:

-  ``ping(<timeout : long>)``: Launches an asynchronous ping invocation to check
   whether this data source is accessible from Virtual DataPort or not.
   
   This operation does not return any value. Instead, the corresponding
   ``DataSourceManagementInfo`` MBean attributes are updated when the ping ends.
   
   The parameter ``timeout`` represents the time in milliseconds that the ping
   invocation will be waiting for an answer from the data source. This parameter
   is optional. If you provide ``NULL`` as a value, the ping will consider the
   default timeout, whose value is ``15000``.
