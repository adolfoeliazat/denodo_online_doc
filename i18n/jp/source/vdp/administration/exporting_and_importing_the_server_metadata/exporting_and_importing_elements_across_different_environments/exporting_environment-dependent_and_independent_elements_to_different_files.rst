===========================================================================
Exporting Environment-Dependent and Independent Elements to Different Files
===========================================================================

.. note:: This section describes the parameters
   ``--property includeEnvSpecificElements`` and
   ``--property includeNonEnvSpecificElements=yes`` of the export
   script. They are deprecated and it may not be available in future
   releases of the Denodo Platform.

   They cannot be used to export the new types of environment-dependent
   elements added in Denodo 5.5. For example, a custom wrapper that has an
   input parameter that is a route is an environment dependent element.
   However, it will not be included in the output of the script when you
   use ``--property includeEnvSpecificElements=yes``.

Virtual DataPort can export to one file the elements that depend on the
environment where the Server is running, and to another file, the
environment-independent elements. Then, you can edit the parameters’
values of the first file to match the configuration of the target
environment before importing it. Afterwards, you can import the second
file without any modification.

The elements that are considered dependent on the environment (option
``--property includeEnvSpecificElements=yes`` of the scripts
``export`` and ``import`` are the following:


-  Server settings. This includes the settings of the following items
   (settings that can be changed in the wizards of the menu
   **Administration** > **Server configuration** of the Administration
   Tool):

   -  Cache engine
   -  Concurrent requests
   -  Default I18n
   -  HTTP Proxy
   -  Memory usage
   -  Server connectivity
   -  Stored procedures
   -  Threads pool


-  JVM settings of the Virtual DataPort server and the Denodo Web
   Container (not the JVM settings of ITPilot or Scheduler).
   These are the settings that can be changed in the Denodo Platform
   Control Center, in the dialog “Configuration > JVM Options”.
   
   When these settings are imported, the Server regenerates the following
   scripts so they use the imported values:
   
   .. code-block:: none
   
      <DENODO_HOME>/bin/vqlserver
      <DENODO_HOME>/bin/vqlserver_startup.l4j.ini
      <DENODO_HOME>/bin/vqlserver_itpilot_startup.l4j.ini
      <DENODO_HOME>/conf/vdp/server.conf
      <DENODO_HOME>/resources/apache-tomcat/bin/catalina
      
-  Users, but not their privileges. Their privileges are considered
   independent of the environment.


-  Roles, but not their privileges. Their privileges are considered
   independent of the environment.


-  JMS listeners


-  Database creation and configuration


-  Folders


-  Data sources including Custom data sources (also called Custom wrappers)


-  ITPilot wrappers


-  I18n maps: only those that are referenced by other environment-dependent
   elements. I.e.: databases and ITPilot wrappers.


-  Jars: only those that are referenced by other environment-dependent
   elements and you are using the option to deal with Jars. That is, if
   you are exporting from the Administration Tool, the option **Include
   Jars** is selected. Or, if you are using the scripts ``export`` or
   ``import``, you are using the option
   ``-P includeJars=yes``.


The elements that are considered independent of the environment (option
``--property includeNonEnvSpecificElements=yes`` of the scripts
``export`` and ``import`` are the following:

-  Privileges assigned to users. The creation of users is considered
   dependent on the environment, but not their privileges.
-  Privileges assigned to roles. The creation of roles is considered
   dependent on the environment, but not their privileges.
-  Folders
-  Wrappers, except ITPilot wrappers, which are considered dependent on
   the environment.
-  Types
-  Views (base and derived views)
-  Denodo Stored procedures
-  Web services: includes the creation of the Web services and if
   necessary, the information to deploy them.
-  Widget creation: includes the creation of the widgets and if
   necessary, the information to deploy their auxiliary Web services.
-  Jars: only those that are referenced by other environment-independent
   elements and you are using the option to deal with Jars. That is, if
   you are exporting from the Administration Tool, the option **Include
   Jars** is selected. Or, if you are using the scripts ``export`` or
   ``import``, you are using the option ``--P includeJars=yes``.
-  Simple maps
-  I18n maps: only those that are referenced by other
   environment-independent elements such as views, Web services,
   widgets, etc.
