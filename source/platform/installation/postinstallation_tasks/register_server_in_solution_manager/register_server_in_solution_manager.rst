.. _install-license:

==================================================
Register the Denodo Servers in the License Manager
==================================================

If you have not done this already, register the Scheduler and Virtual DataPort of this installation on the Denodo License Manager of your organization. The section :ref:`License Management` of the Solution Manager Administration Guide explains how to do this.

The servers of this installation will not start if you do not register them in the Solution Manager or they cannot reach it.

If the License Manager has SSL enabled, follow these steps:

   1. Stop all the servers and tools of this installation.
   #. Close the Control Center of this installation.
   #. Edit the file :file:`{<DENODO_HOME>}/conf/SolutionManager.properties`.
   #. Uncomment the property ``com.denodo.license.security.ssl.enabled`` **and** set it to ``true``.

|

If during the installation you did not provide the host and port of the License Manager or you want to change it, do this:

a. Open the Denodo Control Center of this installation, click **Configure** and in the section **License Manager Configuration**, enter the host and port.
b. Or on a host without graphical interface, edit the file :file:`{<DENODO_HOME>}/conf/SolutionManager.properties` and set the value of these properties:

   - `com.denodo.license.host`: the License Manager's host.
   - `com.denodo.license.port`: the License Manager's port.

If you installed Denodo Express, you do not need to do this because Denodo Express does not support the License Manager. Instead, you have to use the license you downloaded from the site.