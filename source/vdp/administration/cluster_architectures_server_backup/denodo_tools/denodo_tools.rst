============
Denodo Tools
============

The Denodo Tools are a toolkit that helps administrators to build a
cluster of Virtual DataPort servers.

These tools are also useful to perform backups of the metadata stored in
a Virtual DataPort server.

The scripts that form the Denodo Tools are:

-  ``ping``: checks that a Virtual DataPort server is “alive”. See
   section :ref:`How to Check if a Virtual DataPort Server is Alive`.
-  ``export``: exports the metadata from a Virtual DataPort server. See
   section :doc:`Export Script <../using_the_import_export_scripts_for_backup_and_or_replication/export_script>`.
-  ``import``: imports metadata into one or more Virtual DataPort
   servers. See section :ref:`Import Script`.
-  ``encrypt_password``: the scripts ``export`` and ``import`` require
   you to fill in the password of the Server. Using this script, you can
   encrypt the password to avoid entering as plain text.

These scripts are located in the path :file:`{<DENODO_HOME>}/bin`. However, if
you want to execute them from another host, you can copy the file
:file:`{<DENODO_HOME>}/tools/db/denodo-db-tools.zip`. This zip file contains
all the scripts and libraries required to use the Denodo Tools.

