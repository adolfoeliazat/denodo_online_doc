==================================================
Default Ports Used by the Solution Manager Modules
==================================================

The table below lists the TCP/IP port numbers that each module of the
Solution Manager listens for incoming connections.

If these modules are behind a firewall, you must open the appropriate
ports in the firewall. If the Solution Manager runs on Windows, remember
to open the ports in Windows Firewall as well.

These are the default port numbers. They can be changed during the
installation process or later, in the administration tool of each
module.

.. table:: Default TCP/IP port numbers opened by the Solution Manager modules
   :name: Default TCPIP port numbers opened by the Solution Manager modules

   +--------------------------------------+--------------------------------------+
   | Server                               | Default Port                         |
   +======================================+======================================+
   | **Solution Manager Server**                                                 |
   +--------------------------------------+--------------------------------------+
   | Port                                 | 10090                                |
   +--------------------------------------+--------------------------------------+
   | **License Manager Server**                                                  |
   +--------------------------------------+--------------------------------------+
   | Port                                 | 10091                                |
   +--------------------------------------+--------------------------------------+
   | **Virtual DataPort Server**                                                 |
   +--------------------------------------+--------------------------------------+
   | Server port (Virtual DataPort        | 19999                                |
   | administration tool and JDBC port)   |                                      |
   +--------------------------------------+--------------------------------------+
   | ODBC port                            | 19996                                |
   +--------------------------------------+--------------------------------------+
   | Auxiliary port                       | 19997                                |
   +--------------------------------------+--------------------------------------+
   | Shutdown port                        | 19998                                |
   +--------------------------------------+--------------------------------------+
   | **Denodo Platform Web container (Solution Manager Administration Tool)**    |
   +--------------------------------------+--------------------------------------+
   | Web container port                   | 19090                                |
   +--------------------------------------+--------------------------------------+
   | Shutdown port                        | 19099                                |
   +--------------------------------------+--------------------------------------+
   | JMX port                             | 19098                                |
   +--------------------------------------+--------------------------------------+
   | Auxiliary JMX port                   | 19097                                |
   +--------------------------------------+--------------------------------------+
