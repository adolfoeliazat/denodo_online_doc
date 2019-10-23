.. _control-center-configuration:

=============================
Denodo Platform Configuration
=============================

Click **Configure** to open the “Denodo Platform Control Center
configuration”. In this screen, you can do the following:

-  Select the Java Virtual Machine (JVM) used to run the Denodo Platform
   servers and tools.
-  Configure the JVM parameters used to launch each Denodo server and tool.
-  Install a license file or configure access to a Denodo License Manager.
-  Select the ports that the Denodo embedded Web container listens for incoming
   connections.

   .. note:: Not all the options are available in all the editions of the
      Denodo Platform (regular or client).

|

To select the Java Virtual Machine (JVM) used to run the Denodo Platform
servers, click **Configure** and then, **Edit**. In this dialog, you can
add a JVM present on your system that you want the Denodo Platform
servers to run with. The Denodo servers will run with the JVM that is
selected on the list.

Although it is possible to change the JVM, we recommend running the
Denodo servers with the “Denodo (Internal JVM)”, unless you have used
the ``denodo-install-7.0.zip`` installer, which does not include a Java
Runtime Environment (JRE).

|

To configure the JVM options of each Denodo server, click Configure and
then, JVM options. This dialog has two tabs:

#. **Memory options**. In this tab, you can change the JVM parameters of
   each Denodo server.
   The section :doc:`Configuration of the JVM Parameters from the Command
   Line<../configuration_of_the_jvm_parameters_from_the_command_line/configuration_of_the_jvm_parameters_from_the_command_line>` explains how to change these parameters in a host without
   graphical support.
#. **RMI host**. In this tab, you can configure the RMI host name for
   each server of the Denodo Platform. You have to change this if the
   host where the Denodo server runs provides several network
   interfaces. In this case, you have to specify a network interface (as
   an IP address or a host name) that is visible to clients that have to
   connect to this server.
   
   If the Denodo servers run on Linux, you always have to do this.

In both tabs, next to each field there is a button (|image0|) that can
be used to go back to the default value. The Ok button will save the
configuration changes, which will be applied in the next startup of the
affected programs.

|

To configure the ports that the Denodo embedded Web container listens
for incoming connections, click Configure and enter the desired ports in
**HTTP port number**, **Shutdown port number** and **Auxiliary port
number**.

You can also configure these ports using the Virtual DataPort
Administration Tool (see section :doc:`Server Administration - Configuring the Server <../../../../vdp/administration/server_administration_-_configuring_the_server/server_administration_-_configuring_the_server>`
of the Virtual DataPort Administration Guide for information about how to do this).

|

Configuring the Connection to the License Manager
=================================================

.. TODO for Denodo 8.0: remove the reference to the update of March 2019.

The Denodo server components (Virtual DataPort, Scheduler, etc.) request a license to the License Manager when they start. 
In addition, periodically, they send a request to the License Manager to check that this license is still valid. If these server components cannot contact the License Manager, they will not start.

If you did not provide the connection details to the License Manager when installing the Denodo Platform, follow these steps:

a. On a host with graphical support *and* with the Denodo update of March 2019 or newer, do the following:

   1. Open the Control Center.
   #. Stop all the servers and web applications of this installation of Denodo.
   #. Click **Configure**.
   #. In the section  **LICENSE CONFIGURATION**, enter the **Host** and **Port** of the License Manager.
   #. If SSL/TLS was enabled on the License Manager, select the check box **License Manager uses SSL/TLS**. 
   #. Restart the Control Center

b. On a host *without* graphical support *or* with the update of October 2018 or earlier, do the following:

   1. Stop all the servers and web applications of this installation of Denodo.
   #. Close the Control Center.
   #. Edit the file :file:`{<DENODO_HOME>}/conf/SolutionManager.properties`.
   #. If SSL/TLS was enabled on the License Manager, uncomment the property ``com.denodo.license.security.ssl.enabled`` **and** set it to ``true``.
   #. Start the Control Center.

If you are using Denodo Express, you do not have the License Manager available. In this case, select **Use a license file**, click **Browse...** and select the file with the Denodo license you 
obtained from Denodo. You will need to restart all the servers and tools for the license changes to take effect.

.. |image0| image:: DenodoPlatform.InstallationGuide-24.png