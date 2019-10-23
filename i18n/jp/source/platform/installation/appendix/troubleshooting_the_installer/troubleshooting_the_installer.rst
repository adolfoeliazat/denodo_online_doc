=================================================
Troubleshooting the Denodo Platform Installer
=================================================

During the installation of the Denodo Platform errors can occur that prevent the installation to complete correctly. For example, the host runs out of disk space during the installation or the user that launches the installer does not have enough privileges to install a Windows service.

To troubleshoot these issues, start the installer in debug mode. To do this, follow these steps:

1. Make sure that the user account that you use to launch the installer has privileges to create files on the directory where you are installing the Denodo Platform. To test this, you can create a file there and then, delete it.

2. If you already run the installer, delete the directory where you tried to install the Denodo Platform.

3. Start the installer in debug mode:

   a. To run the **graphical installer in debug mode**, open a command prompt (in Windows, an "admin" command prompt) and execute this:

**For Windows**

.. code-block:: batch

   cd denodo-install-7.0
   .\jre\bin\java.exe -DDENODO_INSTALL_DEBUG=ENABLED -jar denodo-install-7.0.dat > output-install.log 2>&1

**For Linux**

.. code-block:: bash

   cd denodo-install-7.0
   ./jre/bin/java -DDENODO_INSTALL_DEBUG=ENABLED -jar denodo-install-7.0.dat > output-install.log 2>&1

..

      Once the installation finishes, the file output-install.log will contain logging information and more information about what went wrong. You can remove the part ``> output-install.log 2>&1`` from the command so the logging information is printed on the command line.

   b. To launch the **command line installer in debug mode**, run this:

**For Windows**

.. code-block:: bash

   cd denodo-install-7.0
   installer_cli.bat install --debug

**For Linux**

.. code-block:: bash

   cd denodo-install-7.0
   ./installer_cli.sh install --debug


