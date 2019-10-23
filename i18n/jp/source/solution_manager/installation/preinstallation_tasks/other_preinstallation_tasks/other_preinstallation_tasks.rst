============================
Other Preinstallation Tasks
============================


The following subsections describes tasks that you must complete before you install
the Solution Manager:

.. contents::
   :depth: 1
   :local:
   :backlinks: none


Check that the Required Ports are Free
======================================

The servers of the Solution Manager listen for incoming connections on several ports. The appendix 
:ref:`Default Ports Used by the Solution Manager Modules`
lists the default ports. 

Make sure these ports are available on your system. If they are not available, select others that are free during the installation process.

Check the PATH Environment Variable on Windows
==============================================

Ignore this subsection if you are installing the Solution Manager on
Linux.

If you are installing the Solution Manager on Windows, check that the
PATH environment variable meets these rules:

#. PATH cannot have double quotes. If it does, the modules of the Solution Manager will not start. E.g. the following PATH is invalid:

   .. code-block:: batch
   
      PATH=path1;"C:\Program Files\Software"


   If PATH has double quotes, remove them.


#. PATH cannot end with a backslash (``\``). If it does, the modules of
   the Solution Manager will not start as a Windows service.

   If it does, remove the backslash or add a semicolon at the end of the 
   value of the variable.

Select a User Account to Install the Solution Manager
=====================================================

In the host where you are installing the Solution Manager servers, create
a user account to install and run the Solution Manager servers.

.. important:: Always install and run the Solution Manager servers with
   the same user account. The reason for this is that the Solution Manager servers
   modify files in the directory where they are installed and the user
   account needs read and write privileges. If you perform the installation
   with one user account and run the servers with another, the second user
   account may not be able to modify the files in the installation.

   You can also use an existing account instead of creating one; but you
   must use the same for both installing and running the Solution Manager
   servers.
