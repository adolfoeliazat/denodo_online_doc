=================================
General Information on the Server
=================================

The following MBeans provide general information about the Server:

-  ``VDBServerManagementInfo``: see section :ref:`VDBServerManagementInfo
   MBean`.
-  ``MemoryManagementInfo``: see section :ref:`MemoryManagementInfo MBean`.
-  ``LogManagementInfo``: see section :ref:`LogManagementInfo MBean`.


VDBServerManagementInfo MBean
=================================================================================

The ``VDBServerManagementInfo`` MBean provides general information about
the state of the Virtual DataPort server. It is located in
``com.denodo.vdb.management.mbeans``.


Attributes of the VDBServerManagementInfo MBean
-----------------------------------------------

The ``VDBServerManagementInfo`` MBean has these attributes:

-  ``ActiveConnections``: number of clients connected currently. This
   figure includes any type of clients: Administration Tools, JDBC
   clients, ODBC clients, published Web services, etc.
-  ``ActiveRequests``: number of queries that are currently running.
-  ``ActiveTransactions``: number of active transactions.
-  ``InstanceName``: if the attribute ``Master`` is ``false``, this
   attribute contains the name of the instance.
-  ``Master``: ``true`` if the Server is running standalone or is the
   Master instance of a group of instances of a Server.
   See more information about launching several instances of the same
   Server in the section :ref:`Configuring Several Instances of a Virtual
   DataPort Server`.
-  ``ServerName``: URI of the Virtual DataPort server. E.g.
   ``//localhost:9999/admin``
-  ``ServerUpdate``: name of the update installed on this server. For
   example:
   “7.0 20160905”.
-  ``SingleUserModeConnection``: id of the connection that has switched
   the Server to single user mode. This occurs if the client application
   issued the command ``ENTER SINGLE USER MODE`` but not
   ``EXIT SINGLE USER  MODE``; or if it is running a statement that
   temporarily switches the server to single user mode (e.g. create a
   user, create a database, etc.)
   
   The section :ref:`Automatic Single User Mode` lists what operations block the entire Server.
   
   The attribute is empty if the server is not in single user mode.
   
   
-  ``TotalConnectionFailures``: number of failed connections. For
   example, clients that tried to connect to the Server with invalid
   credentials.
-  ``TotalConnections``: number of connections established with the
   Server, since it was started.
-  ``TotalTransactions``: number of transactions started since the Server
   was started.
-  ``WaitingRequests``: number of requests that are waiting to be
   executed. These requests are waiting because the Server has surpassed
   the “Maximum concurrent requests” threshold.



Operations of the VDBServerManagementInfo MBean
-----------------------------------------------

The ``VDBServerManagementInfo`` MBean has these operations:


-  ``cancelRequest( <request id:integer> )``: cancels the request with the
   identifier “request id”.

   To obtain the id of a request you want to cancel, invoke the
   ``getActiveRequestList`` operation (see below), which will return
   information about the running requests, including their id.


-  ``cancelCacheProcess( <cache process id:long> )``: cancels the cache
   process with the identifier “cache process id”.

   To obtain the identifier of a cache process open to the
   ``DatabaseCache`` MBean of the database where the cache process is
   running and invoke the operation ``getActiveRefreshCacheProcesses``.
   This operation returns information about the cache processes running.
   Look for the “Identifier” attribute of the process you want to stop and
   use it as parameter of this operation. See more about the
   ``getActiveRefreshCacheProcesses`` operation in the section
   :ref:`DatabaseCache MBean`.

   When you invoke this operation, the cache process is cancelled, but the
   query continues until it finishes.


-  ``closeConnectionById( <connection id : long> )``: closes the connection
   with the identifier “connection id”.

   To obtain the identifier of a connection, invoke the operation
   ``getSessions(...)`` described below, which returns information about
   all the opened connections. In the result, look for the attribute
   “ConnectionId”.


