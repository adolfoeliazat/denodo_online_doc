================================================
Components and Configuration of Solution Manager
================================================

You must install the following Solution Manager modules:

-  *Solution Manager Server*. Main component of the Solution Manager.
-  *License Manager Server*. Administrates licenses within the Solution Manager.
-  *Solution Manager Administration Tool*. The graphical administration tool of
   the Solution Manager and License Manager servers.

|

When you select *Custom Installation* in the step 2 of the installation
wizard, you can configure the following settings:

For the Solution Manager server:

-  **Solution Manager port number**: port that the Solution Manager server will
   listen for incoming connections.
-  In Windows operating systems, select **Install as a Windows service**
   to install the Solution Manager server as a Windows service.

For the License Manager server:
   
-  **License Manager port number**: port that the License Manager server will
   listen for incoming connections.
-  **Metadata DB port number**: port used internally by the License Manager
   server.
-  In Windows operating systems, select **Install as a Windows service**
   to install the License Manager server as a Windows service.

.. note:: If a firewall software is used to control the traffic between
   the clients and the server, it must be configured to allow communication
   using these ports.
