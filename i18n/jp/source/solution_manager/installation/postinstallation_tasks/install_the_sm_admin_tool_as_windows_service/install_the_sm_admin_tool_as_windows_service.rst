====================================================================
Installing the Solution Manager Web Applications as Windows Services
====================================================================

By default, Solution Manager administration tool has to be started manually. If the Solution Manager runs on Windows, consider to install it as a Windows service. To do this, do the following:

#. Start a command prompt as an administrator and execute the following:

.. code-block:: batch

   cd <SOLUTION_MANAGER_HOME>
   cd bin

   REM To install the "Solution Manager Administration tool" as a Windows service
   solutionmanagerwebtool_service.bat install

   REM To install the "Diagnostic & Monitoring tool" as a Windows service
   diagnosticmonitoringtool_service.bat install

2. Execute ``services.msc``. This will launch the Windows services
   wizard.
#. Look for one of the new Solution Manager services (all of them start with word
   “Denodo”), right-click it and click **Properties**.
#. Click the **Log on** tab.
#. Select **This account**, enter the user name of the user with which
   you run the Solution Manager servers.
#. Repeat steps 3 to 5 to configure the other new Solution Manager services.

To remove the Windows service of one of these applications, execute the
same script but with the parameter ``remove`` instead of ``install.``
