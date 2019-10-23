==========================
Installation and Execution
==========================

The :doc:`Denodo Platform Installation Guide <../../../platform/installation/index>`
provides all the
information required to install Denodo Scheduler, including the minimum
requirements for hardware and software, and instructions for the use of
the installation tool and for the initial system configuration.

 

Denodo Scheduler includes a server for scheduling jobs and a Web server
that supports the administration tool.

 

The servers can be started and stopped using the Denodo Platform Control
Center. To
connect to the administration tool you need to use the user “admin” with
the initial password “admin”. The default URL for accessing the Web
administration tool from a local machine is
http://localhost:9090/webadmin/denodo-scheduler-admin.

 

Alternatively, scripts are provided in the path ``DENODO_HOME/bin``.
There is a script for the scheduling server ``scheduler_startup.sh``
(``scheduler_startup.bat`` in Windows) to
start it and a script ``scheduler_shutdown.sh``
(``scheduler_shutdown.bat`` in Windows)
to stop it. To start and stop the Web administration tool the scripts
``scheduler_webadmin_startup.sh`` and ``scheduler_webadmin_shutdown.sh``
are available (``scheduler_webadmin_startup.bat`` and
``scheduler_webadmin_shutdown.bat`` in Windows).

 

In the case of Windows machines, a script is included to install the
scheduling server as a service. The script receives the name
``schedulerservice.bat``. If the service is configured to be started
with a specific user account, this one needs to have full or
administration privileges on over the Denodo Platform installation
folder and all of its subfolders.
