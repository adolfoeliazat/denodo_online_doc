================================
Outline of the Migration Process
================================

This document lists the steps of a successful migration to Denodo 7.0.

.. note:: If you are migrating from Denodo 5.5 or earlier, you cannot migrate to Denodo 7.0 directly. See below 
   :ref:`Migrating from Denodo 5.5 or Earlier`.

The upgrade process has four main steps:

#. Install Denodo Platform 7.0 on a new location.
   
   We will refer to the platform that has just been installed as *new version*, installed at :file:`{<NEW_DENODO_HOME>}`.

#. Export all metadata from your current Denodo Platform installation.

   We will refer to the previously installed version 
   of the Denodo Platform as the *current version*, installed at 
   :file:`{<OLD_DENODO_HOME>}`.

#. Import the metadata from the "old" version to the new one.

#. Configure the new Denodo Platform.

.. note:: We strongly
   recommend migrating and testing the non-production environments before
   the production environments.

.. note:: If you are going to upgrade from Denodo 5.0 or earlier,
   contact the `Denodo Support Team <https://support.denodo.com>`_.

Migrating from Denodo 5.5 or Earlier
====================================

To migrate the metadata of Virtual DataPort version 5.5 or earlier into version 7.0, you cannot import the VQL file of version 5.5 into 7.0, directly. You have to "go through" Virtual DataPort 6.0. This is because, among other things, several VQL statements of Denodo 5.5 are not supported by Denodo 7.0. Denodo 6.0 does support them and the VQL it generates is compatible with Denodo 7.0.

In this scenario, follow these steps:

1. Install a Denodo 6.0 server and its latest update.
#. Follow the steps of the following sections of the Migration Guide **version 6.0**, to export the metadata from the Virtual DataPort server version 5.5 or earlier:

   i. `Export the Metadata of the Current Installation <https://community.denodo.com/docs/html/browse/6.0/platform/migration/export_the_metadata_of_the_current_installation>`_ 
   #. `Modify the Virtual DataPort Metadata <https://community.denodo.com/docs/html/browse/6.0/platform/migration/modify_the_virtual_dataport_metadata>`_
   #. `Import the Metadata to the New Installation <https://community.denodo.com/docs/html/browse/6.0/platform/migration/import_the_metadata_to_the_new_installation>`_
#. Install a Denodo 7.0 server and its latest update.
#. Follow the steps of this Migration Guide to export the metadata from Virtual DataPort 6.0 into Virtual DataPort 7.0.
