===========================
Supported JDBC Data Sources
===========================

The following table lists the databases for which Virtual DataPort
provides a specific JDBC adapter.

When a database is not listed here, use the adapter "Generic" to connect to it.

.. table:: JDBC Drivers supported by Virtual DataPort
   :name: JDBC Drivers supported by Virtual DataPort

   +--------------+---------------+----------------------+----------------------+
   | Database     | Version       | Notes                | Driver included in   |
   |              |               |                      | Denodo               |
   +==============+===============+======================+======================+
   | Amazon       | N/A           |                      | Yes                  |
   | Redshift     |               |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Amazon       | 1.0           |                      | Yes                  |
   | Athena       |               |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Apache       | 10            |                      | Yes                  |
   | Derby        |               |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Apache       | 1.5           |                      | Yes                  |
   | Spark SQL    +---------------+                      |                      |
   |              | 1.6           |                      |                      |
   |              +---------------+                      |                      |
   |              | 2.x           |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Cassandra    | 3.x           | Using                | Yes                  |
   |              |               | DbSchema/DataStax    |                      |
   |              |               | JDBC Driver for      |                      |
   |              |               | Apache Cassandra     |                      |
   +--------------+---------------+----------------------+----------------------+
   | Elasticsearch| 6.4           |                      | Yes                  |
   |              +---------------+                      |                      |
   |              | 6.7           |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Greenplum    | 4.2           |                      | Yes                  |
   +--------------+---------------+----------------------+----------------------+
   | Hive         | 0.13.0        |                      | No (\*)              |
   |              | (Hive Server  |                      |                      |
   |              | 2)            |                      |                      |
   |              +---------------+                      |                      |
   |              | 1.1.0         |                      |                      |
   |              | (Hive Server  |                      |                      |
   |              | 2)            |                      |                      |
   |              +---------------+                      |                      |
   |              | 2.0.0         |                      |                      |
   |              | (Hive Server  |                      |                      |
   |              | 2)            |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Hive for     | 1.1.0         |                      | No                   |
   | Cloudera     |               |                      |                      |
   |              |               |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Hive for     | 1.2.1         |                      | No                   |
   | Hortonworks  |               |                      |                      |
   |              |               |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | IBM DB2      | 8.2           |                      | No                   |
   |              +---------------+                      |                      |
   |              | 9             |                      |                      |
   |              +---------------+                      |                      |
   |              | 9 for z/OS    |                      |                      |
   |              +---------------+                      |                      |
   |              | 10            |                      |                      |
   |              +---------------+                      |                      |
   |              | 10 for z/OS   |                      |                      |
   |              +---------------+                      |                      |
   |              | 11            |                      |                      |
   |              +---------------+                      |                      |
   |              | 11 for z/OS   |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Impala       | 2.3           |                      | No                   |
   +--------------+---------------+----------------------+----------------------+
   | Informix     | 7             |                      | No                   |
   |              +---------------+                      |                      |
   |              | 12            |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Microsoft    | 2000          | You can use either   | Both drivers are     |
   | SQL          +---------------+ the jTDS driver or   | included.            |
   | Server       | 2005          | the Microsoft        |                      |
   |              +---------------+ driver.              |                      |
   |              | 2008          |                      |                      |
   |              +---------------+                      |                      |
   |              | 2008R2        |                      |                      |
   |              +---------------+                      |                      |
   |              | 2012          |                      |                      |
   |              +---------------+                      |                      |
   |              | 2014          |                      |                      |
   |              +---------------+                      |                      |
   |              | 2016          |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | MySQL        | 4             |                      | No                   |
   |              +---------------+                      |                      |
   |              | 5             |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Netezza      | 4.6           |                      | No                   |
   |              +---------------+                      |                      |
   |              | 5.0           |                      |                      |
   |              +---------------+                      |                      |
   |              | 6.0           |                      |                      |
   |              +---------------+                      |                      |
   |              | 7.0           |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Oracle       | 8i            |                      | Yes                  |
   |              +---------------+                      |                      |
   |              | 9i            |                      |                      |
   |              +---------------+                      |                      |
   |              | 10g           |                      |                      |
   |              +---------------+                      |                      |
   |              | 11g           |                      |                      |
   |              +---------------+                      |                      |
   |              | 12c           |                      |                      |
   |              +---------------+                      |                      |
   |              | 12c In-Memory |                      |                      |
   |              +---------------+                      |                      |
   |              | E-Business    |                      |                      |
   |              | Suite 12      |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Oracle       | 11g           | The connection URI   | Yes                  |
   | TimesTen     |               | is different,        |                      |
   |              |               | depending on if      |                      |
   |              |               | there is a DSN in    |                      |
   |              |               | the same host as     |                      |
   |              |               | Virtual DataPort     |                      |
   |              |               | that points to       |                      |
   |              |               | Oracle TimesTen, or  |                      |
   |              |               | not.                 |                      |
   |              |               |                      |                      |
   |              |               | With DSN:            |                      |
   |              |               | ``jdbc:timesten:     |                      |
   |              |               | client:<client DSN>``|                      |
   |              |               |                      |                      |
   |              |               | Without DSN:         |                      |
   |              |               | ``jdbc:timesten:     |                      |
   |              |               | client:ttc_server=   |                      |
   |              |               | <host>;tcp_port=     |                      |
   |              |               | <port>;              |                      |
   |              |               | ttc_server_dsn=      |                      |
   |              |               | <server_dsn>``       |                      |
   +--------------+---------------+----------------------+----------------------+
   | PostgreSQL   | 8             |                      | Yes                  |
   |              +---------------+                      |                      |
   |              | 9             |                      |                      |
   |              +---------------+                      |                      |
   |              | 10            |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Presto       | N/A           |                      | Yes                  |
   +--------------+---------------+----------------------+----------------------+
   | SAP HANA     | 1.0           |                      | No                   |
   +--------------+---------------+----------------------+----------------------+
   | Snowflake    | N/A           |                      | Yes                  |
   +--------------+---------------+----------------------+----------------------+
   | Sybase ASE   | 12            | Either you can use   | Yes: Denodo provides |
   | / SAP ASE    +---------------+ the Sybase driver or | the jTDS driver.     |
   |              | 15            | the jTDS open source |                      |
   |              |               | driver.              |                      |
   +--------------+---------------+----------------------+----------------------+
   | Teradata     | 12            |                      | No                   |
   |              +---------------+                      |                      |
   |              | 13            |                      |                      |
   |              +---------------+                      |                      |
   |              | 14            |                      |                      |
   |              +---------------+                      |                      |
   |              | 15            |                      |                      |
   |              +---------------+                      |                      |
   |              | 16            |                      |                      |
   +--------------+---------------+----------------------+----------------------+
   | Vertica      | 7             |                      | No                   |
   |              +---------------+----------------------+----------------------+
   |              | 9             |                      | No                   |
   +--------------+---------------+----------------------+----------------------+

(\*): To connect to Hortonworks Hive, selects its specific adapter; do not select one of the generic Apache Hive adapters. Also, when obtaining the driver, obtain the specific driver for Hortonworks. The same applies to Cloudera Hive.

For legal reasons, the Denodo Platform does not include the JDBC driver of all the databases it supports. When that is the case, copy the file(s) of the driver to the folder :file:`<DENODO_HOME>/lib-external/jdbc-drivers/{database name - version}`, in the host where the Denodo server runs. For example, for Teradata 15, copy the driver to :file:`<DENODO_HOME>/lib-external/jdbc-drivers/{teradata-15}`. You do *not* need to restart after copying the driver there. Sometimes a JDBC driver is a set of jars and in that case, you need to copy all of them to this folder.
