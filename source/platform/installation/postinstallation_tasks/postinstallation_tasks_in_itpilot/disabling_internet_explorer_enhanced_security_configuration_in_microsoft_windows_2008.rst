=====================================================================================
Disabling Internet Explorer Enhanced Security Configuration in Microsoft Windows 2008
=====================================================================================

In Microsoft Windows Server 2008, the Internet Explorer Enhanced
Security Configuration feature interferes with the correct functioning
of the Sequence Generation Toolbar, and has to be disabled.

Follow these steps to disable this feature:

#. Log on to the computer with a user account that is a member of the
   local Administrators group.
#. Click **Start**, go to **Administrative Tools**, and then click
   **Server Manager**.
#. If the “User Account Control” dialog box appears, click **Continue**.
#. Under **Security Summary**, click Configure **IE ESC**.
#. Set **IE ESC** to **Off** for the appropriate user type
   (**Administrators**, **Users**).
#. Click **OK**.
#. Restart Internet Explorer.

Do this in all the hosts where a developer is going to use the “Sequence
Generation Toolbar” to develop ITPilot wrappers.
