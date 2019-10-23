=====================================
7.0 GA: New Features of the Installer
=====================================

This section lists the new features of the installers of the Denodo Platform |version| GA.

There are now three installers:

1. A new installer (denodo-install-solutionmanager-7.0). It includes the :ref:`Web Panel <Web Panel Guide>` and the :ref:`Solution Manager and License Manager <Solution Manager Administration Guide>`.

2. The installer of the Denodo Platform (denodo-install-7.0). This is the installer you are already familiar with, which includes Virtual DataPort, Scheduler and ITPilot.

3. The installer of the Virtual DataPort administration tool (denodo-install-vdp-client-7.0). This installer includes the Virtual DataPort administration tool and the JDBC and ODBC drivers. It is aimed at developers.

Other Changes
=============

.. Installer #31331 Add correct port on server configuration of scheduler

When during the installation you set a port for the Virtual DataPort server that is not the default one, the Scheduler server is now configured to connect to this port. In previous versions, the administrator has to modify that setting in Scheduler after the installation.

|

.. Installer #32457 Additional metainformation for Denodo Platform installations

It is now possible to uniquely identify an installation of the Denodo Platform using the file :file:`{<DENODO_HOME>}/install.xml`. This file now contains the installation date and a random identifier.

|

.. # Installer #35445 Use a default installation path in Windows without blank spaces

The default installation path is now a path outside "Program Files" and without spaces. The installation path must not contain spaces (e.g. ``C:\Denodo\Denodo_Platform_7_0``). Otherwise, the connection to some parallel processing databases will not work.

|

.. Installer #33792 Client installer: remove the dialog to configure the Tomcat because the Tomcat is no longer necessary in the VDP administration tool
.. csantos@2018/03/13: Line below is commented because it is not actually true.
.. The client installer of Denodo no longer installs a web container because it is no longer necessary. Previous versions require it to show a simplified version of the user manuals.