-  ``closeConnectionsByIP( <IP address : String> )``: closes all the
   connections opened from an IP address. This operation returns these
   fields:

   -  ``ClosedConnectionIds``: identifiers of the connections successfully
      closed.
   -  ``FailedClosedConnectionIds``: identifiers of the connections
      unsuccessfully closed.


-  ``closeConnectionsByUser( <user name : String> )``: closes all the
   connections opened by a user. This operation returns these fields:

   -  ``ClosedConnectionIds``: identifiers of the connections successfully
      closed.
   -  ``FailedClosedConnectionIds``: identifiers of the connections
      unsuccessfully closed.


-  ``getActiveRequestList( <max requests:integer>, <database name:text)``:
   returns the list of the queries sent to the database
   ``<database name>``. This list contains:

   -  The requests that are currently being executed.
   -  The requests that are queued. A request is queued when the “Maximum
      concurrent requests” limit has been reached. See more about this
      limit in the section :ref:`Limiting the Number of Concurrent Requests`.
  
      When you cancel a request that has been queued, the request is not immediately
      canceled. Instead, it is canceled just when the Execution Engine is ready to
      execute this query and takes it out of the list of queued queries. Therefore,
      this operation returns all the requests that are queued, even if they already have been canceled. 

   If you want to cancel a query, execute this operation to get the value
   of the attribute ``Identifier`` of the query you want to cancel. Then
   invoke the operation ``cancelRequest(...)`` and pass this value to it.
   
   If the input parameter ``<database name>`` is empty, this operation
   returns all the requests sent to all the databases.
   
   For each query, the operation returns the attribute ``Queued``, among
   others. This attribute is ``true`` when the limit of maximum number of
   concurrent queries has been reached.
   
   See more about this limit and how to increase it in the section
   :ref:`Limiting the Number of Concurrent
   Requests`.


-  ``getRequestById ( <request id : number> )``: returns information about
   a specific request that is currently running or waiting to be executed.
   If the request has finished, you will get an error.


-  ``getSessions( <database name : text> )``: returns details about the
   sessions opened by clients to the Virtual DataPort server.

   If you do not provide a database name, it returns information about all
   the sessions. If you provide a name, it returns information about the
   sessions established with that database but not the others.
   
   This operation does not return information about connections opened by
   JMX clients.
   
   This procedure only returns information about connections opened by
   JMS clients during the time they are executing a query.
   
   This operation returns the following fields for each opened connection:

   -  ``AccessInterface``: type of client connected to the Server. :ref:`Possible
      values of the attribute "access interface"` lists the possible
      values of this attribute.
   -  ``AdminUser``: ``true`` if the user is an administrator user or it
      has the role ``serveradmin``. ``false`` otherwise.
   -  ``ClientIP``: IP address of the client. In case of Web services this
      is the IP address of the final client. I.e. the one that sends the
      HTTP request.
   -  ``ConnectionId``: unique identifier of the connection.
   -  ``ConnectionStartTime``: instant when the connection was opened.
   -  ``DatabaseName``: database that the client application is connected to.
   -  ``JMSQueueName`` (only for JMS connections): name of the JMS queue
      where the query is being sent from.
   -  ``IntermediateClientIP`` (only for SOAP, REST and the global RESTful
      Web service): IP address where the service is running.
      
      If the service has been deployed in the Web container embedded in Denodo, this will be the same IP of the Virtual DataPort server. Otherwise, it is the IP address of the JEE container where the service is deployed.
      
   -  ``LastExecutedQuery``: last query executed by the client.
   -  ``LastExecutedQueryEndTime``: instant when the last query finished.
   -  ``Login``: user name of the client.
   -  ``QueryRunning``: query that the client is executing right now and
      that has not finished yet. Empty, if there is no active query.
   -  ``QueryRunningId``: unique identifier of the query that the client is
      executing right now. Empty, if there is no active query.
   -  ``QueryRunningQueued``: ``true`` if the client has executed a query
      but the query is waiting because the Server has reached the “Max
      concurrent requests” limit.
      See more about this limit and how to increase it in the section
      :ref:`Limiting the Number of Concurrent
      Requests`.
   -  ``SessionId``: unique identifier of the session.
   -  ``SessionStartTime``: instant when the session was opened.
   -  ``SessionStatus``: ``true`` if the session is active. ``False``
      otherwise.
   -  ``SingleUserMode``: ``true`` if this session has switched one or more
      databases, or the entire Server to single user mode.
   -  ``SingleUserModeDatabases``: list of databases that have been
      switched to “single user mode”. If empty, it means that the entire
      Server was switched to single user mode.
   -  ``UserAgent``: name of the application that opens the connection.
      Setting the user agent in the application is useful to know which
      application opens each connection.
      The section :ref:`Setting the User Agent of an Application` explains how
      to set this.
   -  ``UserAuthenticationType``: type of authentication used by the
      client. The values can be “LOCAL”, “LDAP” or “KERBEROS”.
   -  ``WebServiceName`` (only for connections opened by SOAP and REST web services published by Denodo): name of the
      web service. For other types of clients, this is an empty string.


