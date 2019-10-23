===============================
Installing Updates and Hotfixes
===============================

This section explains how to install Denodo updates and hotfixes over an installation of the Denodo Platform. These instructions also apply for updates and hotfixes of the Solution Manager.

The updates or hotfixes are released in a zip file, which include the
following:

-  A file (RELEASENOTES) that describes the bug fixes and enhancements included.
-  A jar file that contains the update.
-  Beta updates include a temporary license that allow you install the
   beta update on a separate environment without interfering with your
   current installations of Denodo.

There are two ways of installing an update:

1. :ref:`Graphically, from the Control Center <Installing an Update or Hotfix Graphically>`.
2. :ref:`From the command line <Installing an Update or Hotfix from the Command Line>`.

The Denodo servers keep backward compatibility regarding its administration tools and its drivers, within the same major version. That is, you can install a newer update in the server without having to install it on the administration tool or JDBC/ODBC clients.

.. important:: After installing an update or a hotfix for the Solution Manager, start the License Manager 
   **before** the Solution Manager server. Otherwise, the Solution Manager server may fail.

Installing an Update or Hotfix Graphically
====================================================

Follow these steps to install an update or a hotfix from the Control Center:

1. Decompress the zip file of the update/hotfix.

#. Read the RELEASE NOTES file included in the update/hotfix. Pay special attention to
   the *Post-installation actions* section.

#. Stop all the Denodo servers and tools and all the instances of Microsoft Internet Explorer, in the host where you are going to install the update or hotfix.

#. On production servers, we recommend making a copy of the entire folder of the Denodo Platform. It is not possible to rollback a Denodo update, so this copy will allow you to restore quickly the Platform to its previous state, if necessary.

   Before copying the folder, delete the content of these folders to save space:
   
   -  :file:`{<DENODO_HOME>}/work/vdp/swap`
   -  :file:`{<DENODO_HOME>}/work/vdp/pipe`
   
   These folders contain information that is not necessary once the Denodo server is stopped.

#. On production servers, if any of the components is configured to store its metadata and settings on an external database, 
   we recommend making a backup of the catalog of the database where the metadata is stored.

   The components that can be configured to store its metadata and settings on an external database are the Solution Manager, 
   Data Catalog and Scheduler.
   
   This recommendation is not about the cache database of Virtual DataPort.
   
#. Open the Control Center.
#. Click **Update**. This dialog
   lists the updates that have been installed (the last update of the
   list is the current one).
#. Click **Install update** and **select the jar file** (not the zip file) of the update. The Control
   Center will display a dialog with a progress bar.

Installing an Update or Hotfix from the Command Line
====================================================

You can install an update or a hotfix from the command line, without displaying a GUI. This is useful if you want to install an update or a hotfix using a script or in a host without graphical support.

To do this, follow these steps:

1. Decompress the zip file of the update or hotfix.

#. Read the RELEASE NOTES file included in the update/hotfix. Pay special attention to
   the *Post-installation actions* section.

#. Stop all the Denodo servers and tools and all the instances of Microsoft Internet Explorer, in the host where you are going to install the update or hotfix.

#. On production servers, we recommend making a copy of the entire folder of the Denodo Platform. It is not possible to rollback a Denodo update, so this copy will allow you to restore quickly the Platform to its previous state, if necessary.
         
   Before copying the folder, delete the content of these folders to save space:
   
   -  :file:`{<DENODO_HOME>}/work/vdp/swap`
   -  :file:`{<DENODO_HOME>}/work/vdp/pipe`
   
   These folders contain information that is not necessary once the Denodo server is stopped.

#. On production servers, if any of the components is configured to store its metadata and settings on an external database, 
   we recommend making a backup of the catalog of the database where the metadata is stored.

   The components that can be configured to store its metadata and settings on an external database are the Solution Manager, 
   Data Catalog and Scheduler.
   
   This recommendation is not about the cache database of Virtual DataPort.
   
#. Open a command line. On Windows, launch the command line with the option
   “Run as administrator” even if you are logged in as an administrator.
#. Execute the following command:

  .. code-block:: batch

     cd <DENODO_HOME>
     cd jre
     cd bin
     java -jar <path to the jar file of the update>/denodo-v70-update-<yyyyMMdd>.jar <DENODO_HOME> -c

In the commands above, replace ``<DENODO_HOME>`` with the path to the installation of Denodo.
	 
This installs the update in the installation of ``<DENODO_HOME>``.

If the installer detects that any Denodo server or tool is running,
it will ask for configuration to continue. The installer does this to
make sure that the update or the hotfix can be installed correctly. Take
this into account if you are using a script to install the same update
or hotfix on several hosts.

This command returns one of these exit codes:

-  0: the update/hotfix is installed successfully.
-  1: one or more commands executed during the installation of the update/hotfix fails.
-  -1: when any other kind of error occurs.
