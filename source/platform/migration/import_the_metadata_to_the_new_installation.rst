===========================================
Import the Metadata to the New Installation
===========================================

Follow the steps of this section to import the metadata of the previous
installation to the new one.

#. Stop all the servers and tools of the **current** installation.

#. Copy the external classes and libraries that *you* added to the
   directory :file:`{<OLD_DENODO_HOME>}/extensions/thirdparty/lib` to
   :file:`{<NEW_DENODO_HOME>}/extensions/thirdparty/lib`.

  
   -  Do **not** copy the JDBC drivers you copied there. Denodo 7.0 creates a folder in :file:`{<NEW_DENODO_HOME>}/lib-external/jdbc-drivers` for each supported JDBC adapter whose driver is not included in the Denodo Platform. You should copy the drivers to the appropriate subdirectory. 
   
      For example, copy the Vertica driver to :file:`{<NEW_DENODO_HOME>}/lib-external/jdbc-drivers/vertica-7`

   -  Do **not** copy the following files to the new installation (depending on your current version, these files may exist or not):

      -  denodo-vdp-jdbcdriver-basic.jar
      -  derbyclient-\*.jar
      -  jtds-\*.jar
      -  ojdbc14-10.2.0.3.jar
      -  oracle-driver-\*.jar
      -  postgresql-\*.jar
   
      
   
      The installer of the previous version of Denodo copied these files to
      that folder. Copying them to the Denodo |version| will cause
      interoperability problems.
      
      Some of these files may not exist depending on the version of
      Denodo you are upgrading.

#. If the cache engine on the previous installation uses the “Generic”
   cache adapter, copy the files ``cacheConfig-generic.xml`` and
   ``cacheConfig-generic-unicode.xml`` of the directory
   :file:`{<OLD_DENODO_HOME>}/conf/vdp`, to :file:`{<NEW_DENODO_HOME>}/conf/vdp`.

#. If on the previous installation you enabled SSL, copy the keystore file to the new installation. To check if SSL was enabled, open the file :file:`{<OLD_DENODO_HOME>}/conf/vdp/VDBConfiguration.properties` and check if the property ``com.denodo.security.ssl.enabled`` is uncommented and is ``true``. If it is, copy the file of the property ``com.denodo.security.ssl.keyStore`` to the new installation.
   
#. Start all the servers of the **new** Denodo Platform |version|.

#. Import the metadata to **Virtual DataPort** and ITPilot’s **Wrapper
   server** using the *import* script of the **new** installation. To do this,
   open a command line and execute the following:
 
.. code-block:: bash
   
   cd <NEW_DENODO_HOME>
   cd bin
   import --file export_from_denodo_60.vql --server //localhost:9999/admin?<administrator user>@<password> --singleuser 
 
..

   Alternatively, you can do this from the wizard **File** > **Import** of the Virtual DataPort
   Administration tool.

   The section :doc:`Exporting and Importing the Server Metadata <../../vdp/administration/exporting_and_importing_the_server_metadata/exporting_and_importing_the_server_metadata>`
   of the Virtual DataPort Administration Guide provides more details
   about how to use the *import* script and the “File > Import” wizard.


7. Import the metadata of the **ITPilot Wrapper Generation Tool**: open the Wrapper Generation Tool, click
   **File** > **Import**  and select the file exported in the :ref:`step 8 <export_the_metadata_of_the_current_installation_step_8>` of section "Export the Metadata of the Current Installation".

#. Import the metadata of **Scheduler**: use the *import* script
   located at :file:`{<NEW_DENODO_HOME>}/tools/scheduler`.

#. Once you finish setting-up Denodo 7.0, run all the Scheduler jobs of the type "VDP Indexer" to index the data again. The indexes generated with Denodo Scheduler for the Information Self-Service Tool of Denodo 6.0 cannot be migrated to Denodo 7.0. The reason is that their format is incompatible with the Data Catalog of Denodo 7.0 (in Denodo 7.0, the Data Catalog replaces the Information Self-Service Tool of 6.0).
