==============
Denodo Monitor
==============

.. toctree::
   :hidden:
   
   local_monitors.rst
   server_monitors.rst
   virtual_dataport_monitors.rst
   configuring_the_denodo_monitor.rst
   launching_the_denodo_monitor.rst   

The Denodo Monitor is a tool included in the Denodo Platform that logs
information about the Denodo servers:

-  Virtual DataPort
-  ITPilot Browser Pool and ITPilot Verification server
-  Scheduler and Scheduler Index

The Denodo Monitor runs indefinitely and it gathers information about
these Servers periodically and logs it into tab-separated values files.

The Denodo Monitor launches three types of “monitors” that record the
activity of one or more Denodo servers and the host where they are
running.

-  **Local monitors**. They gather information about the host where the
   Denodo Monitor is running. See section :ref:`Local Monitors`.
-  **Server monitors**. They log data about the running threads and
   memory usage of a Denodo server. See section :ref:`Server Monitors`.
-  **Virtual DataPort monitors**. They log information specific to a
   Virtual DataPort server and that the other Denodo servers do not
   provide. See section :ref:`Virtual DataPort Monitors`.

Except the Local monitors, all the monitors connect to the JMX interface (`Java Management eXtensions <https://www.oracle.com/technetwork/java/javase/tech/javamanagement-140525.html>`_)
of the Denodo servers so they can log information about Servers running in remote hosts.

The section :ref:`Configuring the Denodo Monitor` explains how to configure
the Denodo Monitor and the section :ref:`Launching the Denodo Monitor`
explains how to launch it.
