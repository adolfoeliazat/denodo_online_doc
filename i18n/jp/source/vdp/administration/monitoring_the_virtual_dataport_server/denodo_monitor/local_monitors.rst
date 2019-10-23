==============
Local Monitors
==============

The Local monitors gather information about the host where the Denodo
Monitor is running. They do so by executing commands in the local
computer where the Denodo Monitor is running.

These monitors are useful if they are running in the same host as the
Denodo Platform servers you want to monitor.

There are two local monitors:

#. **Processes Monitor**. It logs information about **all** the running
   processes in the local host.
   The output is logged to ``denodo-monitor/logs/processes.log``.
   In Windows OS, it logs the output of the command
   ``tasklist.exe`` and in Unix/Linux, the output of ``ps``.
#. **Sockets Monitor**. It logs the information of the active
   connections of the local computer by executing the command
   ``netstat``.
   The output is logged to ``denodo-monitor/logs/sockets.log``.
