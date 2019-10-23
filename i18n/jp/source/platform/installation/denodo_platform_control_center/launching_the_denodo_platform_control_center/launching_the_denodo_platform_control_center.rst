============================================
Launching the Denodo Platform Control Center
============================================

To launch the Denodo Platform Control Center do the following:

-  Use the shortcuts created on the desktop during the installation
   and/or the application menus of your Operating System
-  Or, execute :file:`{<DENODO_HOME>}/bin/denodo_platform`

.. note:: In Windows Vista, Windows 7 and Windows Server 2008, we
   recommend disabling the User Account Control (UAC) before launching the
   Denodo Platform Control Center. UAC cannot be disabled in Windows 8 or
   in Windows Server 2012.

If UAC cannot be disabled, keep in mind the following:

-  Launch the Control Center (``<DENODO_HOME>\bin\denodo_platform.bat``)
   with the “Run as Administrator” option of the contextual menu of
   ``denodo_platform.bat``, even if you are already logged in as an
   administrator user. If you launch a “standalone” Internet Explorer
   (i.e. it is not launched from the ITPilot Wrapper Generation Tool),
   you also may need to explicitly use the “Run as Administrator” option
   to properly use the ITPilot Sequence Generator Toolbar.
-  If you are using the PDF, Excel or Word conversion features, you may
   have to disable the Internet Explorer protected mode (this can be
   done from the Security page of the Internet Options dialog of
   Internet Explorer).

.. I comment the note below because no one uses the 32-bit installer. 
   #25823 - Tray icons are not shown when running the platform servers using a 64-bit jre (still pending)

   .. note:: When you launch any Denodo server, an icon will be displayed
      in the notification area of your operating system. This icon will not be
      displayed when using a 64-bit Java Virtual Machine to launch the server.
