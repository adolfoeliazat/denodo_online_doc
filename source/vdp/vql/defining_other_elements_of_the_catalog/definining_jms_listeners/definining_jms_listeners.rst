========================
Defining JMS Listeners
========================

Virtual DataPort can subscribe to a JMS server to listen to requests.
Therefore, clients, instead of connecting to Virtual DataPort via JDBC,
ODBC or a Web service, can send a request to a JMS server, which
forwards it to Virtual DataPort. Then, the response is sent back to a
queue or a topic of the JMS server, which forwards it to the client/s.

When creating a JMS listener, you can set it up to:

-  Execute the VQL statements received from a JMS server.
-  Or, define a query with the interpolation variable
   (``@JMSEXPRESSION``) in the JMS listener and at runtime, replace
   this variable with the values received from the JMS server.

The output of a JMS listener can be either an XML document or a JSON
document.

Example of how the **option a)** works: a client sends a message such as
``SELECT * FROM internet_inc WHERE iinc_id = 1`` to the JMS server.
The JMS server will forward this to Virtual DataPort, which will send a
response back like `XML response message sent by a JMS listener`_ if the
selected output is XML, or like `JSON response message sent by a JMS
listener`_ if the selected output is JSON.



.. code-block:: xml
   :caption: XML response message sent by a JMS listener
   :name: XML response message sent by a JMS listener

   <?xml version="1.0" encoding="UTF-8"?>
   <response>
       <item>
           <iinc_id>1.00</iinc_id>
           <summary>Error in ADSL router</summary>
           <ttime>29-jun-2005 19h 19m 41s</ttime>
           <taxid>B78596011</taxid>
           <specific_field1>1</specific_field1>
           <specific_field2>1</specific_field2>
       </item>
   </response>


.. code-block:: json
   :caption: JSON response message sent by a JMS listener
   :name: JSON response message sent by a JMS listener

   [
       {
           "IINC_ID": 1,
           "SUMMARY": "Error in ADSL router",
           "TTIME": "2005-06-29",
           "TAXID": "B78596011",
           "SPECIFIC_FIELD1": "1",
           "SPECIFIC_FIELD2": "1"
       }
   ]
   
If the request is a DML sentence such as
``ALTER VIEW incidents CACHE INVALIDATE``, the response will be empty
(see `XML response message to a DML query`_ and `JSON response message
to a DML query`_).



.. code-block:: xml
   :caption: XML response message to a DML query
   :name: XML response message to a DML query

   <?xml version="1.0" encoding="UTF-8"?>
   <response />



.. code-block:: json
   :caption: JSON response message to a DML query
   :name: JSON response message to a DML query

   []


Example of how **option b)** works: if you have created a JMS listener
with the following query:



.. code-block:: vql
   :caption: Sample query defined in a JMS listener
   :name: Sample query defined in a JMS listener

   SELECT * FROM incidents
   WHERE taxid = '@JMSEXPRESSION'


When the listener receives the value ``B78596014``, the Server will
execute the following query:



.. code-block:: sql

   SELECT * FROM incidents
   WHERE taxid = 'B78596014'
   
If the query defined in the JMS listener does not contain the
``@JMSEXPRESSION`` variable, the value received from the listener is ignored and the query is executed “as is”.

The following figures contain the syntax of the various commands to deal
with JMS listeners.



.. code-block:: bnf
   :caption: Syntax of the CREATE LISTENER JMS statement
   :name: Syntax of the CREATE LISTENER JMS statement

   CREATE [ OR REPLACE ] LISTENER JMS <name:identifier>
       VENDOR { ACTIVEMQ | WEBSPHEREMQ | JNDI }
       DESTINATION = <name:literal>
       [ IGNOREREPLYTO ]
       [ REPLYTO = <name:literal> ]
       { QUEUE | TOPIC }
       OUTPUT = { XML | JSON }     
       [ USER = <name:literal> PASSWORD = <name:literal> ]
       VDPUSER = <name:literal>
       [ 
         MESSAGESCONTAINVARIABLE = { TRUE | FALSE } 
         QUERY = <query:literal> 
       ]
       ENABLED = { TRUE | FALSE }
       PROPERTIES ( <property> [, <property> ]* )
       [ FOLDER = <folder:literal> ]
       [ DESCRIPTION = <description:literal> ]
   
   <property> ::=
       <key:literal> = <value:literal>

