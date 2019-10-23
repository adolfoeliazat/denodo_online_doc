==========================
Installation and Execution
==========================

The Web Panel Administration Tool is a web application that runs on the
web container included in the Denodo Platform. The Web Panel can be installed
with the Solution Manager installer.

Launching the Web Panel Administration Tool
===========================================

There are two options to start and stop the Web Panel Administration Tool:

1. Using the Denodo Platform Control Center, which allows to start and stop
   all servers and tools of the Denodo Platform.

#. Using the scripts of the :file:`{<SOLUTION_MANAGER_HOME>}/bin` directory:

   -  To start the tool: ``webpanel_startup``
   -  To stop the tool: ``webpanel_shutdown``

   Each script has a version for Linux (``.sh``) and other one for Windows
   (``.bat``).

The default URL for accessing this tool from a local machine is
http://localhost:19090/webpanel.
