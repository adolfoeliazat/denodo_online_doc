=============
JMS Listeners
=============
.. toctree::
   :hidden:
   
   creating_a_new_jms_listener/creating_a_new_jms_listener.rst
   who_can_create_jms_listeners/who_can_create_jms_listeners.rst


Virtual DataPort can subscribe to a JMS server (`Java Message
Service (JMS) <https://www.oracle.com/technetwork/java/jms/>`_) to listen
to requests. Therefore, clients, instead of connecting to Virtual
DataPort via JDBC, ODBC or a Web service, can send a request to a JMS
server, which forwards it to Virtual DataPort. Then, the response is
sent back to a queue or a topic of the JMS server, which forwards it to
the client/s.

When creating a JMS listener, you can set it up to:

-  Execute the VQL statements received from a JMS server.
-  Or, define a query with the interpolation variable
   (``@JMSEXPRESSION``) in the JMS listener and at runtime, replace
   this variable with the values received from the JMS server.

The output of a JMS listener can be either an ``XML`` document or a
``JSON`` document.

Example of how the **option a)** works: a client sends a message such as
``SELECT * FROM internet_inc WHERE iinc_id=1`` to the JMS server.
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
   
   [{
      "IINC_ID": 1,
      "SUMMARY": "Error in ADSL router",
      "TTIME": "2005-06-29",
      "TAXID": "B78596011",
      "SPECIFIC_FIELD1": "1",
      "SPECIFIC_FIELD2": "1"
   }]


If the request is a DML sentence such as
``ALTER VIEW incidents CACHE INVALIDATE``, the response will be
empty (see `XML response message to a DML query`_ and `JSON response
message to a DML query`_)

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

 
.. code-block:: sql
   :caption: Sample query defined in a JMS listener
   
   SELECT * 
   FROM incidents 
   WHERE taxid = '@JMSEXPRESSION'


When the listener receives a value like ``B78596014``, the Server will
execute the following query:

.. code-block:: sql
   :caption: Sample query defined in a JMS listener
   
   SELECT * 
   FROM incidents 
   WHERE taxid = 'B78596014'


If the query defined in the JMS listener does not contain the
``@JMSEXPRESSION`` variable, the value received from the listener is
ignored and the query will be executed “as is”.

Advanced JMS Configuration
==========================

There are several configuration options available to control how JMS messages
are processed by Denodo JMS listeners. 

All of these options must be configured in
:file:`{<DENODO_HOME>}/conf/vdp/VDBConfiguration.properties` and require
restarting the Virtual DataPort server.

Acknowledge Mode
----------------

JMS messages acknowledge mode, as defined by the 
`JMS documentation <https://javaee.github.io/javaee-spec/javadocs/index.html?javax/jms/Session.html>`_, 
can be configured with property ``com.denodo.jms.acknowledgeMode``. This
property must be set to one of the following values:

#. ``AUTO_ACKNOWLEDGE`` (default value)

2. ``CLIENT_ACKNOWLEDGE``

3. ``DUPS_OK_ACKNOWLEDGE``

Acknowledge On Query Finish and Serialized Processing
-----------------------------------------------------

When "Acknowledge On Query Finish" mode is enabled, JMS listeners will 
acknowledge the reception of a message only after the query associated to it has
finished.

To enable this mode, set ``com.denodo.jms.acknowledgeMode=CLIENT_ACKNOWLEDGE`` 
and ``com.denodo.jms.acknowledgeOnQueryFinish=true``. If you wish to add 
specific configuration for particular JMS listeners, you can add as many 
parameters like this as necessary: 
``com.denodo.jms.acknowledgeOnQueryFinish.<jms_listener_database>.<jms_listener_name>={true|false}``.

"Acknowledge On Query Finish" mode is disabled by default.

When "Serialized Processing" mode is enabled, JMS listeners will only process
one JMS message at a time. 

To enable this mode, set ``com.denodo.jms.serializeMessageProcessing=true``. 
If you wish to add specific configuration for particular JMS listeners, you can 
add as many parameters like this as necessary: 
``com.denodo.jms.serializeMessageProcessing.<jms_listener_database>.<jms_listener_name>={true|false}``.

"Serialized Processing" mode is disabled by default.

Activating both modes ("Acknowledge On Query Finish" and 
"Serialized Processing") is useful when you need to make sure that all the
messages in the queue that a JMS listener is subscribed to will be executed by 
Virtual DataPort. 

You may activate "Serialized Processing" only (without activating "Acknowledge 
On Query Finish") if the messages of a particular queue define long queries and 
you need the queue to be emptied as soon as possible: in this case, there will 
be no guarantee that all the queries defined by the messages in the queue are 
actually executed by Virtual DataPort.

Enabling Advanced Logging for JMS Listeners
===========================================

You can enable advanced logging for JMS listeners. This way, whenever a JMS 
message issues a query that fails, the following information will be logged:

#. The JMS listener which received the message.

2. JMS message metadata (id, timestamp, etc.).

3. VQL query executed by the listener.

4. Obtained error.

In order to activate the advanced logging for JMS listeners, follow these steps:

#. Open the file :file:`{<DENODO_HOME>}/conf/vdp/log4j2.xml` in an editor.

2. Add a new appender like this (replace :file:`{<DENODO_HOME>}` with the 
   actual path):
   
   .. code-block:: xml
      :caption: JMSOUT appender
      :name: JMSOUT appender
      
      <RollingFile name="JMSOUT" 
                   fileName="<DENODO_HOME>/logs/vdp/vdp-jms${env:vdp.instance.log}.log" 
                   filePattern="<DENODO_HOME>/logs/vdp/vdp-jms${env:vdp.instance.log}.log.%i">
             <Policies>
                   <SizeBasedTriggeringPolicy size="10 MB" />
             </Policies>
             <DefaultRolloverStrategy max="7" />
             <PatternLayout pattern="%-4r [%t] %d{yyyy-MM-dd'T'HH:mm:ss.SSS} %x -\t%m  %n" />
      </RollingFile>

3. Add a new logger:

   .. code-block:: xml
      :caption: com.denodo.jms.verbose logger
      :name: com.denodo.jms.verbose logger
     
      <Logger name="com.denodo.jms.verbose" level="debug" additivity="false" >
             <AppenderRef ref="JMSOUT" />
      </Logger>
   
This way, the detailed error information will be logged to
:file:`{<DENODO_HOME>}/logs/vdp/vdp-jms.log`.
