=================================================
Default Ports Used by the Denodo Platform Modules
=================================================

The table below lists the TCP/IP port numbers that each module of the
Denodo Platform listens for incoming connections.

These are the default port numbers. They can be changed during the
installation process or later, in the administration tool of each
module.

.. table:: Default TCPIP port numbers opened by the Denodo Platform modules
   :name: Default TCPIP port numbers opened by the Denodo Platform modules

   +--------------------------------------+--------------------------------------+
   | Server                               | Default Port                         |
   +======================================+======================================+
   | **Virtual DataPort and ITPilot Wrapper Server**                             |
   +--------------------------------------+--------------------------------------+
   | Server ports (for Virtual DataPort   | 9999 and 9997                        |
   | administration tools and JDBC        |                                      |
   | clients)                             |                                      |
   +--------------------------------------+--------------------------------------+
   | ODBC port                            | 9996                                 |
   +--------------------------------------+--------------------------------------+
   | Shutdown port                        | 9998                                 |
   +--------------------------------------+--------------------------------------+
   | **ITPilot Browser Pool**                                                    |
   +--------------------------------------+--------------------------------------+
   | Server ports                         | 6001 and 6003                        |
   +--------------------------------------+--------------------------------------+
   | Shutdown port                        | 6002                                 |
   +--------------------------------------+--------------------------------------+
   | Initial Microsoft Internet Explorer  | 6100                                 |
   | browser port                         |                                      |
   |                                      | The Browser Pool listens to this     |
   |                                      | port to communicate with the first   |
   |                                      | opened browser. The consecutive      |
   |                                      | ascending port numbers will be used  |
   |                                      | when additional browsers are         |
   |                                      | requested.                           |
   |                                      |                                      |
   |                                      | For example, if the size of the      |
   |                                      | Browser Pool is set to 30, the       |
   |                                      | Browser Pool will listen in the port |
   |                                      | range from 6001 to 6030.             |
   +--------------------------------------+--------------------------------------+
   | **ITPilot Verification Server**                                             |
   +--------------------------------------+--------------------------------------+
   | Server ports                         | 7001 and 7003                        |
   +--------------------------------------+--------------------------------------+
   | Shutdown port                        | 7002                                 |
   +--------------------------------------+--------------------------------------+
   | **ITPilot PDF Conversion Server**                                           |
   +--------------------------------------+--------------------------------------+
   | Server port                          | 8448                                 |
   +--------------------------------------+--------------------------------------+
   | **Scheduler Server**                                                        |
   +--------------------------------------+--------------------------------------+
   | Server ports                         | 8000 and 7998                        |
   +--------------------------------------+--------------------------------------+
   | Shutdown port                        | 7999                                 |
   +--------------------------------------+--------------------------------------+
   | **Scheduler Index Server**                                                  |
   +--------------------------------------+--------------------------------------+
   | Server ports                         | 9000 and 8998                        |
   +--------------------------------------+--------------------------------------+
   | Shutdown port                        | 8999                                 |
   +--------------------------------------+--------------------------------------+
   | **Denodo Platform Web container (Scheduler and ITPilot Administration       |
   | Tools, Virtual DataPort Web services, Data Catalog                          |
   | and Diagnostic & Monitoring Tool)**                                         |
   +--------------------------------------+--------------------------------------+
   | Web container port  (for http/https  |                                      |
   | requests)                            | 9090                                 |
   +--------------------------------------+--------------------------------------+
   | Shutdown port                        | 9099                                 |
   +--------------------------------------+--------------------------------------+
   | JMX port (for monitoring tools)      | 9098 and 9097                        |
   +--------------------------------------+--------------------------------------+

Almost all Denodo servers use two ports to listen to connections. Both are necessary and if the server is behind a firewall, both need to be opened in the firewall so the client applications can successfully connect to both.

If Denodo runs behind a firewall, open the following ports in the firewall:

-  9999 and 9997: to allow connections from JDBC clients and the administration tool of Virtual DataPort.
-  9996: to allow connections from ODBC clients of Virtual DataPort.
-  9090: to allow connections from client applications/browsers to the web applications and web services.

The other ports do not need to be opened on the firewall because they are accessed locally. For example, the Scheduler administration tool connects to the Scheduler server but because the administration tool and the server run on the same machine, the firewall does not intercept the network traffic between them.