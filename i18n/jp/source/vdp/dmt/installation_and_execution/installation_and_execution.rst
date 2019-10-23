.. _dmt-installation:

==========================
Installation and Execution
==========================

The Diagnostic & Monitoring Tool is a web application that runs in the web
container included in the Denodo Platform. It is available to install in the
Denodo Platform installer, as a component of the Virtual DataPort module.

Take into account that the Diagnostic & Monitoring Tool depends on the Denodo
Virtual DataPort server to work; hence, you need to install this product too.

The :ref:`Denodo Platform Installation Guide` provides all the
required information to install Denodo Virtual DataPort and the Diagnostic &
Monitoring Tool.

.. note::
   We do not recommend installing the Diagnostic & Monitoring Tool in the production
   environment, since it may affect the performance of those machines. Using a 
   dedicated machine or the development or the staging environments is a better solution.

   Besides, when monitoring, remember to use a different machine from the monitored server
   to not affect the results.

Launching the Monitoring and Diagnostic Tool
============================================

There are two options to start and stop the Diagnostic & Monitoring
Tool:

#. Using the :doc:`Denodo Platform Control Center </platform/installation/denodo_platform_control_center/denodo_platform_control_center>`.

#. Using the scripts of the :file:`{<DENODO_HOME>}/bin` directory:

   -  To start the tool: ``diagnosticmonitoringtool_startup``
   -  To stop the tool: ``diagnosticmonitoringtool_shutdown``

   Each script has a version for Linux (``.sh``) and one for Windows
   (``.bat``).

The default URL for accessing this tool from a local machine is
``http://localhost:9090/diagnostic-monitoring-tool``.
