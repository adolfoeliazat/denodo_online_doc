===============================================================
Unattended Installation of the Solution Manager
===============================================================

The following sections explain how to perform an unattended installation
of the Solution Manager and its updates.

.. contents::
   :depth: 1
   :local:
   :backlinks: none

Modifying the Solution Manager Installer to Include the Latest Update
=====================================================================

If by the time you are going to install the Solution Manager, there is an
update available for this version (you can check this in the `Denodo Support Site <https://support.denodo.com>`_),
you can easily modify the installer so it also installs
the update automatically.

This is useful if you are going to perform an installation on many computers. For example,
to generate an installer for all the developers of your organization.

To do this, follow these steps:

#. Download a Solution Manager installer from the *Denodo Support Site* and decompress it.
   See the section :doc:`../preinstallation_tasks/download_an_installer/download_an_installer` to know which installer you
   need.
#. Download the latest update *of the Solution Manager* (not of the Denodo Platform) and decompress it.
#. Inside the folder of the installer, create a folder named
   ``denodo-update``. I.e. ``denodo-install-solutionmanager-7.0\denodo-update``
#. Rename the jar file inside the zip file of the update to
   ``denodo-update.jar``.
#. Copy ``denodo-update.jar`` to the ``denodo-update`` folder. Thus
   having this path:
   ``denodo-install-solutionmanager-7.0\denodo-update\denodo-update.jar``
#. Compress the directory ``denodo-install-solutionmanager-7.0`` again.

When using this modified installer, the installer will automatically
install the update after the installation process finishes.

Unattended Installation of the Solution Manager
===============================================

You can automate the installation of the Solution Manager by generating a
response file instead of using the graphical wizard or the command line
one. The benefit of doing this is that you can complete several
installations on multiple hosts without user intervention.

This process has two main steps:

-  Generating a response file. The process is similar to installing the
   Solution Manager from the command line, but the result is a response
   file instead of an actual installation.
-  Using this response file, execute the unattended installation.

To perform an unattended installation, follow these steps:

#. Download a Solution Manager installer from the Support Site. The section
   :doc:`../preinstallation_tasks/download_an_installer/download_an_installer` explains which one you should select.

   If by the time you are doing this, there is an update available, you may
   be interested in modifying the installer to automatically install the
   update as well. The section :ref:`Modifying the Solution Manager Installer to
   Include the Latest Update` explains how to do this.

#. Decompress the downloaded file.

#. Open a command line and execute the following commands. On Windows,
   launch the command line with the option “Run as administrator” even if
   you are logged in as an administrator.


   a. **Windows**:

      .. code-block:: batch

         cd denodo-install-solutionmanager-7.0
         installer_cli.bat generate response_file_7_0.xml

   b. **Linux**:

      .. code-block:: bash

         cd denodo-install-solutionmanager-7.0
         chmod +x installer_cli.sh
         installer_cli.sh generate response_file_7_0.xml



   After following the steps of the wizard, the file
   ``response_file_7_0.xml`` will contain the necessary information to
   perform the installation.

#. On each host where you want to install the Solution Manager, decompress
   the zip of the installer and execute the following commands. This will
   start the unattended installation:

   a. **Windows**:

      .. code-block:: batch

         cd denodo-install-solutionmanager-7.0
         installer_cli.bat install --autoinstaller response_file_7_0.xml

   b. **Linux**:

      .. code-block:: bash

         cd denodo-install-solutionmanager-7.0
         chmod +x installer_cli.sh
         installer_cli.sh install --autoinstaller response_file_7_0.xml


You can use a response file generated with the Linux installer to
perform installations on Windows and vice versa. You will only have to
modify the response file to set a path that is valid on the operating
system. To change this path, edit the response file and change the value
of the property “INSTALL\_PATH”.

Note that you still have to perform the post-installation tasks
described in the section :doc:`../postinstallation_tasks/postinstallation_tasks` on each
installation. Most of them can be scripted as well to make the process
faster.

