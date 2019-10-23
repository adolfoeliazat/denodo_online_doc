===================================================================================
SOAP Over JMS on Web Services Created with Previous Versions of the Denodo Platform
===================================================================================

.. warning:: This section is about the Web services created with previous
   version of the Denodo Platform.

SOAP is transport-independent and can be bound to any protocol. Although
it is usually used with HTTP, it can also be used with JMS (`Java Message
Service <https://www.oracle.com/technetwork/java/jms/>`_). When using SOAP over JMS
(`SOAP over Java Message Service <https://www.w3.org/TR/soapjms/>`_), 
the client sends the SOAP message to
the JMS server, which forwards it to the Web service. Then, the Web
service sends the response back to the JMS server, which forwards it to
the client.

SOAP over HTTP is more interoperable as there is more support for it.
However, there are certain factors that you can only achieve using SOAP
over JMS:

-  **Scalability**. By using SOAP over JMS, clients can send requests
   without waiting for the response. That way, the server does not have
   to process all the requests as they arrive and clients are not
   blocked waiting for a response.
-  **Reliability**. The JMS server ensures that requests and responses
   are delivered. In case of any failure in the communication, the JMS
   server keeps trying to send the messages. This is important in
   environments that deal with critical data.

Virtual DataPort can subscribe to a JMS server to listen to SOAP
messages. Follow these steps to enable this feature:

#. Click on the **SOAP** tab.
#. Click on SOAP over JMS.
#. Select **On** to enable SOAP over JMS support
#. **Destination** is the name of the queue or topic that Virtual
   DataPort will subscribe to, waiting for SOAP messages.
   Depending on the vendor of the JMS server, you might have to create
   the destination, or it is created automatically when the new Web
   service tries to subscribe to it.
#. Select **Queue** or **Topic**.
#. **User name** and **Password**. Credentials to connect to the JMS
   server.
#. **JMS vendor**. Select **GENERIC** if the vendor of the JMS server is
   not in the list and it can be accessed via JNDI. In this case, you
   have to provide the appropriate JNDI connection properties (see
   appendix :ref:`JMS Connection Details: JNDI Properties`).
#. Click **Ok**.

When you deploy the Web service, it connects to the selected JMS server
and subscribes to the JMS queue or topic, so it can receive the messages
sent to that destination.

.. important:: A Web service with SOAP over JMS enabled needs the
   client jars of the JMS server to establish a connection with it. The
   section :doc:`Installing the JMS Connectors to Create JMS Listeners and Web
   services with SOAP over JMS <../../../../platform/installation/postinstallation_tasks/postinstallation_tasks_in_virtual_dataport/installing_the_jms_connectors_to_create_jms_listeners_and_web_services_with_soap_over_jms>` 
   of the Installation Guide explains how to
   install these client jars into the Denodo Platform.

If the Web service is deployed into an external application server, you
have to copy these jars into the ``/WEB-INF/lib`` directory of the
``war`` file, before deploying it.
