==========================================
Values of the Attribute “Access Interface”
==========================================

Virtual DataPort assigns the attribute “access interface” to the
incoming connections from clients.

The following table lists the possible values of this attribute.

 
.. table:: Possible values of the attribute "access interface"
   :name: Possible values of the attribute "access interface"
   
   +--------------------------------------+----------------------------------------+
   | Possible Values of the “access       | Meaning                                |
   | interface” Attribute                 |                                        |
   +======================================+========================================+
   | Data-Catalog                         | Data Catalog. Although it uses         |
   |                                      | the JDBC interface to execute          |
   |                                      | queries, it uses its own interface     |
   |                                      | for the authentication process.        |
   +--------------------------------------+----------------------------------------+
   | Diagnostic-Monitoring-Tool           | Denodo Diagnostic & Monitoring Tool    |
   +--------------------------------------+----------------------------------------+
   | ITP                                  | Client that uses the ITPilot API       |
   |                                      | described in the ITPilot Developer     |
   |                                      | Guide.                                 |
   +--------------------------------------+----------------------------------------+
   | JDBC                                 | JDBC client                            |
   |                                      |                                        |
   |                                      | In addition to third-party             |
   |                                      | applications, the following Denodo     |
   |                                      | components also use the JDBC           |
   |                                      | interface: Data                        |
   |                                      | Catalog,                               |
   |                                      | Diagnostic Monitoring Tool and         |
   |                                      | Solution Manager. These components     |
   |                                      | also have their own access interface.  |
   |                                      | They use their own                     |
   |                                      | interface to execute administrative    |
   |                                      | operations and the JDBC interface to   |
   |                                      | run queries.                           |
   +--------------------------------------+----------------------------------------+
   | JMS                                  | JMS client                             |
   +--------------------------------------+----------------------------------------+
   | JMX                                  | JMX client such as JMeter,             |
   |                                      | JVisualVM, etc.                        |
   +--------------------------------------+----------------------------------------+
   | ODATA                                | OData service of Denodo                |
   +--------------------------------------+----------------------------------------+
   | ODBC                                 | ODBC client                            |
   +--------------------------------------+----------------------------------------+
   | PORTLET                              | Portlet JSR-168 or JSR-286 published   |
   |                                      | by Denodo.                             |
   +--------------------------------------+----------------------------------------+
   | SCHED                                | Denodo Scheduler. Although it uses     |
   |                                      | the JDBC interface to execute          |
   |                                      | queries, it uses its own interface     |
   |                                      | for the authentication process.        |
   +--------------------------------------+----------------------------------------+
   | Solution-Manager                     | Solution Manager. Although it uses     |
   |                                      | the JDBC interface to execute          |
   |                                      | queries, it uses its own interface     |
   |                                      | for the authentication process.        |
   +--------------------------------------+----------------------------------------+
   | VDP-AdminTool                        | Virtual DataPort administration tool   |  
   +--------------------------------------+----------------------------------------+
   | Web-Panel                            | Web Panel                              |
   +--------------------------------------+----------------------------------------+
   | WS-REST                              | REST Web service published from        |
   |                                      | Denodo                                 |
   +--------------------------------------+----------------------------------------+
   | WS-REST-Generic                      | Denodo global RESTful Web service.     |
   |                                      | I.e. the one accessed from             |
   |                                      | http://localhost:9090/denodo-restfulws |
   +--------------------------------------+----------------------------------------+
   | WS-SOAP                              | SOAP Web service published from        |
   |                                      | Denodo                                 |
   +--------------------------------------+----------------------------------------+


 
