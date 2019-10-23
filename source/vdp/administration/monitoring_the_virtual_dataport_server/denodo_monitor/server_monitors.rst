===============
Server Monitors
===============

The Server monitors log data about the running threads and memory usage
of Denodo servers. They connect to the Denodo server via JMX so, unlike
local monitors, they can obtain information from servers running in a
different host than the Denodo Monitor.

There are two server monitors:

.. contents:: 
   :depth: 1
   :local:
   :backlinks: none
   
Resources Monitors
=================================================================================

The Resources monitor logs several parameters of the memory footprint of
a Denodo server.

The output is logged to
``denodo-monitor/logs/<Denodo server name>-resources.log``

The information collected is the following:


-  Memory information: memory usage of the JVM that executes the monitored
   Server: Perm Gen, Survivor Space, Tenured Gen, Eden Space, Code Cache,
   Heap Memory Usage and Non-Heap Memory Usage.


-  Classes information: number of currently loaded classes (Loaded Class
   Count), total number of loaded classes (Total Loaded Class Count),
   number of threads (Thread Count) and maximum number of threads (Peak
   Thread Count).


-  Virtual DataPort Information (only when the monitor is connected to a
   Virtual DataPort server)

   -  VDP Total Conn. Number of connections opened during the interval.
   -  VDP Active Conn. Number of currently opened connections.
   -  VDP Active Requests. Number of requests that are currently being
      executed.
   -  VDP Waiting Requests. Number of queued requests.
   -  VDP Total Mem. Estimate of the total memory occupied by the server.
   -  VDP Max Mem. Estimate of the memory peak reached by the server since
      its start.




Threads Monitors
=================================================================================

The Threads monitor logs the threads launched by a Denodo server and how
much CPU time they consume.

Its log file is stored in
``denodo-monitor/conf/<Denodo server name>-threads.log``.



