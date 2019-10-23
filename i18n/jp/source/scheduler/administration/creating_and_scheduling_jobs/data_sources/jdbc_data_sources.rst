=================
JDBC Data Sources
=================

JDBC data sources can be used for the following objectives:

-  Creating a JDBC job (see section :ref:`Configuring new jobs`).
-  Use data from a relational database to obtain values for a variable
   in a parameterizable query of ITP, VDP, or JDBC jobs.
-  Create a relational database exporter (see section :ref:`Postprocessing
   Section (Exporters)`).

 

To create a JDBC data source you need to specify the following
parameters:

-  **Database adapter**. Name of the JDBC adapter to be used to access the
   relational database. In section :ref:`Plugins and JDBC Adapters`, the
   adapters distributed with Denodo Scheduler are discussed as well as how
   new ones can be added. When selecting an adapter, the connection URI
   fields, name of the driver class, and classpath are filled in
   automatically. In the case of a connection URI, a connection template
   for the database will appear which should be modified according to the
   remote server that you want to access.

-  **Connection URI**. Database access URI.

-  **Driver class name**. Name of the Java class of the JDBC adapter to be
   used.

-  **Classpath**. Path for the folder that contains the JAR files with the
   implementation classes needed by the JDBC adapter.

-  **Username** (optional). Username for accessing the external database.

-  **Password** (optional). User password for accessing the external
   database.



-  **Enable pool** (optional). It is possible to enable the use of a pool
   of connections against the database server by checking this check box. In
   this case, the following parameters can be specified for the pool.

   -  **Initial size of the pool** (optional). Number of connections with
      which the pool is to be initialized. The specified number of
      connections are established and created in an “idle” state, ready to
      be used.
   -  **Maximum active connections in the pool** (optional). Maximum number
      of active connections that can be managed by the pool at the same
      time (zero means no limit).
   -  **Maximum idle connections in the pool** (optional). Maximum number
      of active connections that can remain idle in the pool without the
      need for additional connections to be disengaged (zero implies no
      limit).
   -  **Maximum time to wait for a connection from the pool**
      (optional). Maximum amount of time to wait for an idle object
      when the pool is exhausted (-1 means no limit).
   -  **Validation query** (optional). SQL query used by the pool to verify
      the status of the connections that are cached. The query should be
      simple, and the table in question should exist.
   -  **Test connections** (optional). If you check this option, the pool
      will try to validate each connection before being returned. Where the
      connection is not valid (restart of the management system, closed
      connection, etc.), it will be eliminated from the pool and a new one
      will be created.
