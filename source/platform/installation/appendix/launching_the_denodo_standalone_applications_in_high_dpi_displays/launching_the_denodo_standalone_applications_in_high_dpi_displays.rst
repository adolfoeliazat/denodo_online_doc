=================================================================
Launching the Denodo Standalone Applications in High DPI Displays
=================================================================

When using a monitor with a high DPI display, Windows will display the Denodo Control Center,
the Virtual DataPort administration tool and the ITPilot Wrapper Generation
Tool with a very tiny font. To avoid this problem, follow these steps:

1. In the host where you are having this problem, open the file explorer and go to the folder <DENODO_HOME>/jre/bin.

2. In this folder, right-click "javaw.exe" and click **Properties**.

3. Click the tab **Compatibility**.

4. Select **Override high DPI scaling behavior** and in **Scaling performed by**, select **System**.

5. Repeat steps 2 to 4, for the file "java.exe".

This should fix the problem.

|

If the previous steps did not work, try the following:

-  Modify the Windows Registry to tell Windows to look for an external
   manifest file.
-  Create external manifest files.

You only have to do this when running these tools on Windows.

To do this, follow these steps:

#. Press Windows logo+ R, enter ``regedit`` and click **OK**.

#. Navigate to the following registry key:

   .. code-block:: none

      HKEY_LOCAL_MACHINE > SOFTWARE > Microsoft > Windows > CurrentVersion
      > SideBySide

#. After selecting the entry ``SideBySide`` on the tree of keys,
   right-click on the right side of the window and click **New** >
   **DWORD (32 bit) Value**.

#. Type ``PreferExternalManifest`` and press Enter.

#. Right-click ``PreferExternalManifest`` and click **Modify**.

#. In the **Value Data** box, enter ``1``, select **Decimal** and click
   **Ok**.

#. In the rare case that you are launching the Virtual DataPort
   administration tool or the ITPilot Wrapper Generation Tool with a
   32-bit Java Virtual Machine (JVM) on a 64-bit Windows, continue to
   the next step.

   Otherwise, go to step 12. If you are not sure if you are in this scenario,
   go to step 12 because using a 32-bit JVM on a 64-bit Windows is a rare
   configuration.

#. Navigate to the following registry key:

   .. code-block:: none

      HKEY_LOCAL_MACHINE > SOFTWARE > WOW6432Node > Microsoft > Windows
      > CurrentVersion

#. Right-click ``CurrentVersion`` and click **New** > **Key**. The name
   of the new key is ``SideBySide``.

#. After selecting the new entry ``SideBySide`` on the tree of keys, on
   the right side of the window right-click and select **New** > **DWORD
   (32 bit) Value**.

#. In the **Value Data** box, enter ``1``, select **Decimal** and click
   **Ok**.

#. Close the Registry Editor.

#. Download the `Windows tool “Sigcheck” <https://docs.microsoft.com/en-us/sysinternals/downloads/sigcheck>`_
   and unzip it in the directory ``<DENODO_HOME>\jre\bin``.

#. Open a command line and execute the following:

   .. code-block:: bash

      cd <DENODO_HOME>\jre\bin
      sigcheck -m java.exe

#. The output of this file looks like:

   .. code-block:: xml
      :name: launching_the_denodo_standalone_applications_in_high_dpi_displays-internal_link:


            Verified:       Signed
            Signing date:   11:37 AM 9/14/2017
            Publisher:      Oracle America, Inc.
            Company:        Oracle Corporation
            Description:    Java(TM) Platform SE binary
            Product:        Java(TM) Platform SE 8
            Prod version:   8.0.1520.16
            File version:   8.0.1520.16
            MachineType:    64-bit
            Manifest:

      <assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0" xmlns:asmv3="urn:schemas-microsoft-com:asm.v3">
      ...
      ...
      ...

..

      Copy the output starting from ``<assembly xmlns="urn:schemas-microsoft-com:asm.v1"``
      (right below ``Manifest:``) until the end of the output.

      Paste this in a new file and save it as ``<DENODO_HOME>\jre\bin\java.exe.manifest``.
      This file has to start with ``<assembly xmlns="urn:schemas-microsoft-com:asm.v1"``.

16.Edit this file and replace

   ::
   
      <dpiAware>true</dpiAware>

   with

   ::
      
      <dpiAware>false</dpiAware>

17. Save this file and copy it to the same directory with the name
    ``javaw.exe.manifest``. I.e. in the
    directory ``<DENODO_HOME>\jre\bin`` you should have the
    files ``java.exe.manifest`` and ``javaw.exe.manifest``; both with
    the same content.

After this, the next time you start the Denodo Control Center, Virtual DataPort administration
tool or the ITPilot Wrapper Generator Tool they will be scaled properly.
