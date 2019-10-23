============================================================================================
Disabling Internet Explorer Enhanced Security Configuration in Microsoft Windows Server 2012
============================================================================================

In Microsoft Windows Server 2012, the Internet Explorer Enhanced
Security Configuration feature interferes with the correct functioning
of the Sequence Generation Toolbar, and has to be disabled.

Follow these steps to disable this feature:

#. Log on to the computer with a user account that is a member of the
   local Administrators group.
#. Click **Start**, go to **Administrative Tools**, and then click
   **Server Manager**.
#. If the “User Account Control” dialog box appears, click **Continue**.
#. Click **Local Server**.
#. On the right side of the “Server Manager”, look for the “IE Enhanced
   Security Configuration Setting” (its default value is “On”). Set it
   to **Off** for the appropriate user type (**Administrators**,
   **Users**).
#. Click **OK**.
#. Restart Internet Explorer.

Do this in all the hosts where a developer is going to use the “Sequence
Generation Toolbar” to develop ITPilot wrappers.
