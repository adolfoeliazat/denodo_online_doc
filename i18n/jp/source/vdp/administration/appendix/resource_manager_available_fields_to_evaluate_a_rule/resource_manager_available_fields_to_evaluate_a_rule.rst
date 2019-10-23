=====================================================
Resource Manager: Available Fields to Evaluate a Rule
=====================================================

This appendix lists the attributes that you can use in the condition of
a rule of the Resources Manager.

-  ``access_interface``: type of client connected to the Server.
   The table :ref:`Possible values of the attribute "access interface"` lists the
   possible values of this attribute.
   
   To create a rule that only applies to the users of the Data Catalog, create a rule with the condition 
   ``access_interface in ('Data-Catalog', 'JDBC')``. You need to specify "JDBC" as well because the Data Catalog executes some requests using its own access interface and others, like a regular JDBC application.

   This also applies to the Scheduler, the Diagnostic Monitoring Tool and the Solution Manager.
   
-  ``admin_user``: is ``true`` if the user is an administrator user or
   it has the role ``serveradmin``. ``false`` otherwise.
-  ``client_ip``: IP address of the client. For clients of web services
   this is the IP address of the final client. I.e. the one that sends
   the HTTP request.
-  ``connection_start_time``: instant when the connection is opened.
   This can be useful to forbid opening new connections at a specific time of the day.
   
   .. note:: To create a rule based on the instant when the query is received by the Server, use the function "now()" instead of "connection_start_time".

   In the following subsection, find some examples of how to forbid queries at certain times of the day or the week.

-  ``database_name``: database that the user is connected to.
-  ``intermediate_client_IP`` (only for SOAP, REST and the global
   RESTful Web service): IP address where the service is running.
   
   If the service has been deployed in the Web container embedded in Denodo, this will be the same IP of the Virtual 
   DataPort server. Otherwise, it is the IP address of the JEE container where the service is deployed.
   
-  ``JMS_queue_name`` (only for JMS connections): name of the JMS queue
   where the query is being sent from.
-  ``last_executed_query``: last query executed by this user.
-  ``query_running``: query that the user is executing right now and
   that has not finished yet. Empty, if there is no active query.
-  ``query_running_queued``: ``true`` if the user already has executed a
   query and the query is waiting because the Server has reached the
   “Max concurrent requests” limit.
   See more about this limit and how to increase it in the section
   :ref:`Limiting the Number of Concurrent Requests`.
-  ``(roles).value``: attribute to create conditions over the roles
   assigned to the user that is opening the connection to Virtual
   DataPort.
   
   Example: if the condition of a rule is  
   
   .. code-block:: sql
      
      (roles).value = 'developer' or (roles).value = 'tester'
      
..
  
   the plan will be applied if at least one of the roles granted to the user is developer or tester.
   
   
-  ``user_agent``: name of the application that opens the connection.
   Setting the user agent in the application is useful to know which
   application opens each connection.
   The section :ref:`Setting the User Agent of an Application` explains how
   to set this.
-  ``user_authentication_type``: type of authentication used by the
   client. The values can be “LOCAL”, “LDAP” or “KERBEROS”.
-  ``user_name``: login name of the user that opened the connection.
-  ``web_service_name`` (only for SOAP or REST Web services): name of
   the Web service.

Examples of Resource Manager Rules
==================================

.. rubric:: Example 1

To forbid any query to run on weekends, do this:

1. Create a plan with the restriction “Stop query always”.

2. Create a rule with this condition:

::

   (getdayofweek(current_date) = 7 OR getdayofweek(current_date) = 1)

When a query is executed, before the actual execution starts, the resource manager checks this condition. If it is true, it immediately stops the query.

The result of :ref:`GETDAYOFWEEK` depends on the i18n of the Server. This example assumes that the i18n of the Server is "us_pst" in which 7 is Saturday and 1, Sunday. If the i18n is "es_euro" (Spain), Saturday is 6 and Sunday, 7.

.. rubric:: Example 2

To forbid any query from starting after 6 PM or before 7 AM, do this:

1. Create a plan with the restriction “Stop query always”.

2. Create a rule with this condition:

::

   (gethour(now()) >= 18 OR gethour(now()) < 7)


.. rubric:: Example 3

To forbid any query to be run if the connection was opened after 6 PM and before 7 AM, do this:

1. Create a plan with the restriction “Stop query always”.

2. Create a rule with this condition:

::

   (gethour(connection_start_time) >= 18 OR gethour(connection_start_time) < 7)
   
If the connection was established before 6 PM, the query will go through, even if it is executed later. If instead of using "connection_start_time", you use "now()", the query will be stopped.