Notifications of the VDBServerManagementInfo MBean
--------------------------------------------------

The ``VDBServerManagementInfo`` MBean provides these notifications:

-  ``loginOk``: when a client logs in correctly into the Virtual
   DataPort server.
-  ``loginFailure``: when a client fails to connect. For example,
   because the password provided is not correct or the user does not
   have enough privileges to connect to the database.
-  ``logout``: when a client logs out.
-  ``openSession``: when a client opens a session.
-  ``closeSession``: when a client closes a session.

All of these notifications provide this information:

-  AccessInterface: the table :ref:`Possible values of the attribute "access
   interface"` lists the possible values of this attribute.
-  ClientIP
-  IntermediateClientIP
-  ConnectionId
-  ConnectionStartTime
-  ConnectionEndTime
-  SessionId
-  SessionStartTime
-  SessionEndTime
-  Login
-  DatabaseName
-  JMSQueueName
-  WebServiceName
-  UserAgent

The meaning of these fields is explained in the previous section.





MemoryManagementInfo MBean
=================================================================================

The ``MemoryManagementInfo`` MBean provides information about the memory
used by the Server. It is located in
``com.denodo.vdb.management.mbeans``.

Attributes of the MemoryManagementInfo MBean
--------------------------------------------

The ``MemoryManagementInfo`` MBean has these attributes:

-  ``TotalMemory``: total amount of memory used by the Server.
-  ``MaxMemory``: maximum amount of memory that the Server will attempt
   to use.



Operations of the MemoryManagementInfo MBean
--------------------------------------------

The ``MemoryManagementInfo`` MBean has this operation:

-  ``gc()``: this operation requests the Java Virtual Machine (JVM) that
   executes the Virtual DataPort server, to run the garbage collector of
   the JVM. This request may be ignored by the JVM.





LogManagementInfo MBean
=================================================================================

The ``LogManagementInfo`` MBean provides operations to obtain and change
the settings of the Log4J appenders
See more about the logging system in the section :ref:`Configuring the Logging Engine`. It is
located in ``com.denodo.mbeans``.

-  ``setLogLevel(<logger name>, <debug level>)``: sets the log level of
   a logger.
   ``<debug level>`` can be one of these: ``TRACE``, ``DEBUG``,
   ``INFO``, ``WARN``, ``ERROR`` or ``FATAL``.
   For example, ``setLogLevel(com.denodo.vdp.requests, INFO)`` enables
   the “requests log”, which logs all the requests processed by the
   Server in :file:`{<DENODO_HOME>}/logs/vdp/vdp-requests.log`.
   
   For example, ``setLogLevel(com.denodo.vdp.requests, INFO)`` enables the “requests log”, 
   which logs all the requests processed by the Server in 
   :file:`{<DENODO_HOME>}/logs/vdp/vdp-requests.log`.
   
-  ``getLogLevel(<logger name>)``: obtains the log level of a logger.
   If the logger has not been defined, it returns an error. Otherwise,
   it can return one of these: ``TRACE``, ``DEBUG``, ``INFO``, ``WARN``,
   ``ERROR`` or ``FATAL``.
