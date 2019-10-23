============================
Launching the Denodo Monitor
============================

After changing the configuration file as explained in the previous
section, go to the ``<DENODO_MONITOR_HOME>/bin`` directory. The following sections explain how to use the 
scripts of this directory to monitor your Denodo servers.

Start Denodo Monitor
====================

To launch all the monitors you have configured, execute the script
``denodomonitor_startup`` with this syntax:

.. code-block:: bnf
   :caption: Syntax of the script denodomonitor_startup to start monitoring

   denodomonitor_startup 
       [ --directory <logs_path> ]
       [ --workspace <work_path> ]
       [ --singlepass ]

The supported options are:

* ``-d``, ``--directory <logs_path>``. You can configure the
  directory where the log files will be stored. When not specified, the logs
  are stored at ``<DENODO_MONITOR_HOME>/logs``.

* ``-w``, ``--workspace <work_path>``. You can launch the same
  instance of the Denodo Monitor with different configurations at the same time
  using a different workspace for each execution. When you specify a workspace,
  the Denodo Monitor will look for the configuration in the directory
  ``<work_path>/conf``. In addition, the logs will be stored in the directory 
  ``<work_path>/logs``, unless a specific logs directory was indicated with the
  option ``--directory <logs_path>`` or ``-d <logs_path>``. When not specified,
  the workspace by default is the ``<DENODO_MONITOR_HOME>`` directory.

* ``-s``, ``--singlepass``. If this parameter is present, the Denodo Monitor
  does a single execution of all the “monitors”, prints the results in the logs
  and exits. Otherwise, the Denodo Monitor runs indefinitely waiting for a
  shutdown request.

  .. note:: In the singlepass mode, the Virtual DataPort monitors are not
            launched.

Stop Denodo Monitor
===================

To stop the execution of the Denodo Monitor in a controlled way, execute the
script ``denodomonitor_shutdown`` with this syntax:

.. code-block:: bnf
   :caption: Syntax of the script denodomonitor_shutdown to stop monitoring

   denodomonitor_shutdown 
       [ --workspace <work_path> ]

.. note:: If you launched the Denodo Monitor in a specific workspace, you have
          to stop it using the same workspace.

Obtain a Memory Dump with the Denodo Monitor
============================================

To request a memory dump of one of the Denodo servers, execute the script
``denodomonitor_startup`` with the following syntax:

.. code-block:: bnf
   :caption: Syntax of the script denodomonitor_startup to request a memory dump

   denodomonitor_startup 
       [ --heapdump <Denodo server> ]

   <Denodo server> ::= 
         vdp
       | browserpool
       | arn
       | arnindex
       | verification
       | scheduler

The result is stored in the temporary directory of the host where the Denodo
server is running.