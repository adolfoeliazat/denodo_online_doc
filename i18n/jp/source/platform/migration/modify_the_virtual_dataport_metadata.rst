====================================
Modify the Virtual DataPort Metadata
====================================

Before importing the VQL file ``export_from_denodo_60.vql`` into the new Denodo server, you may need to modify it.

#. Open the VQL file.

#. Search for the following lines and delete them (you may have all these properties defined): 

   .. code-block:: none
      
      SET 'com.denodo.vdb.interpreter.execution.processor.VDBActionProcessor.enableAutoSingleUserMode'
      
      SET 'com.denodo.vdb.vdbinterface.server.vcs.VCSConfigurationManager.localRepositoriesHome'
      
      SET 'com.denodo.vdb.interpreter.execution.processor.VDBActionProcessor.strictAutoSingleUserMode'
      
      SET 'com.denodo.vdb.catalog.exportMigrationCompatibility'
      
      SET 'com.denodo.vdb.catalog.exportMigrationCompatibility.migrateDateTypes'
      
      SET 'com.denodo.vdb.security.allowLDAPAdministratorsToAssignPrivileges'
      
      SET 'com.denodo.vdb.cache.i18nCode'
      
      SET 'com.denodo.security.ssl.enabled'

  The property "com.denodo.security.ssl.enabled" controls if SSL is enabled on the Virtual DataPort server. We remove this property to avoid enabling SSL until the migration process is completed.

.. rubric:: If you are using the cache engine of Denodo, also do the following:
  
#. Search ``DATASOURCE JDBC vdpcachedatasource`` 
   and modify the value of
   the parameter ``DATABASEURI`` to point to the new schema of the cache
   database. This is the schema you created when following the steps of the section :ref:`Before Beginning the Migration`.
   
   Check if this statement has the parameters ``TARGET_CATALOG`` or ``TARGET_SCHEMA`` and if it does, update their value 
   with the name of the new catalog or schema.

   If the password for this catalog/schema is different, you can obtain the encrypted password with the statement ``ENCRYPT_PASSWORD`` (execute it from the VQL Shell). 
   For example,
   
   ::
   
      ENCRYPT_PASSWORD 'new password' FOR_PROPERTIES_FILE; 

#. Search ``DATASOURCE JDBC customvdpcachedatasource``.
   
   Do the same changes as in the step above. Take into account that:

   -  There may be more than one
      ``CREATE DATASOURCE JDBC customvdpcachedatasource`` statement and you
      have to modify the ``DATABASEURI`` in all of them.
   -  If you do not find this statement, it means that all the databases
      use the global configuration of the cache.
   -  You may find this statement, but without the parameter
      ``DATABASEURI``. In this case, ignore this statement and continue
      searching.

#. Search for the line that starts with:

   .. code-block:: none

      SET 'java.env.DENODO_OPTS_START' = '
   
   In this line, remove ``-XX:MaxPermSize=<value>``. This parameter is not supported by Java 8, the version of Java distributed with Denodo.
   
#. Search ``CATALOG_PERMISSIONS``. If a statement references this stored procedure, you may have to change it because in Denodo 7.0, some of the output fields of this procedure have been renamed. E.g. the output field ``dbread`` has been renamed to ``dbexecute``, ``viewread`` has been renamed to ``elementexecute``, etc.

   Find the full list of changes in the section :ref:`CATALOG_PERMISSIONS Procedure`.

.. rubric:: If upgrading from Denodo 5.5 or 5.0

-  Open the file ``export_from_denodo_60.vql`` generated in the previous section and
   search ``'formatted' = 'yes'``.

   For each occurrence of this parameter inside the ``CONTEXT`` clause of a 
   ``CREATE VIEW`` statement, check that the syntax of the statement is valid 
   in Denodo |version|. The reason is that starting with Denodo 6.0, the syntax of a few 
   operators changed slightly. The section :doc:`Changes in VQL Syntax <./backward_compatibility/changes_in_the_denodo_platform_7.0/changes_in_virtual_dataport>` lists the changes 
   in the VQL syntax.
