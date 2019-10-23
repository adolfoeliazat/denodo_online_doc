.. _sm-installation:

=========================================
Launching the Solution Manager Components
=========================================

The following sections explain how to launch:

-  The :ref:`License Manager Server <Launching the License Manager Server>`.
-  The :ref:`Solution Manager Server <Launching the Solution Manager Server>`.
-  The :ref:`Solution Manager Administration Tool <Launching the Solution Manager Administration Tool>`.

Launching the License Manager Server
====================================

.. note:: You need to launch the License Manager Server before launching any of the servers of the Denodo Platform. That is because they will connect to the License Manager to obtain their license          

There are two options to start and stop the License Manager Server:

#. Use the :ref:`Denodo Platform Control Center <sm-control-center>` of your
   Solution Manager installation. The launcher of the License Manager Server is
   on the Solution Manager section.

#. With the scripts of the :file:`{<SOLUTION_MANAGER_HOME>}/bin/` directory:

   -  To start the License Manager Server: ``licensemanager_startup``.
   -  To stop the License Manager Server: ``licensemanager_shutdown``.

   
Launching the Solution Manager Server
=====================================

.. note:: Before you launch the Solution Manager Server, you need to have
          running the :ref:`License Manager Server <Launching the License Manager Server>`
          and the :ref:`Solution Manager Virtual DataPort Server <Launching the Virtual DataPort Server>`.

There are two options to start and stop the Solution Manager Server:

#. Use the Denodo Platform Control Center of your
   Solution Manager installation. The launcher of the Solution Manager Server is
   on the Solution Manager section.

#. With the scripts of the :file:`{<SOLUTION_MANAGER_HOME>}/bin/` directory:

   -  To start the Solution Manager Server: ``solutionmanager_startup``.
   -  To stop the Solution Manager Server: ``solutionmanager_shutdown``.

Launching the Solution Manager Administration Tool
==================================================

.. note:: Before you launch the Solution Manager Administration Tool, you need
          to have running the
          :ref:`Solution Manager Server <Launching the Solution Manager Server>`.

There are two options to start and stop the Solution Manager Administration
Tool:

#. Use the :ref:`Denodo Platform Control Center <sm-control-center>` of your
   Solution Manager installation. The launcher of the Solution Manager
   Administration Tool is on the Solution Manager section.

#. With the scripts of the :file:`{<SOLUTION_MANAGER_HOME>}/bin/` directory:

   -  To start the Solution Manager Administration Tool: ``solutionmanagerwebtool_startup``.
   -  To stop the Solution Manager Administration Tool: ``solutionmanagerwebtool_shutdown``.

The default URL for accessing the Solution Manager Administration Tool is http://localhost:19090/solution-manager-web-tool.