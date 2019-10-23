===================================================================================================
How to Debug Kerberos in Web Applications
===================================================================================================

When setting up Kerberos for the Web Administration Tools (Data Catalog, Scheduler Web Administration Tool and Web Panel)
we recommend selecting the check box **Activate Kerberos debug mode** in case you run into any issues. 
   
When this option is enabled, the Java Runtime Environment logs messages
related to Kerberos in the standard output but not in the log files. To
see these messages, you have two options, depending on whether the 
Virtual DataPort server and the Web administration tools are on the same installation or not.

Virtual DataPort Server and the Web Administration Tools are on the Same Installation
======================================================================================

1. Stop the Virtual DataPort server and the web administration tools.

#. Edit the file :file:`{<DENODO_HOME>}/conf/vdp/log4j2.xml` to set the logger:
   
   .. code-block:: guess
   
      <Logger name="com.denodo.tomcat" level="TRACE" />

#. Start the Virtual DataPort server.

#. Wait until the tomcat log stops writing in :file:`{<DENODO_HOME>}/logs/vdp/vdp.log`.

#. Start the web administration tools.

#. Try to log in the web administration tools using Kerberos.

#. See the Kerberos debug messages in the file :file:`{<DENODO_HOME>}/logs/vdp/vdp.log`.
   
   .. note:: Relevant log lines start by *OUTPUT>*.


Virtual DataPort Server and the Web Administration Tools are not on the Same Installation
==========================================================================================

1. Stop the web administration tools.

#. Edit the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/webapps/<web-app>/WEB-INF/classes/log4j2.xml` to set the logger:
   
   .. code-block:: guess
   
      <Logger name="com.denodo.tomcat" level="TRACE" />

#. Start the internal Tomcat by means of a command line interface:

   .. code-block:: bash
   
      <DENODO_HOME>/resources/apache-tomcat/bin/catalina.{bat|sh} start

#. Start the web administration tools by means of the Control Center.

#. Try to log in the web administration tools using Kerberos.

#. See the Kerberos debug messages in the command line.


.. note:: When the Virtual DataPort Server and the Web Administration Tools are on the same installation,
   you could also use this option, but the first one is simpler.