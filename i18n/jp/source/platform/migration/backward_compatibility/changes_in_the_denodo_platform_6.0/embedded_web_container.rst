==========================================================================
Changes in the Embedded Web Container of Denodo 6.0
==========================================================================

Location of the Log4j Configuration File
============================================================================

In Denodo 6.0, the log4j.xml file is in the directory
:file:`{<DENODO_HOME>}/resources/apache-tomcat/lib/log4j.xml`.

In earlier versions is in
:file:`{<DENODO_HOME>}/resources/apache-tomcat/common/classes`.



Changes in the Default Configuration of the Logging System
============================================================================

In Denodo 6.0, Tomcat logs message either to the file denodows.log or to
tomcat.log (both files are located in
<DENODO\_HOME>/logs/apache-tomcat).

In earlier versions of the Denodo Platform, some messages were logged to
both files.



Configuration Changes
============================================================================

The Denodo Platform 6.0 includes a more recent version of Apache Tomcat
(the embedded Web container). Because of this change, the following
tasks are performed differently than in earlier versions:

-  Enable SSL in the Web container.
-  Increase the maximum number of simultaneous HTTP requests.

The main effect of this is that the file
:file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/server.xml.template` no
longer exists. Instead, you have to modify the ``server.xml`` directly.


