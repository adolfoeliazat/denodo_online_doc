=======================================
JMS Connection Details: JNDI Properties
=======================================

This appendix provides the configuration values required to configure a
JMS listener or publish a Web service with SOAP over JMS enabled, so
they can connect to a JMS server:

.. contents:: 
   :depth: 1
   :local:
   :backlinks: none

.. note:: The first time you create a JMS listener or publish a Web
   service with the option SOAP over JMS enabled, you have to install the
   client jars of the JMS server. The section :doc:`Installing the JMS
   Connectors to Create JMS Listeners and Web services with SOAP over JMS <../../../../platform/installation/postinstallation_tasks/postinstallation_tasks_in_virtual_dataport/installing_the_jms_connectors_to_create_jms_listeners_and_web_services_with_soap_over_jms>` 
   of the Installation Guide explains how to do this.

Apache ActiveMQ 5.6.0
=====================

The properties for a default installation are the following:

.. table:: Default values for Apache Active MQ

   +--------------------------------------+--------------------------------------+
   | Property                             | Property value                       |
   +======================================+======================================+
   | JNDI Provider URL                    | \tcp://acme:8161                     |
   +--------------------------------------+--------------------------------------+
   | Broker URL                           | \tcp://acme:61616                    |
   +--------------------------------------+--------------------------------------+


IBM WebSphere MQ 7.0
====================

The properties for a default installation are the following:

.. table:: Default values for IBM WebSphere MQ

   +--------------------------------------+--------------------------------------+
   | Property                             | Property value                       |
   +======================================+======================================+
   | Host                                 | acme                                 |
   +--------------------------------------+--------------------------------------+
   | Port                                 | 1414                                 |
   +--------------------------------------+--------------------------------------+
   | Channel                              | SYSTEM.DEF.SVRCONN                   |
   +--------------------------------------+--------------------------------------+
   | Queue Manager                        | QM\_acme                             |
   +--------------------------------------+--------------------------------------+
   
Progress SonicMQ 8.0
====================

The properties for a default installation are the following:

.. table:: Default JNDI properties for Progress SonicMQ

   +--------------------------------------+--------------------------------------+
   | JNDI property name                   | Property value                       |
   +======================================+======================================+
   | java.naming.provider.url             | acme:2506                            |
   +--------------------------------------+--------------------------------------+
   | java.naming.factory.initial          | com.sonicsw.jndi.mfcontext.MFContext |
   |                                      | Factory                              |
   +--------------------------------------+--------------------------------------+
   | transport.jms.ConnectionFactoryJNDIN | ConnectionFactory                    |
   | ame                                  |                                      |
   |                                      |                                      |
   |                                      | **Note**: This connection factory    |
   |                                      | name has to be defined in the JNDI   |
   |                                      | service of SonicMQ.                  |
   +--------------------------------------+--------------------------------------+