.. _installation_guide_configuration_of_the_jvm_parameters_from_the_command_line:

=========================================================
Configuration of the JVM Parameters from the Command Line
=========================================================

In a host with graphical support, you can use the Denodo Control Center
to change the parameters of the Java Virtual Machine (JVM) used to
launch each Denodo server (see section :ref:`Denodo Platform Configuration`).
However, this is not possible in a
“headless” environment (i.e. without graphical support).

To change these parameters on a host without graphical support, follow these steps:

#. On each file of the column *Configure File* of the table below, the property ``java.env.DENODO_OPTS_START``
   contains the parameters passed to the Java Runtime Environment (JRE) when launching each server.
   
   .. note:: The character ``:`` has to be escaped with the character ``\``. E.g.
      ``-XX\:NewRatio\=4``

.. table:: Properties to modify to change the JVM parameters
   :name: Properties to modify to change the JVM parameters

   +-----------------------------+-------------------------------------------------------------------+
   | Component                   | Configuration File                                                |
   +=============================+===================================================================+
   | Virtual DataPort server     | <DENODO_HOME>/conf/vdp/VDBConfiguration.properties                |
   |                             |                                                                   |
   | ITPilot Wrapper server      |                                                                   |
   +-----------------------------+-------------------------------------------------------------------+
   | ITPilot Browser Pool        | <DENODO_HOME>/conf/iebrowser/IEBrowserConfiguration.properties    |
   +-----------------------------+-------------------------------------------------------------------+
   | ITPilot Verification Server | <DENODO_HOME>/conf/maintenance/ConfigurationParameters.properties |
   +-----------------------------+-------------------------------------------------------------------+
   | Scheduler Server            | <DENODO_HOME>/conf/scheduler/ConfigurationParameters.properties   |
   +-----------------------------+-------------------------------------------------------------------+
   | Scheduler Index Server      | <DENODO_HOME>/conf/arn-index/ConfigurationParameters.properties   |
   +-----------------------------+-------------------------------------------------------------------+
   | Web Container               | <DENODO_HOME>/resources/apache-tomcat/conf/tomcat.properties      |
   +-----------------------------+-------------------------------------------------------------------+

2. To change the host name on which one of the components below listens to connection, edit the file of the column *Configuration File* and change the property *Property to Modify*.

   For example, for the Virtual DataPort server, edit :file:`{<DENODO_HOME>}/conf/vdp/VDBConfiguration.properties` and change the value of ``com.denodo.vdb.vdbinterface.server.VDBManagerImpl.registryURL`` to ``denodo-dv1-prod.contoso.com``.

.. table:: Properties to modify to change the RMI host
   :name: Properties to modify to change the RMI host

   +-----------------------------+-------------------------------------------------------------------+---------------------------------------------------------------+
   | Component                   | Configuration File                                                | Property to Modify                                            |
   +=============================+===================================================================+===============================================================+
   | Virtual DataPort server     | <DENODO_HOME>/conf/vdp/VDBConfiguration.properties                | com.denodo.vdb.vdbinterface.server.VDBManagerImpl.registryURL |
   |                             |                                                                   |                                                               |
   | ITPilot Wrapper server      |                                                                   |                                                               |
   +-----------------------------+-------------------------------------------------------------------+---------------------------------------------------------------+
   | Scheduler Server            | <DENODO_HOME>/conf/scheduler/ConfigurationParameters.properties   | Launcher/registryURL                                          |
   +-----------------------------+-------------------------------------------------------------------+---------------------------------------------------------------+
   

3. Execute the script :file:`{<DENODO_HOME>}/bin/regenerateFiles.sh`

   This script regenerates the scripts that launch each component.

The next time you launch any of the Denodo servers, they will run with
the new parameters.

You only need to execute the script ``regenerateFiles.sh`` when you change the RMI host or the JVM properties.
