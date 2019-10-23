===========================
Configuring the VCS Clients
===========================

Virtual DataPort can use a Version Control System (VCS) to store the
metadata of the Virtual DataPort server (data sources, views, etc.).
This allows users to do the main tasks involved in version control from
the Administration Tool: check out / update and check in / commit of
databases and their elements.

The supported Version Control Systems are:

-  GIT: there are no post-installation tasks to use GIT in Denodo.
-  Microsoft Team Foundation Server (TFS). The section :ref:`Configure the
   Denodo Platform to Work with Microsoft TFS` describes how to
   configure the Denodo Platform to work with TFS.
-  Subversion. The sections :ref:`Configure a Subversion Client to Use the
   VCS Integration` and :ref:`Configure the Denodo Platform to Work with
   Subversion` describe how to configure your Subversion client to work
   with the Denodo Platform.

To learn how to use the VCS support of Virtual DataPort, read the
section :doc:`Version Control Systems Integration <../../../../vdp/administration/version_control_systems_integration/version_control_systems_integration>`
of the Virtual DataPort Administration Guide.

Configure a Subversion Client to Use the VCS Integration
==============================================================

To use Subversion to store the metadata of Virtual DataPort, you need to set up a Subversion server (the supported version is 1.7). In addition, in the host where the Virtual DataPort server will run, follow these steps:

1. Install a Subversion client. The version of the client has to be 1.7.
   The recommended client is `Apache Subversion <http://subversion.apache.org/>`_ 1.7.x.

2. Add to the ``PATH`` environment variable the directory where the ``svn``
   executable is located.
   For Apache Subversion, that is the ``bin`` directory.
   
3. If the Virtual DataPort server will run on Windows, download the "Microsoft Visual C++ 2010 Redistributable 
   Package". To do this, follow these steps:
   
   i. Open the page to download the `Microsoft Visual C++ 2010 Redistributable 
      Package <https://www.microsoft.com/en-us/download/details.aspx?id=26999>`_.
      
   #. Select one of the following platform-specific files depending on your
      scenario:
   
      a. Virtual DataPort server running on a 32-bit O.S: vcredist\_x86.exe
      #. Virtual DataPort server running on a 32-bit JVM, on a 64-bit O.S:
         vcredist\_x86.exe
      #. Virtual DataPort server running on a 64-bit JVM: vcredist\_x64.exe
      #. Itanium system: vcredist\_IA64.exe
      
4. Make sure that the “global ignores” list of the Subversion client does
   *not* include any of the following patterns:

   -  ``d*.a, *.o, (in general, *.<character>)``
   -  ``d*.vql``
   -  ``*.properties``
   -  ``*.dependencies``
   
   With Apache Subversion, you have to change the value of the property 
   global-ignores in this configuration file:
   
   -  On Windows: ``%APPDATA%\Subversion\config``
   -  On Linux: ``~/.subversion/config`` or ``/etc/subversion/config``

The default configuration of Subversion clients includes several file
and directory name patterns that are ignored by Subversion operations.
For example, by default Apache Subversion 1.7.5, ignores the files that
match any of the following patterns:

.. code-block:: none

   global-ignores = \*.o \*.lo \*.la \*.al .libs \*.so \*.so.[0-9]\* \*.a
   \*.pyc \*.pyo \*.rej \*~ #\*# .#\* .\*.swp .DS\_Store

Note that ``*.o`` and ``*.a`` are included in the list. This is
problematic because of the way Virtual DataPort maps folders to their
physical location in the file system when exporting to repository or
performing VCS operations.

For example, a folder named ``a`` will be physically located at
``<some>/<path>/folder.a``. As all ``*.a`` files and directories are
ignored by Subversion by default, VCS operations involving such Virtual
DataPort folders will fail.

Make sure that the *global-ignores* list does not include any of the
mentioned patterns, as they correspond to the types of files involved in
VCS operations.

.. note:: Some Subversion servers such as “CollabNet Subversion Edge”
   cannot handle files whose name contains characters reserved by the file
   system like ``\``, ``/``, ``:``, ``*``, ``?``, etc. Therefore, we
   strongly recommend not using any of these characters in the name of the
   database or any of its elements if this database will be stored in a
   Subversion server.

Configure the Denodo Platform to Work with Subversion
==============================================================

Ignore this section if you installed the Denodo Platform on Windows.

If you installed the Denodo Platform on Linux and are going to use
Subversion to store the metadata of Virtual DataPort, follow these
steps:

#. Launch a Virtual DataPort Administration Tool and log in as an
   administrator user (e.g. the default ``admin`` user).
#. Perform the steps described in the section :doc:`Enabling Uniqueness
   Detection <../../../../vdp/administration/version_control_systems_integration/uniqueness_detection/enabling_uniqueness_detection>` of the Administration Guide.
#. If you are going to connect to Subversion using the http or https
   protocol, follow the steps described in the section :ref:`Activating the LS Optimization`.


Configure the Denodo Platform to Work with Microsoft TFS
==============================================================

To use Microsoft Team Foundation Server (TFS) to store the metadata of
Virtual DataPort, you have to set up a Microsoft Team Foundation Server
(TFS). The supported versions are 2010 or higher.

The Denodo Platform includes the necessary libraries to connect to a TFS
server.

The TFS administrator is in charge of creating and managing the
collections that will contain team projects with Virtual DataPort
metadata and their different development branches, if any. We recommend
having at least one branch in each project with Virtual DataPort
metadata (as described by this `MSDN article <https://docs.microsoft.com/en-us/vsts/tfvc/branch-folders-files>`_).

The recommended repository structure will be like this (the nodes in
italics are managed by the TFS administrators, the others are managed by
Virtual DataPort):

-  *TFS Servers*

   -  *DefaultCollection*

   -  *Collection 1*

   -  *Collection 2*

   -  …

   -  *VDP Collection 1*

      - *Team Project 1*


      - *Team Project 2*

      -  ...

      -  *Team Project n*

         -  *Branch-1*
         -  *Branch-2*
         -  ...
         -  *Branch-n*

         -  *Main*
         
            -  *databases*
            
               -  my database 1
               
               -  my database 2

               -  …
               
               -  my database n

                  -  …


            -  extensions
            -  environments
            -  maps


Each TFS collection is backed up by a different database, so it must be
managed separately from the others.

Each TFS project can contain several Virtual DataPort databases for each
development branch, which will share environments and global elements,
so we recommended that each TFS project contain Virtual DataPort
metadata related to only one application.