-  ``VENDOR``. Use the value ``JNDI`` if the JMS server that this listener
   will connect to, is neither Apache Active MQ nor IBM WebSphere MQ.

-  ``DESTINATION`` is the name of the queue or topic that Virtual
   DataPort will subscribe to, waiting for requests.
   
   Depending on the vendor of the JMS server, we might have to create the
   JMS destination, or it will be created automatically when the listener
   tries to subscribe to it.

-  ``REPLYTO`` is the name of the JMS queue or topic where the responses
   will be sent to.
   
   The responses will always be sent to this destination, even if the JMS
   message contains the “Reply to” field.
   
   If the clause ``REPLYTO`` not present, the responses will be sent to
   the destination indicated in the “Reply to” field of the JMS request.
   
   If the JMS request does not have this field either, the Server will
   not send any response.
   
   If the clause ``IGNOREREPLYTO`` is present, the listener will never
   send a response.

-  ``QUEUE`` or ``TOPIC``. Use one parameter or the other depending on the
   type of the destination that this listener will connect to.

-  ``OUTPUT``. If the parameter is ``XML`` the output of the listener will
   be an XML document. If it is ``JSON``, a JSON document.

-  ``USER`` and ``PASSWORD``. They are the credentials to access the JMS
   server.

-  ``VDPUSER`` is the username that Virtual DataPort will use to check if
   the listener has enough privileges to execute a query. E.g. if our
   Virtual DataPort server has two users:

   -  ``admin``: is an “administrator” or a user promoted to “local
      administrator” of the database so it can access any view of the
      database.
   -  ``user1``: is a “normal user” that only has ``EXECUTE`` privileges over this
      database.

   If the parameter ``VDPUSER`` is “admin”, the listener will be able to
   execute any query. However, if ``VDPUSER`` is ``user1``, the
   ``CREATE``, ``DROP``, ``INSERT``, ``UPDATE`` and ``DELETE`` queries will
   fail because ``user1`` only has ``EXECUTE`` privileges.

-  ``MESSAGESCONTAINVARIABLE`` and ``QUERY``: if present and
   ``MESSAGESCONTAINVARIABLE`` is ``true``, when the listener receives a
   JMS message, the Server will replace the variable ``@JMSEXPRESSION`` of
   ``QUERY`` with the content of the JMS message. Then, the Server will
   execute the resulting query.

-  ``ENABLED``. ``TRUE`` to enable the listener. That is, after creating
   it, the listener will try to connect to the JMS server to listen to
   requests. ``FALSE``, to disable it.

-  ``PROPERTIES``. List of properties that will be used to obtain a
   connection to the JMS server. The appendix :doc:`/vdp/administration/appendix/jms_connection_details_jndi_properties/jms_connection_details_jndi_properties`
   of the Administration Guide contains a list of the
   properties required to connect to the most popular vendors.
   | Usually, the JMS listeners need these properties, at least:

   -  ``java.naming.factory.initial``
      (``javax.naming.Context.INITIAL_CONTEXT_FACTORY``)
   -  ``java.naming.provider.url`` (``javax.naming.Context.PROVIDER_URL``)
   -  ``transport.jms.ConnectionFactoryJNDIName``. Name of the connection
      factory in the JNDI context of the JMS server.

-  ``FOLDER``. Path of the folder where the listener will be stored. E.g.:
   ``FOLDER = '/jms listeners'``.

-  ``DESCRIPTION``. Description of the listener.


.. code-block:: vql 
   :name: Command to enable/disable a JMS listener: ALTER LISTENER JMS
   :caption: Command to enable/disable a JMS listener: ALTER LISTENER JMS
   
   ALTER LISTENER JMS <name:identifier>
   ENABLED EQ { true \| false }                                             

If ``ENABLED = TRUE`` and it was ``FALSE``, it tries to connect to the
JMS server to listen to requests.

If ``ENABLED = FALSE`` and it was ``TRUE``, it closes the connection
with the JMS server.

