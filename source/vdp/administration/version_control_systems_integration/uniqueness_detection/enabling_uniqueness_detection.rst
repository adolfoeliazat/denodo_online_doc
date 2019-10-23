=============================
Enabling Uniqueness Detection
=============================

Ignore this section if you are using Git. It is only relevant for Subversion.

You only need to enable this feature when all these conditions are met:

-  The Virtual DataPort server runs on Linux
-  You are going to use Subversion

Otherwise, the uniqueness detection is already enabled. I.e. it is
enabled by default when Virtual DataPort runs on Windows or the VCS
system is TFS.

The uniqueness detection is already enabled by default when Virtual
DataPort runs on Windows, or the VCS system is TFS.

To activate uniqueness detection, follow these instructions:


#. Open the “VQL Shell” (Tools menu)


#. Execute this command:


.. code-block:: sql

   SET 'com.denodo.vdb.vdbinterface.server.vcs.VCSConfigurationManager.uniquenessConflictsDetection.svn' 
       = '3'

|
   The value of the property
   ``com.denodo.vdb.vdbinterface.server.vcs.VCSConfigurationManager.uniquenessConflictsDetection.svn``
   can be one of the following:
   
   -  ``0``: Uniqueness detection is disabled.
   -  ``1``: Uniqueness detection is enabled if Virtual DataPort runs on
      Windows.
   -  ``2``: Uniqueness detection is enabled if Virtual DataPort runs on
      Linux.
   -  ``3``: Uniqueness detection is enabled regardless of the operating
      system.


3. If you are going to connect to Subversion using the HTTP or HTTPs
   protocol, enable the LS Optimization. The section :ref:`LS Optimization for
   Subversion` describes this optimization and how to enable it.


#. Restart the Virtual DataPort Server.


