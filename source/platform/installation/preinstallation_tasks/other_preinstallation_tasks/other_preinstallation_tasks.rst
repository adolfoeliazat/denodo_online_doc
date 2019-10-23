============================
Other Preinstallation Tasks
============================


The following subsections describes tasks that you must complete before you install
the Denodo Platform:

.. contents::
   :depth: 1
   :local:
   :backlinks: none

Check that the Required Ports Are Free
======================================

The servers of the Denodo Platform listen for incoming connections on several ports. The appendix 
:ref:`Default Ports Used by the Denodo Platform Modules`
lists the default ports. 

Make sure these ports are available on your system. If they are not available, select others that are free during the installation process.

Check the PATH Environment Variable on Windows
==============================================

Ignore this subsection if you are installing the Denodo Platform on
Linux.

If you are installing the Denodo Platform on Windows, check that the
PATH environment variable meets these rules:

#. PATH cannot have double quotes. If it does, the modules of the Denodo
   Platform will not start. E.g. the following PATH is invalid:

   .. code-block:: batch
   
      PATH=path1;"C:\Program Files\Software"


   If PATH has double quotes, remove them.


#. PATH cannot end with a backslash (``\``). If it does, the modules of
   the Denodo Platform will not start as a Windows service.

   If it does, remove the backslash or add a semicolon at the end of the 
   value of the variable.

Select a User Account to Install the Denodo Platform
====================================================

In the host where you are installing the Denodo Platform servers, create
a user account to install and run the Denodo Platform servers.

.. important:: Always install and run the Denodo Platform servers with
   the same user account. The reason for this is that the Denodo servers
   modify files in the directory where they are installed and the user
   account needs read and write privileges. If you perform the installation
   with one user account and run the servers with another, the second user
   account may not be able to modify the files in the installation.

   You can also use an existing account instead of creating one; but you
   must use the same for both installing and running the Denodo Platform
   servers.
   
Close All the Browsers
======================

If you are installing any of the components of ITPilot, close all the browsers of the host. Some ITPilot modules need the browser to be closed in order to be installed correctly.
