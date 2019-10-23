=============
GET_SESSIONS
=============

.. rubric:: Description

The stored procedure ``GET_SESSIONS`` returns information about all the
opened sessions.

This procedure does not return information about connections opened by
JMX clients.

This procedure only returns information about connections opened by a
JMS client during the time this client is executing a query.

.. rubric:: Syntax

.. code-block:: bnf

   GET_SESSIONS ( 
       database_name : text
   )

-  ``database_name``: name of the database. If ``null``, it returns information about all the sessions.
   Otherwise, it only returns information about the sessions of this database. 

The procedure returns a row for each field of each connection.

The procedure returns this fields:

-  ``database_name``: name of the database that you are connected to.
-  ``connection_id``: unique identifier of the connection.
-  ``connection_start_time``: instant when the connection was opened.
-  ``client_ip``: IP address of the client. In case of Web services this
   is the IP address of the final client. I.e. the one that sends the
   HTTP request.
-  ``user_agent``: name of the application that performed the request.
   
   Setting the user agent in the application is useful to know which
   application opens each connection.
   
   The section :ref:`Setting the User Agent of an Application` of the Administration Guide explains how
   to set this.
-  ``access_interface``: type of client that performed the request:
   JDBC, ODBC, JMX, VDP-AdminTool, etc. The table :ref:`Possible values of the attribute "access interface"` of the Administration Guide lists the
   possible values of this attribute.
-  ``session_id``: unique identifier of the session.
-  ``session_start_time``: instant when the session was opened.
-  ``user_name``: user name of the client.
-  ``web_service_name``: (only for SOAP or REST Web services): name of
   the Web service.
-  ``jms_queue_name`` (only for JMS connections): name of the JMS queue
   where the query is being sent from.
-  ``intermediate_client_ip`` (only for SOAP, REST and the global
   RESTful Web service): IP address where the service is running.
   
   If the service has been deployed in the Web container embedded in Denodo, this will be the same IP of the Virtual DataPort server. Otherwise, it is the IP address of the JEE container where the service is deployed.
   
-  ``user_authentication_type``: type of authentication used by the
   client. The values can be “LOCAL”, “LDAP”, “KERBEROS”, "SAML2" or "OAUTH2".
-  ``admin_user``: ``true`` if the user is an administrator user or it
   has the role serveradmin. ``false`` otherwise.
-  ``session_status``: ``true`` if the session is active. ``False``
   otherwise.
-  ``query_running``: query that the client is executing right now and
   that has not finished yet. Empty, if there is no active query.
-  ``query_running_id``: unique identifier of the query that the client
   is executing right now. Empty, if there is no active query.
-  ``query_running_queued``: true if the client has executed a query but
   the query is waiting because the Server has reached the “Max
   concurrent requests” limit.
   
   See more about this limit in the section :ref:`Limiting the Number of Concurrent Requests` of
   the Administration Guide.
   
-  ``last_executed_query``: last query executed by the client.
-  ``last_executed_query_end_time``: instant when the last query
   finished.

-  ``active_transaction`` (boolean): true if the session is running a transaction.
-  ``single_user_mode`` (boolean): true if the statement that this session is currently running switched one or more
   databases to single user mode. This occurs when the query running creates a view, a web service, etc.)
-  ``single_user_mode_databases``: comma-separated list of databases that have been
   switched to “single user mode”. A single statement can switch several databases to single user mode when it affects several databases. For example, if you create a view in database DB_1 that projects a view of DB_2, both databases will
   be switched to single user mode during the creation of the view.

.. rubric:: Privileges Required

The information returned by this procedure depends on the privileges of
the user.

-  Administrator users will obtain information about all the sessions.
-  Users that are administrators of a database will obtain information
   about all the sessions connected to that database.
-  Other users will only obtain information their own sessions.
