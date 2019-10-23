===================================================
Information and Events on the Running of Statements
===================================================

The ``RequestsManagementInfo`` MBeans provide information about the
“read only” and Data Manipulation Language (DML) queries processed by a
database of the Server. These MBeans are located in
``com.denodo.vdb.management.mbeans`` > ``RequestsManagementInfo`` >
*database name*.

Attributes of the RequestsManagementInfo MBeans
-----------------------------------------------

There is a ``RequestsManagementInfo`` MBean for each database and they
have the following attributes:


-  ``DatabaseName``. Name of the database.


-  ``MaxRequests``. Maximum number of requests exported at the same time in
   the MBean. Each request appears as an attribute of the form
   ``Request<i>``.


-  ``TotalRequests``. Total number of requests processed since the launch
   of the Server.


-  ``ActiveRequests``. Number of active requests.


Notifications of the RequestsManagementInfo MBeans
--------------------------------------------------

The MBean RequestsManagementInfo of a database generates two types of notifications:

1. Notification that indicate the beginning of a request.
#. Notification that indicate the end of a request.

Client applications can subscribe to these notifications. To subscribe to these in Java Visual VM, open the MBean RequestsManagementInfo of the database, click the tab **Notifications** and click **Subscribe**.

These notifications have the following attributes:

-  ``Timestamp``. Instant at which the notification is generated.
-  ``Type``. Type of notification. Its value can be ``startRequest`` or ``endRequest``.
-  ``UserData``. Compound element. Its properties contain information about the request
   sent to this database. To see them, expand
   the node of this MBean in the MBeans browser. If there are no
   ``Request`` nodes, it means that this database has not received any
   request yet.
   These MBeans have the following properties:

   -  ``AccessInterface``. Type of client that performed the request.
      :ref:`Possible values of the attribute "access interface"` lists the
      possible values of this attribute.
   -  ``Cache``. ``true`` if the query has accessed the cache during its
      execution. ``false`` otherwise.
   -  ``ClientIP``: IP address of the client. In case of Web services this
      is the IP address of the final client. I.e. the one that sends the
      HTTP request.
   -  ``Completed``. ``true`` if the query finished correctly. ``false``
      otherwise.
   -  ``DatabaseName``. Name of the database on which the statement is
      executed.
   -  ``Elements``. Name and database of the views/stored procedures referenced
      in the query.
      This property has the format "<database of the view/stored procedure>"."<name of the view/stored procedure>".
   -  ``EndTime``. Moment at which the statement finished its execution.
   -  ``Identifier``. Identifier of the request. This number is unique but it may not be consecutive (i.e. a request may have identifier "123" and the next one, "125"). When the Server is restarted, this value is restarted to 1.
   -  ``NumRows``. Number of rows returned by the query.
   -  ``Queued``. ``true`` if this query has been queued because the limit of
      maximum number of concurrent queries has been reached. ``false``
      otherwise.
   -  ``RequestType``. This indicates the type of statement. It can take
      the following values: ``SELECT BASE VIEW``, ``SELECT VIEW``,
      ``QUERY WRAPPER``, ``CALL STOREDPROCEDURE``, ``INSERT``, ``UPDATE``
      or ``DELETE``.
   -  ``SessionId``. Id of the session that this request belongs to.
   -  ``StartTime``. Instant at which the Server received the query.
   -  ``State``. This attribute stores the state of the top node of the
      execution plan and its value can be ``OK``, ``ERROR`` or
      ``PROCESSING``. Note that the state of the top node can be ``OK`` and
      the state of other nodes be ``ERROR``. For example, if one of the
      branches of a JOIN query fails and does not return results, its state
      is ``ERROR``. However, as the JOIN operation finishes correctly, its
      state is ``OK``. In this scenario, the attribute ``State`` is ``OK``
      and ``Completed`` is ``false``.
   -  ``Swap``. ``True`` if the server swapped the intermediate results to
      disk during the execution of the query. ``False`` otherwise.
   -  ``TransactionId``. Identifier of the transaction. Empty string if this request was not part of a transaction.
   -  ``UserAgent``. Name of the application that performed the request.
      Setting the user agent in the application is useful to know which
      application opens each connection.
      The section :ref:`Setting the User Agent of an Application` explains how
      to set this.
   -  ``UserName``. ID of the user running the statement.
   -  ``VQLQuery``. VQL code for the statement.
   -  ``WaitingTime``. Number of milliseconds the query was waiting in the
      queue of queries before the Server began executing it.
      A query is hold in the queue of queries when the limit of concurrent
      requests is reached.
      See more about this limit and how to increase it in the section
      :ref:`Limiting the Number of Concurrent Requests`.
   -  ``WebServiceName``. If this request was sent by a SOAP or REST web service published by this Denodo server, this is the name of the web service.
      If the request comes from a different type of client, this is an empty string.


-  ``SeqNum``. Identifier of the notification.
-  ``Message``. In this type of notification, it takes the value
   ``Started the request`` or ``Finished the request``.
-  ``Event``. This will be

   .. code-block:: none
   
      javax.management.Notification[source=com.denodo.vdb.management.mbeans:type=RequestsManagementInfo,databaseName=<dbname>][type=endRequest][message=Finished the request '<i>']
      
   , where ``<dbName>`` is the name of the Virtual DataPort database and ``<i>``
   is the statement identifier.
-  ``Source``. MBean name.
