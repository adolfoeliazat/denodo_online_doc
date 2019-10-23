==========================
Configure Windows Services
==========================

The installer of the Denodo Platform for Windows creates Windows
services for some of its components: Virtual DataPort server, Scheduler server, etc. 

By default, new Windows services are configured to:

-  Start manually.
-  Run under the predefined “local system user account”.

We recommend configuring the Windows services of Denodo you are going to use to:

1. Setting the *Startup type* to *Automatic*. That way, if the host is restarted, Windows will start the Denodo services automatically.

#. Changing the account under which the Denodo services run, to be the same
   account used to launch the Denodo Control Center.
   
   This user account
   needs full privileges over the folder and subfolders where the Denodo Platform is
   installed. The reason is that if first, you launch a Denodo server using the
   Control Center and later, you launch it as a Windows service, the Denodo server
   may not be able to modify the files created by the Denodo server when it was launched from the Control Center.

To configure a Windows service this way, follow these steps:

#. Press the keys **Windows logo+R** to open the “Run” dialog.
#. Enter ``services.msc`` to open the Window services configuration
   tool.
#. Search for a Denodo service, right-click on it (all of them start with word "Denodo")
   and click **Properties**.
#. In the tab **General**, in the box **Startup type**, select **Automatic**.
#. Click the **Log on** tab.
#. Select **This account**, enter the user name of the user that will be
   used to run the Denodo Control Center and enter its password.
#. Repeat steps 3 to 6 to configure the other Windows services of Denodo.

.. important:: If Denodo is installed on Windows 10, Windows Server 2016 or newer, **do not** configure the Browser Pool component as a Windows service. That is because in these versions, Microsoft Internet Explorer does not use the configuration of the user starting the Windows service so it will start with an incorrect configuration.

   To automate the launch of the Browser Pool, read the article `Automating status check and startup of Denodo processes <https://community.denodo.com/kb/view/document/Automating%20status%20check%20and%20startup%20of%20Denodo%20processes>`_ of the Knowledge Base. It contains a script to automate this task.

Installing the Denodo Web Applications as Windows Services
==========================================================

By default, the Denodo web applications have to be started manually (Data Catalog, Scheduler administration tool, etc.). If the Denodo servers run on Windows, consider creating a Windows service for each application. To do this, do the following:

#. Start a command prompt as an administrator and execute the following:

.. code-block:: batch

   cd <DENODO_HOME>
   cd bin
   
   REM To install the "ITPilot administration tool" as a Windows service
   itpilot_webadmintool_service.bat install
   
   REM To install the "Scheduler administration tool" as a Windows service
   scheduler_webadmintool_service.bat install
   
   REM To install the "Diagnostic & Monitoring tool" as a Windows service
   diagnosticmonitoringtool_service.bat install
   
   REM To install the "Data Catalog" as a Windows service
   datacatalog_service.bat install

2. Press the keys **Windows logo+R** to open the “Run” dialog.
#. Enter ``services.msc`` to open the Window services configuration
   tool.
#. Configure each new Windows service following the steps of the section above.
