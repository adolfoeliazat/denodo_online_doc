==========================
Installation and Execution
==========================

To use the Data Catalog, you need to install the Virtual
DataPort server.

The Data Catalog is a web application that runs on the
web container included in the Denodo Platform.

Launching the Data Catalog
===========================================

There are two options to start and stop the Data Catalog:

1. Using the Denodo Platform Control Center, which allows to, among other things, start and stop
   all servers and tools of the Denodo Platform.

#. Using the scripts of the :file:`{<DENODO_HOME>}/bin` directory:

   -  To start the tool: ``datacatalog_startup``
   -  To stop the tool: ``datacatalog_shutdown``

   Each script has a version for Linux (``.sh``) and other one for Windows
   (``.bat``).

The default URL for accessing this tool from a local machine is
http://localhost:9090/denodo-data-catalog.

After this, you will see the log-in dialog.