=======================================================================================
Enabling Internet Explorer Sequence Generation Toolbar in Microsoft Windows Server 2008
=======================================================================================

To show the Internet Explorer toolbars in a Microsoft Windows Server
2008 operating system, you have to enable third-party browser
extensions.

To do this, follow the steps below.

It is *very important* that you perform these steps using the same
account that you will use to launch the Denodo Control Center *and* the
Denodo Windows services.

#. Close all instances of Internet Explorer, click **Start**, open
   **Settings**, and then click **Control Panel**.
#. Double-click **Internet Options**.
#. Click the **Advanced** tab.
#. Under the category **Browsing**, select the **Enable third-party
   browser extensions (requires restart)**.
#. Restart Internet Explorer.

The previous steps are equivalent to set value “Enable Browser
Extensions” = “yes” in the registry key
“HKCU\\Software\\Microsoft\\Internet Explorer\\Main”.
