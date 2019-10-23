:orphan:

.. rubric:: Configuring Internet Explorer Running Under the Local System Account

.. important:: If Denodo is installed on Windows 10, Windows Server 2016 or newer, **do not** configure the Browser Pool component as a Windows service. That is because in these versions, Microsoft Internet Explorer does not use the configuration of the user starting the Windows service so it will start with an incorrect configuration.

   If you do not start the Browser Pool as a Windows service, you do not have to do the steps of this page.

   To automate the launch of the Browser Pool, read the article `Automating status check and startup of Denodo processes <https://community.denodo.com/kb/view/document/Automating%20status%20check%20and%20startup%20of%20Denodo%20processes>`_ of the Knowledge Base. It contains a script to automate this task.

We recommend configuring all the Denodo Windows services under a
specific user account instead of the *local system account*. The
section :doc:`Configure Windows Services<../../postinstallation_tasks/configure_windows_services/configure_windows_services>` explains how to do this.

However, if you cannot do this and you have to change the default
settings of Microsoft Internet Explorer when it runs under the *local
system account*, follow these steps:

#. Download the `Microsoft utility suite PsTools <https://docs.microsoft.com/en-us/sysinternals/downloads/pstools>`_ and
   unzip it.
#. Start the “Interactive Services Detection” Windows service.
#. Use the PsExec utility, included in the PsTools suite, to open a
   Microsoft Internet Explorer instance on the local system account. To
   do that, execute the following from a command line (adapting the path
   of the Microsoft Internet Explorer executable to that of your
   system):
   
   .. code-block:: batch
   
      PsExec.exe -s -i 0 "C:\Program Files\Internet Explorer\iexplore.exe"
   
#. Perform the necessary configuration changes.
#. After closing Microsoft Internet Explorer, the system will be ready
   to use the ITPilot Browser Pool as a Windows service on the *local
   system* account.
