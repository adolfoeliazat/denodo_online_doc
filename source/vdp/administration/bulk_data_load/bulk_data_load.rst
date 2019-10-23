==============
Bulk Data Load
==============

There are two scenarios where Virtual DataPort stores data in a
database:

#. When caching data obtained from the source.
#. When performing a data movement. The section :ref:`Data Movement` explains
   what data movement is.

By default, to store data in a database, Virtual DataPort executes the
SQL statement ``INSERT`` to store each row. It uses techniques that
speed up the process of executing these statements (e.g. it uses
prepared statements and executes them in batch).

Most of the databases provide a proprietary interface to load big
amounts of data. These interfaces allow to insert data much faster than
by executing ``INSERT`` statements and Virtual DataPort is capable of
using it for the majority of databases.

When Virtual DataPort executes a query that involves using one of these
interfaces, it writes the data into a temporary text file and then, it
uses this interface to transfer this file to the database and deletes
the file.

Before enabling this feature on a data source or the cache engine, take
into account that these interfaces are worth using when the number of
rows to insert is tens of thousands or higher. With a lower number of
rows, there is no performance increase and sometimes, there even may be
a performance decrease.

JDBC data sources can be configured to use the bulk load APIs of the following databases:

-  Amazon Redshift
-  Hive 2.0.0 and higher
-  IBM DB2
-  Impala
-  Microsoft SQL Server
-  MySQL
-  Netezza
-  Oracle
-  PostgreSQL
-  Presto
-  Snowflake
-  Spark
-  Teradata
-  Vertica

Almost all of these databases have something in common: to use their
interfaces of bulk data load, the client application (in this case, the
Virtual DataPort server) has to write the data to an external file with
a specific format. After this, the data is transferred to the database.

By default, the Server creates these temporary files in the installation
of Denodo. If you want these files to be written in a different
directory, enter a path to that directory in the text field **Work
path** of the **Read & Write** tab of the data source. These temporary
files are deleted once the data is loaded on the database.

Some databases require to install an auxiliary application provided by
the database vendor. This application transfers the data file to the
database. For example, for Oracle, you need to install ``sqlldr``, for
SQL Server, ``bcp``, etc. With others is not necessary because this
functionality is included within their JDBC driver.

The following subsections list the databases for which you need to
install an external application or you have to change its default settings:

.. toctree::
   
   amazon_redshift.rst
   db2.rst
   hive.rst
   impala.rst
   mysql.rst
   oracle.rst
   postgresql.rst
   presto.rst
   snowflake.rst
   spark.rst
   sql_server.rst
   teradata.rst
   settings_of_the_generation_of_the_temporary_files.rst
