=========================================================
Configuration of the JVM Parameters from the Command Line
=========================================================

In a host with graphical support, you can use the Denodo Control Center
to change the parameters of the Java Virtual Machine (JVM) used to
launch each Solution Manager server (see section :doc:`Virtual Machine and Web
Container Configuration <../virtual_machine_and_web_container_configuration/virtual_machine_and_web_container_configuration>`).
However, this is not possible in a “headless” environment (i.e. without graphical support).

In this section, you will learn to change these parameters in a host
without graphical support. To do this, follow these steps:

#. In the first column of the table below, look for the products whose
   JVM parameters you want to change.
#. Edit their configuration file: look for its value in the
   “Configuration file” column.
#. For each file, look for the property of the column “Property to
   Modify” to set the JVM parameters. Note that the characters “:” has
   to be escaped with the character ``\``. E.g.
   ``-XX\:NewRatio\=4``

.. table:: Properties to modify to change the JVM parameters
   :name: Properties to modify to change the JVM parameters

   +-----------------------------+--------------------------------------------------------------------------------------+----------------------------+
   | Product                     | Configuration File                                                                   | Property to Modify         |
   +=============================+======================================================================================+============================+
   | Virtual DataPort server     | <SOLUTION_MANAGER_HOME>/conf/vdp/VDBConfiguration.properties                         | java.env.DENODO_OPTS_START |
   +-----------------------------+--------------------------------------------------------------------------------------+----------------------------+
   | Solution Manager Server     | <SOLUTION_MANAGER_HOME>/conf/solution-manager/SMConfigurationParameters.properties   | java.env.DENODO_OPTS_START |
   +-----------------------------+--------------------------------------------------------------------------------------+----------------------------+
   | License Manager Server      | <SOLUTION_MANAGER_HOME>/conf/license-manager/LMConfigurationParameters.properties    | java.env.DENODO_OPTS_START |
   +-----------------------------+--------------------------------------------------------------------------------------+----------------------------+
   | Web Container               | <SOLUTION_MANAGER_HOME>/resources/apache-tomcat/conf/tomcat.properties               | java.env.DENODO_OPTS_START |
   +-----------------------------+--------------------------------------------------------------------------------------+----------------------------+


4. After saving the changes in the modified files, execute the script
   :file:`{<SOLUTION_MANAGER_HOME>}/bin/regenerateFiles.sh`

The next time you launch any of the Solution Manager servers, they will run with
the new parameters.
