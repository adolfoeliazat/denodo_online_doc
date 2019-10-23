================================
Using the Command Line Installer
================================

The command-line wizard is equivalent to the graphical one but it runs
on the command line and does not require support for a graphical
interface.

The steps are the same as in the graphical installer. For example:

-  There are two installation modes:

   1. “express” (also known as “Default installation”): recommended for most users. You can select the modules you
      want to install and they will be installed with their default
      configuration values.
   #. “custom”: recommended for advanced users. You will be able to set
      the values of several configuration parameters such as the ports
      where the Solution Manager servers will listen for incoming
      connections.

-  Select which modules you want to install.

.. important:: We recommend installing all the modules of the Solution Manager even if right now, you do not plan on using all of them. That is because the installer cannot add modules to an existing installation. If in the future, you want to use a module that is not installed, you would have to install and configure the Solution Manager again.


|

To start the command line installer, do the following:

#. Decompress the Solution Manager installer. The section :doc:`Download an
   Installer <../preinstallation_tasks/download_an_installer/download_an_installer>` explains which one you should download.

#. Execute the following:

   a. **On Windows**, open a command line *as an administrator* and
      execute this:

      .. code-block:: batch

         cd denodo-install-solutionmanager-7.0
         installer_cli.bat install


   b. **On Linux**:

      .. code-block:: bash

         unset DISPLAY
         cd denodo-install-solutionmanager-7.0
         chmod +x installer_cli.sh
         ./installer_cli.sh install

      The command UNSET DISPLAY removes the DISPLAY variable from the environment. This is necessary because the command-line installer will not start 
      if the variable DISPLAY is set and the host does not have graphical support.      


#. Follow the steps. The meaning of each parameter is explained in the
   section that describes how to use the graphical installer (section
   :doc:`Using the Graphical Installation Wizard <../using_the_graphical_installation_wizard/using_the_graphical_installation_wizard>`)

   There are options where at the end you will see something like
   “(default: 19097)”. This means that if you press Enter, that will be the
   selected value.

   .. important:: The installation path must not contain spaces (e.g. ``C:\Denodo\DenodoSolutionManager7.0``). Otherwise, the connection to some parallel processing databases will not work.

      The Denodo Platform writes data in the directory where is installed so the user that launches the Denodo servers needs to be able to write files there.
