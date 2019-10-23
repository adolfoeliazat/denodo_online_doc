================================================
Components and Configuration of Virtual DataPort
================================================

You can install the following Virtual DataPort modules:

-  *Administration Tool*. The graphical administration tool of Virtual
   DataPort as well as the classes required to develop applications that
   query Virtual DataPort.
-  *Diagnostic and Monitoring Tool*. This web tool monitors the current state
   of one or more Virtual DataPort servers and analyzes its state in the past
   in order to identify the cause of a problem.
-  *Virtual DataPort Server*. The Server that stores the metadata of
   objects such as data sources, views, etc., and embeds a Web container
   and execute the queries.

|

When you select *Custom Installation* in the step 2 of the installation
wizard, you can configure the following settings of the Virtual DataPort
server:

-  **Server port number**: port that the Virtual DataPort server will
   listen for incoming connections.
-  **Shutdown port number**: port that the Virtual DataPort server will
   listen for shutdown requests.
-  **Auxiliary port number**: auxiliary port used by the Virtual
   DataPort server to communicate with its clients.
-  **ODBC port number**: port that the Virtual DataPort server will
   listen for incoming ODBC requests.
-  In Windows operating systems, select **Install as a Windows service**
   to install the Virtual DataPort server as a Windows service.

.. note:: If a firewall software is used to control the traffic between
   the clients and the server, it must be configured to allow communication
   using these ports.
