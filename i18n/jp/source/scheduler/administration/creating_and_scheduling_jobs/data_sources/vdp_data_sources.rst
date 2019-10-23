================
VDP Data Sources
================

VDP Data Sources can be used to access a Denodo Virtual DataPort server.
It is necessary to create this type of data source to create a VDP job.
The parameters to be specified are as follows:

-  **Connection URI**: Connection URI to the server.

-  **Username**: Username for connecting to the Virtual DataPort server.

-  **Password**: Password associated with the specified user.

-  **Use Kerberos (optional)**. If you check this option, the connection between the Scheduler
   server and VDP uses Kerberos authentication (with the provided login and password to authenticate
   to the Kerberos server). If the host where this Scheduler server runs does not belong to a Kerberos
   realm, follow the instructions from section :ref:`Database settings`.

-  **Query timeout** (optional). Maximum time (in milliseconds) that
   Scheduler will wait for the statement to be completed. If not indicated
   (or the value 0 is received), then it waits until execution is complete
   (by default 0).

-  **Chunk timeout** (optional). Maximum time (in milliseconds) that
   Scheduler will wait until it arrives a new chunk of results. Where this
   time is exceeded, Virtual DataPort returns the results extracted up to
   that moment. If not specified (or the value 0 is received), Virtual DataPort
   returns all the results together at the end of the statement run (by
   default 0).

-  **Chunk size** (optional). Number of results that make up a chunk of
   results. When Virtual DataPort obtains this number of results, it will return
   them to the Scheduler, even though the **Chunk Timeout** has not been
   reached (by default 100).


-  **Enable pool** (optional). It is possible to enable the use of a pool
   of connections against the Virtual DataPort server by checking this
   check box. In this case, the following parameters can be specified for
   the pool.
    
   -  **Initial pool size** (optional). Number of connections with which
      the pool is to be initialized. The specified number of connections
      are established and created in “idle” state, ready for use.
   -  **Maximum active** **connections in the pool** (optional). Maximum
      number of active connections that can be managed by the pool at the
      same time (zero means no limit).
   -  **Maximum idle** **connections in the pool** (optional). Maximum
      number of active connections that can remain idle in the (zero
      implies no limit).
   -  **Maximum time to wait for a connection from the pool**
      (optional). Maximum amount of time to wait for an idle object
      when the pool is exhausted (-1 means no limit).
   -  **Validation query** (optional). SQL query used by the pool to verify
      the status of the connections that are cached. The query should be
      simple, and the table in question should exist. For instance:
      ``select * from dual()``.
   -  **Test connections** (optional). If you check this option, the pool
      will try to validate each connection before being returned. Where the
      connection is not valid (restart of the management system, closed
      connection, etc.), it will be eliminated from the pool and a new one
      will be created.
