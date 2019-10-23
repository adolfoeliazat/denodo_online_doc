========================================================
Monitoring with a Java Management Extensions (JMX) Agent
========================================================

.. toctree::
   :hidden:
   
   using_javatm_visualvm.rst
   general_information_on_the_server.rst
   information_on_data_sources.rst
   information_on_the_cache.rst
   information_and_events_on_catalog_access_ddl_statements.rst
   information_and_events_on_the_running_of_statements.rst
   information_and_events_on_transactions.rst
   setting_the_user_agent_of_an_application.rst
   

It is possible to access monitoring information of the server using the
`Java Management eXtensions (JMX) standard <https://www.oracle.com/technetwork/java/javase/tech/javamanagement-140525.html>`_.
This information can be used to control the use of the server and audit
the actions carried out on the data sources and/or the Virtual DataPort
metadata.

The available monitoring information is:

-  General information on the server: number of active and inactive
   connections, memory usage, etc.
-  Configuration and accesses to the server cache.
-  Accesses to each data source.
-  Accesses to the views and stored procedures.
-  Transactions executed by the Virtual DataPort server.

DataPort also generates different types of JMX notifications whenever:

-  A DDL statement is executed. E.g. ``ALTER TABLE``, ``CREATE VIEW``,
   etc.
-  A DML statement is executed. E.g. ``SELECT``, ``INSERT``, etc.
-  Transactions start or end.
-  …

In JMX, the information exported is displayed as a set of objects known
as *MBeans*. The *MBeans* exported by Virtual DataPort belong to the JMX domain
``com.denodo.vdb.management.mbeans``.

Any JMX client can be used to access the Virtual DataPort monitoring information
and events. *Visual VM* (included with the Java Developer Kit 1.6
and above), among others, has been
successfully tested to access the information and events exported by the
DataPort server. To use these consoles, usually you need to specify
the URL to the server. If the Virtual DataPort server is
running in the port 9999 in the local host, the URL is:

``service:jmx:rmi:///jndi/rmi://localhost:9999/jmxrmi``

It is also possible to access the MBeans from an external application,
using the JMX API 
(`Java Management eXtensions - JMX <https://www.oracle.com/technetwork/java/javase/tech/javamanagement-140525.html>`_).

The section :ref:`Using JavaTM VisualVM` explains how to use *VisualVM* to
monitor the Virtual DataPort server. The following subsections describe
in further detail the information and events available in each of the
following categories: general server information, general data source
information, general cache information, information and events on DDL
statements, information and events on DML statements and information and
events on transactions.
