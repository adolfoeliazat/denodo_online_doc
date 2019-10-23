==============================================================
Exporting and Importing Elements Across Different Environments
==============================================================

.. toctree::
   :hidden:
   
   exporting_environment-dependent_and_independent_elements_to_different_files.rst
   export_to_a_file_with_properties.rst

In some organizations, applications are deployed in several environments
(e.g. development, staging and production). If you also have a Virtual
DataPort server for every environment, you probably need to have the
same elements (data sources, views, etc.) in all of them. However, the
configuration of the elements that depend on the environment where the
Server is running may be different. For example, in each Server, JDBC
data sources may have to point to different databases, Web service data
sources to different URLs, etc.

By default, the metadata is stored in a single file or in a Repository.
If you want to import these metadata into a Server of a different
environment, you have to edit these files to change the parameters’
values of some VQL statements. This process can be prone to errors.

Virtual DataPort provides two options to help you export and import
metadata between Servers that run in different environments:

#. Export the elements that depend on the environment to one file and
   the elements that do not depend on the environment, to another. See
   section :ref:`Exporting Environment-Dependent and Independent Elements to
   Different Files`.

   .. note:: This option is deprecated and it may be removed in future
      releases of the Denodo Platform.

#. Or, export the metadata to a file with properties. See section
   :ref:`Export to a File with Properties`.

You can use these options when exporting/importing from the
Administration Tool and when using the ``export`` and ``import`` scripts
(section :doc:`Export Script </vdp/administration/cluster_architectures_server_backup/using_the_import_export_scripts_for_backup_and_or_replication/export_script>`).

