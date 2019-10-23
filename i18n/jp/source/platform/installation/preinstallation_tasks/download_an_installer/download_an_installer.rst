.. todo: for 8.0, rename the file to match the title of the section

======================
Download the Installer
======================

Download the Denodo Platform installer from the `Denodo Support Site`_.

Denodo provides several flavors of the installer and the only difference
between them is the Java Runtime Environment (JRE) they include.

-  ``denodo-install-7.0-win64.zip``: it includes a 64-bit JRE for
   Windows.
-  ``denodo-install-7.0-linux64.zip``: it includes a 64-bit JRE for
   Linux.
-  ``denodo-install-7.0.zip``: it does not include any JRE. Only use
   this installer if the previous installers are unsuited for your
   environment. For example, if you are installing on Solaris. Before
   launching this installer, set the environment variable ``JAVA_HOME``
   to point to a JRE version 8. Otherwise, the installer process
   will fail.

|

In addition to the regular installer, the Denodo Platform also provides
the “client installer”. It is aimed at developers because it only includes the Virtual
DataPort administration tool and the Denodo JDBC and ODBC drivers. Its name is
**denodo-install-vdp-client-7.0**. You can also download it from the
`Denodo Support Site`_.

The installation process is the same as with the regular installer with
the difference that it can only install the components mentioned
earlier.

|

For older systems, you also have available installers with a 32-bit Java Runtime Environment. However, we advise against using them. The reason is that the 64-bit JRE allows assigning more memory to the Denodo servers than the 32-bit JRE.

-  ``denodo-install-7.0-linux32.zip``: it includes a 32-bit JRE for Linux.
-  ``denodo-install-7.0-win32.zip``: it includes a 32-bit JRE for Windows.


.. _Denodo Support Site: https://support.denodo.com
