=========================================================================================
Installing the JMS Connectors to Create JMS Listeners and Web Services with SOAP Over JMS
=========================================================================================

To connect to a JMS server, you have to install its connector. JMS
connectors are a set of jars that have to be copied into the Denodo
Platform. You have to do this if you are going to do any of these:

-  Create JMS Listeners: see section :doc:`JMS Listeners <../../../../vdp/administration/jms_listeners/jms_listeners>` of the
   Administration Guide.
   If you plan to do this, copy the client jars of the JMS server to the
   directory :file:`{<DENODO_HOME>}/extensions/thirdparty/lib`.
-  Or, create Web services with SOAP over JMS: see section :doc:`JMS
   Listeners <../../../../vdp/administration/jms_listeners/jms_listeners>` of the Administration Guide.
   
   If you plan to publish Web services with the option SOAP over JMS
   enabled and deploy them in the embedded Web container of the Denodo
   Platform, copy the client jars of the JMS server to the directory
   :file:`{<DENODO_HOME>}/resources/apache-tomcat/lib`.
   
   To deploy Denodo Web services with the option SOAP over JMS enabled,
   in an external application server, copy the JMS client jars into the
   ``/WEB-INF/lib`` directory of the generated war file, before
   deploying it.


JMS Client Jars
==============================================================

This section lists the client jars of the most popular JMS servers. You can find these jars in the installation of the product and copy them to one of the directories indicated above.

.. table:: 

   +-----------------+-------------------------------+-----------------------------------+
   | Vendor          | Location of these Jars in the | Jars you Have to Copy to the      |
   |                 | Installation of the Vendor    | Denodo Installation               |
   +=================+===============================+===================================+
   | Apache ActiveMQ | <ACTIVE_MQ_HOME>/lib          | activemq-broker.jar               |
   |                 |                               |                                   |
   |                 |                               | activemq-client.jar               |
   |                 |                               |                                   |
   |                 |                               | activeio-core.jar                 |
   |                 |                               |                                   |
   |                 |                               | geronimo-spec-j2ee-management.jar |
   |                 |                               |                                   |
   |                 |                               | hawtbuf.jar                       |
   +-----------------+-------------------------------+-----------------------------------+
   | IBM WebSphere   | <WEBSPHERE_MQ_HOME>/java/lib  | com.ibm.mq.jar                    |
   | MQ 7.0          |                               |                                   |
   |                 |                               | com.ibm.mqjms.jar                 |
   |                 |                               |                                   |
   |                 |                               | com.ibm.mq.jmqi.jar               |
   |                 |                               |                                   |
   |                 |                               | dhbcore.jar                       |
   |                 |                               |                                   |
   |                 |                               | fscontext.jar                     |
   |                 |                               |                                   |
   |                 |                               | jta.jar                           |
   |                 |                               |                                   |
   |                 |                               | providerutil.jar                  |
   +-----------------+-------------------------------+-----------------------------------+
   | IBM WebSphere   | <WEBSPHERE_MQ_HOME>/java/lib  | com.ibm.mq.jar                    |
   | MQ 8.0          |                               |                                   |
   |                 |                               | com.ibm.mqjms.jar                 |
   |                 |                               |                                   |
   |                 |                               | com.ibm.mq.jmqi.jar               |
   |                 |                               |                                   |
   |                 |                               | fscontext.jar                     |
   |                 |                               |                                   |
   |                 |                               | providerutil.jar                  |
   +-----------------+-------------------------------+-----------------------------------+
   | Progress        | <SONIC_MQ>/Sonic/MQ8.0/lib    | mfcontext.jar                     |
   | SonicMQ MQ 8.0  |                               |                                   |
   |                 |                               | sonic_Client.jar                  |
   |                 |                               |                                   |
   |                 |                               | sonic_Crypto.jar                  |
   |                 |                               |                                   |
   |                 |                               | sonic_XA.jar                      |
   |                 |                               |                                   |
   |                 |                               | sonic_XMessage.jar                |
   +-----------------+-------------------------------+-----------------------------------+
