===============================================
Virtual Machine and Web Container Configuration
===============================================

Click **Configure** to open the “Denodo Platform Control Center
configuration”. In this screen, you can do the following:

-  Select the Java Virtual Machine (JVM) used to run the Denodo Platform
   servers and tools.
-  Configure the JVM parameters used to launch each Denodo server and tool.
-  Install a license file.
-  Select the ports that the Denodo embedded Web container listens for incoming
   connections.

|

To select the Java Virtual Machine (JVM) used to run the Solution Manager
servers, click **Configure** and then, **Edit**. In this dialog, you can
add a JVM present on your system that you want the Solution Manager
servers to run with. The Solution Manager servers will run with the JVM that is
selected on the list.

Although it is possible to change the JVM, we recommend running the
Solution Manager servers with the “Denodo (Internal JVM)”, unless you have used
the ``denodo-install-solutionmanager-7.0.zip`` installer, which does not include a Java
Runtime Environment (JRE).

|

To configure the JVM options of each Solution Manager server, click Configure and
then, JVM options. This dialog has two tabs:

#. **Memory options**. In this tab, you can change the JVM parameters of
   each Solution Manager server.
   The section :doc:`Configuration of the JVM Parameters from the Command
   Line <../configuration_of_the_jvm_parameters_from_the_command_line/configuration_of_the_jvm_parameters_from_the_command_line>` 
   explains how to change these parameters in a host without
   graphical support.
#. **RMI host**. In this tab, you can configure the RMI host name for
   each server of the Solution Manager. You have to change this if the
   host where the server runs provides several network
   interfaces. In this case, you have to specify a network interface (as
   an IP address or a host name) that is visible to clients that have to
   connect to this server.

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
Administration Tool (see section :doc:`Server Administration - Configuring the Server </vdp/administration/server_administration_-_configuring_the_server/server_administration_-_configuring_the_server>`
of the Virtual DataPort Administration Guide for information about how to do this).


.. |image0| image:: DenodoPlatform.InstallationGuide-24.png
