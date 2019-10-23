===============================================
Export the Metadata of the Current Installation
===============================================


.. note:: You only need to export the metadata of the modules that you
   installed in the current setup of the Denodo Platform.

1. Install the latest update for the Denodo Platform 6.0 on the **current**
   installation. To obtain the latest update, go to the `support site <https://support.denodo.com/>`_.

   If you do not want to install permanently the latest update on the
   current installation, follow these steps:

   i. Stop all the servers of the current installation.
   #. Make a copy of the folder of the current installation.
   #. Install the latest update of Denodo 6.0.
   #. Perform the steps explained in this section.
   #. Delete the folder of the installation with the latest and put back in
      place the folder copied in step ii.

#. Start the Virtual DataPort server and its administration tool of the **current** installation.

#. Denodo 7.0 removed some deprecated authentication methods from published web services. Web services with these authentication methods must be edited using the Administration Tool before exporting the VQL, assigning any of the authentication methods supported in Denodo 7.0. Section :ref:`Published Web Services: “with LDAP” Authentication` lists the removed authentication methods.

#. Log in as an administrator, open the VQL Shell and execute this:
   
    .. code-block:: vql

       SET 'com.denodo.vdb.catalog.exportMigrationCompatibility' = 'true';

    ..
 
   This property makes the Virtual DataPort server to generate VQL statements adapted for Denodo |version|. This property is only useful when upgrading from Denodo 6.0, Denodo 5.5 or 5.0. It  does not do anything in earlier versions.
   
   Setting this property is necessary because:
   
   i. Denodo 6.0 and Denodo 7.0 introduce minor changes to the VQL syntax to avoid
      ambiguities in the parsing of VQL statements. These changes are
      not backwards compatible with earlier versions.
   
      With this property, Virtual DataPort generates VQL
      statements that are compatible with Denodo |version|. The section :doc:`Changes in VQL Syntax <./backward_compatibility/changes_in_the_denodo_platform_7.0/changes_in_virtual_dataport>` lists these changes.

   #. In Denodo 7.0, Denodo databases no longer have their own i18n configuration. Instead, they use the i18n configuration of the Denodo server. Because of this change, the statement ``ALTER DATABASE`` no longer admits the parameter ``I18N``. If you export from Denodo 6.0 and you set this property, the statements ``ALTER DATABASE`` will not have this parameter.

   #. If you export from Denodo 6.0, the VQL statements that create stored procedures will include the parameter ``USE_DENODO_6_0_TYPE_MAPPING``. This parameter instructs the Denodo server 7.0 to handle the input and output parameters of these procedures as in Denodo 6.0.
   
   The section :ref:`changes in Virtual DataPort 7.0 <Changes in Virtual DataPort 7.0>` provides more details about the changes in the VQL syntax.

5.  Execute this to generate VQL statements so the views take advantage of the new data types for date fields.

    .. code-block:: vql

       SET 'com.denodo.vdb.catalog.exportMigrationCompatibility.migrateDateTypes' = 'true';
       
    ..

   This property is only useful when upgrading from Denodo 6.0. Previous versions ignore this property.

   Denodo 7.0 provides new types for date and timestamp values: ``localdate``, ``time``, ``timestamp`` or ``timestamptz``. These types provide several advantages over the existing ``date`` type. However, ``date`` is still supported to keep backward compatibility.

   *Without* this property, the fields of type ``date`` are exported with the same type.
   
   With this property, the Denodo server analyzes the ``date`` fields to see if they can be exported to one of the new types:

   -  For JDBC base views, Virtual DataPort looks at the field "sourceTypeName" of the statement ``CREATE WRAPPER JDBC`` of the base view, and the adapter of the data source of the base view. With this information, it will export the date fields with one of the new types.
   
   -  For non-JDBC base views or derived views, it looks into the property "sourceTypeId" of the field of the ``CREATE TABLE`` statement:

      -  If the property "sourceTypeId" is ``DATE``, the field is exported as ``localdate``, which is the name of the type in Denodo to represent date values without time (equivalent to type DATE of SQL). 
      
      -  If it is ``TIME``, the field is exported as ``time``.
      
      -  If it is ``TIMESTAMP``, the field is exported as ``date`` (deprecated). There is not enough information to know if the field is timestamp or timestamptz.
      
      -  If the field does not have a source type, the field is exported as ``date (deprecated)``. 
   
   If there is not enough information to export the field with one of the new date types, the field is exported as ``date``. This will occur in non-JDBC base views whose fields do not have the source type id defined, or for fields of derived views that are the result of applying a function.

   We suggest setting this property because, although it may introduce a few backward compatibility issues with existing clients, these can be quickly solved. In the long run, by using the new date types, you will avoid the problems introduced by the fact that the ``date`` type in Denodo represents a timestamp with time zone while in many cases the underlying  column in the SQL table has a type without time zone.

6. Restart the Virtual DataPort server to apply the change to the value of these properties.

#. Start all the servers of the **old installation**. If Virtual
   DataPort was already started, restart it to apply the change of the
   previous step.

#. Export the metadata of **Virtual DataPort** and the ITPilot’s **Wrapper
   server** using the script *export* of the **current** installation. To
   do this, open a command line and execute this:

   .. code-block:: batch

      cd <OLD_DENODO_HOME>
      cd bin
      
      export --login <login of an administrator user> --password <password> --server localhost:9999 -f export_from_denodo_60.vql -P includeJars=yes -P includeScanners=yes -P includeCustomComponents=yes -P includeStatistics=yes -P includeDeployments=yes -P replaceExistingElements=yes -P dropElements=no -P excludeServerConfigurationProperties=com.denodo.vdb.vdbinterface.server.VDBManagerImpl.registryURL,com.denodo.vdb.vdbinterface.server.VDBManagerImpl.registryPort,com.denodo.vdb.vdbinterface.server.VDBManagerImpl.shutdownPort,com.denodo.vdb.vdbinterface.server.VDBManagerImpl.factoryPort,com.denodo.vdb.vdbinterface.server.VDBManagerImpl.odbcPort,com.denodo.vdb.cache.cacheMaintenance,java.env.DENODO_OPTS_START,java.env.DENODO_OPTS_STOP -P excludeWebContainerConfigurationProperties=com.denodo.tomcat.jmx.rmi.port,com.denodo.tomcat.http.port,com.denodo.tomcat.shutdown.port,com.denodo.tomcat.jmx.port,java.env.DENODO_OPTS_START,java.env.DENODO_OPTS_STOP


.. _export_the_metadata_of_the_current_installation_step_8:

9. Export the metadata from the **ITPilot Wrapper Generation Tool**: open the Wrapper Generation Tool and click **File >
   Export**.

   The section :ref:`Migrating Wrappers Between Generation Environments: Import and Export` of
   the ITPilot Generation Environment Guide provides more details about this.

#. Export the metadata from **Scheduler**: use the *export* script located
   in :file:`{<OLD_DENODO_HOME>}/tools/scheduler`.

   The section :doc:`Use of the import/export scripts for backup<../../scheduler/administration/appendix/use_of_the_importexport_scripts_for_backup>`
   explains how to use this script.

#. On the administration tool of Virtual DataPort, execute this:

.. code-block:: vql

   SET 'com.denodo.vdb.catalog.exportMigrationCompatibility' = 'false';
   SET 'com.denodo.vdb.catalog.exportMigrationCompatibility.migrateDateTypes' = 'false';


.. 

   After setting these properties to false and restarting, the VQL will go back to being generated with the usual syntax.

12. Restart the Virtual DataPort server.

