===============================================
Changes Common to All the Modules of Denodo 7.0
===============================================

.. contents::
   :local:
   :backlinks: none

Java 8
======

The Denodo servers and its tools need to run with Java 8. They cannot run with earlier Java versions. 

Client applications that connect to the JDBC interface of Denodo need to run with Java 8, 9 10 or 11. Otherwise, they will not be able to load the Denodo JDBC driver.

Extensions
==========

In Denodo 7.0, the logging library (log4j) has been updated to version 2. In Denodo 6.0, the Virtual DataPort server uses Apache Log4j 2 as well but the other components use Log4j 1.x. If in the previous version you customized the log4j.xml of the other modules, adapt them to use the syntax of Log4j 2.

API Documentation
=================
The Javadoc of the Denodo APIs is now published online `the documentation <https://community.denodo.com/docs/html/browse/7.0/>`_. It is no longer included with the Denodo Platform. In previous versions it is located on the folder <DENODO_HOME>/docs.

Web Container (Tomcat)
======================

SSL/TLS
-------

Now, when SSL/TLS is enabled, TLSv1.2 is used by default to secure the connections to the web container embedded in Denodo. This applies to all the incoming connections of Tomcat. I.e. the https connections from external clients and to the connections from the Virtual DataPort server to the Tomcat.

Encoding Square Brackets in the URL
-----------------------------------

In Denodo 7.0, the web container requires that in the URL of the requests it receives, the square brackets (``[`` and ``]``) have to be encoded. That is, they have to be converted to ``%5B`` and ``%5D`` respectively.

Even though the standard requires these characters to be encoded, in previous versions of Denodo, these characters could be included URL encoded or decoded. To avoid having to encode these characters, do the following:

#. Edit the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/server.xml`.

2. Add the following attribute to all the **Connector** elements:
   ``relaxedQueryChars="[]"`` if needed. Note that some of the **Connector**
   elements may have already been automatically updated.
