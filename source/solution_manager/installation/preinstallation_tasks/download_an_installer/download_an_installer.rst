.. todo: for 8.0, rename the file to match the title of the section

======================
Download the Installer
======================

Download the Solution Manager installer from the `Denodo Support Site <https://support.denodo.com>`_.

Denodo provides several flavors of the installer and the only difference
between them is the Java Runtime Environment (JRE) they include. We
recommend downloading the installer that includes the 64-bit JRE because
it allows assigning more memory to the Solution Manager servers than the 32-bit
JRE.

-  ``denodo-install-solutionmanager-7.0-win64.zip``: it includes a 64-bit JRE for
   Windows.
-  ``denodo-install-solutionmanager-7.0-linux64.zip``: it includes a 64-bit JRE for
   Linux.
-  ``denodo-install-solutionmanager-7.0-linux32.zip``: it includes a 32-bit JRE for
   Linux.
-  ``denodo-install-solutionmanager-7.0-win32.zip``: it includes a 32-bit JRE for
   Windows.
-  ``denodo-install-solutionmanager-7.0.zip``: it does not include any JRE. Only use
   this installer if the previous installers are unsuited for your
   environment. For example, if you are installing on Solaris. Before
   launching this installer, set the environment variable ``JAVA_HOME``
   to point to a JRE version 8. Otherwise, the installer process
   will fail.
