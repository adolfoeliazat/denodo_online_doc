================
ITP Data Sources
================

To be able to create an ITP job (see section :ref:`ITP Extraction Section`)
it is necessary to create an ITP data source in advance. To create an
ITP data source the following parameters need to be specified:

-  **Host**. Machine name in which the ITPilot server will run.
-  **Port**. Port number for the ITPilot server.
-  **Database name** (optional). Name of the database for executing the
   wrappers (by default itpilot).
-  **Username** (optional). Username with which the connection to the
   ITPilot server will be made (by default “admin”).
-  **Password** (optional). Password associated with the user specified
   (by default “admin”).
-  **Query timeout** (optional). Maximum time (in milliseconds) that
   Scheduler will wait for the wrapper to execute. If not indicated (or
   the value 0 is received), then it waits until execution is complete
   (by default 0).
-  **Chunk timeout** (optional). Maximum time (in milliseconds) that
   Scheduler will wait for a new chunk of results. Where this time is
   exceeded, ITPilot returns the results extracted up to that moment. If
   not specified (or the value 0 is received), ITPilot returns all the
   results together at the end of the statement run (by default 0).
-  **Chunk size** (optional). Number of results that make up a chunk of
   results. When ITPilot obtains this number of results, it will return
   them to the Scheduler, even though the **Chunk Timeout** has not been
   reached (by default 100).

 

