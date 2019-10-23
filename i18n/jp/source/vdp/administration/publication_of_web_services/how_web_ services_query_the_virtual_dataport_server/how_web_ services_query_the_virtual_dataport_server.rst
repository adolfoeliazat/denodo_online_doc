========================================================
How Web Services Query the Virtual DataPort Server
========================================================

When a Web service receives a request, it executes a SQL statement
(``SELECT``, ``INSERT``, ``UPDATE``, etc.). To do this, it behaves as
any other Virtual DataPort client: it connects to Virtual DataPort with
certain credentials and executes a query equivalent to the request
received. This query may fail if the user associated to these
credentials is not granted the appropriate privileges.

*SOAP Web services*: when you deploy a view as a SOAP Web service, by
default the service has four operations:

-  ``getXXX``: when invoked, the service executes a ``SELECT``
   statement.
-  ``insertXXX``: when invoked, the service executes an ``INSERT``
   statement.
-  ``updateXXX``: when invoked, the service executes an ``UPDATE``
   statement.
-  ``deleteXXX``: when invoked, the service executes a ``DELETE``
   statement.

You can delete any of these operations if you do not want any client to
perform them.

*REST Web services*: as opposed to SOAP Web services, REST Web
services publish views and not operations over view. Therefore, when you
publish a view, you are allowing external clients to perform the
following operations on them:

-  ``SELECT`` statements: when the service receives ``GET`` requests.
-  ``INSERT`` statements: when the service receives ``POST`` requests
-  ``UPDATE`` statements: when the service receives ``PUT`` requests.
-  ``DELETE`` statements: when the service receives ``DELETE``
   requests.

The credentials used by the Web service to connect to Virtual DataPort
depend on the authentication method selected. They fall into two
categories (see the last two columns of :ref:`Authentication methods support
by SOAP and REST Web services`):

#. “Uses the credentials of the Web service”: during the deployment of
   the Web service, the administrator selects a user, whose credentials
   will be used by the Web service to execute all the queries. In order
   for these queries to succeed, the selected user needs to have the
   right privileges to perform these queries.
   
   The section :ref:`Web Service Container Status Table` will explain how 
   the administrator selects a user for the service.
   
#. “Uses the Credentials of the Web Service’s Clients”: in order for
   these queries to succeed, the users that invoke the Web service need
   to have the right privileges to perform the queries that the service
   will execute on their behalf.
   
   For example, if the user ``bruce`` invokes an operation of the service, 
   the service connects to Virtual DataPort with the credentials provided by ``bruce`` in his request. 
   The query may or may not be executed depending on the privileges assigned to ``bruce``. If later, 
   the user ``scott`` invokes the same operation of the service, the service will 
   connect to Virtual DataPort with the credentials provided by ``scott``.

A Web service is a JEE web application, which contains a ``web.xml``
file. When you deploy a Web service or generate its WAR, and the service
uses one of the authentication methods of the column “Uses the
credentials of the Web service”, the user name and the encrypted
password of the user selected during the deployment of the Web service
are stored in the ``web.xml`` file. At runtime, the service uses these
credentials to connect to Virtual DataPort and execute queries.

For the services that use one of these types of authentication, if you
change the password of the user selected to deploy the Web service, you
have to redeploy the Web service. Otherwise, all the invocations to this
service will fail because the password send by the service will not be
correct.

Connection from the Web Services to the Server
==============================================

At runtime, the REST and SOAP Web services establish one or more
connections with the Virtual DataPort server to query views and send the
result to clients.

You can configure the parameters of these connections. The most
important one is the use of a connection pool. If enabled, the
connections opened by the Web service to Virtual DataPort are reused. In
production environments, the use of the connections pool is **strongly
recommended**.

The configuration parameters of the connection are:

-  **Chunk Size (rows)**, **Chunk timeout (milliseconds)** and **Query
   timeout (milliseconds)**. Their meaning is the same as in any other
   VDP client (see section :doc:`Access through JDBC <../../../developer/access_through_jdbc/access_through_jdbc>` of the Developer
   Guide).
-  **Enable Pool**. Select this check box to enable the connection pool.
   We recommend enabling this option.
-  **Initial number of connections**. Initial number of connections
   opened by the pool.
-  **Maximum number of active connections**. Maximum number of
   connections in the pool. A negative value means there is no limit.

If you expect any of your Web services to receive a high number of
concurrent requests, consider increasing the maximum number of threads
that Tomcat will create to attend requests. To do this, read the section :ref:`Increasing the Maximum Simultaneous Requests` of the Denodo Platform
Installation Guide.
