===========================================================
Changing Settings of Virtual DataPort and the Web Container
===========================================================

The command ``SET`` adds, modifies or deletes a property from the configuration file of Virtual DataPort (:file:`{<DENODO_HOME>}/conf/vdp/VDBConfiguration.properties`).

The command ``WEBCONTAINER SET`` adds or modifies a property of the configuration file of the web container of Denodo (:file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/tomcat.properties`).

You can change almost all settings of Virtual DataPort from the administration tool (dialog *Administration* > *Server Configuration*). However, there a few settings that involve modifying these files. Although you can edit the file directly, we recommend doing it with the commands ``SET`` and ``WEBCONTAINER SET`` instead. The benefits of using these commands over modifying the files directly are:

-  When you change the value of some of properties, the value may be applied immediately. This is only true for some properties. However, if you change the file directly, you always have to restart the Virtual DataPort server to apply the change.
-  It is more convenient because you do not have to connect to the host where the server runs.
-  If you change the amount of memory allocated for the Virtual DataPort server directly in the file, then you have to execute the script :file:`{<DENODO_HOME}/bin/regenerateScripts`. If you modify this with the command ``SET``, you do not need to do this but you still have to restart to apply the change.

.. code-block:: bnf
   :name: Syntax of the command SET
   :caption: Syntax of the command SET
   
   ; This adds/replaces a property
   SET <property name:literal> = <value:literal>
   
   ; This removes a property
   SET <property name:literal> = NULL

.. code-block:: bnf
   :name: Syntax of the command WEBCONTAINER SET
   :caption: Syntax of the command WEBCONTAINER SET

   WEBCONTAINER SET <property name:literal> = <value:literal>

.. rubric:: Considerations

-  When changing the value of certain properties, you may need to restart the Virtual DataPort server. For others, the new value is applied immediately. The documentation of the property mentions this.

-  Only administrators can execute the command ``SET``.

.. rubric:: Examples

.. rubric:: Example 1

Execute this to allocate 4 gigabytes of memory for the Virtual DataPort server and change other settings of the Java Virtual Machine:

.. code-block:: vql

   SET 'java.env.DENODO_OPTS_START' = '-server  -Xms4g  -Xmx4g  -XX:+DisableExplicitGC -XX:+UseConcMarkSweepGC -XX:NewRatio=4 -XX:CMSInitiatingOccupancyFraction=60 -XX:ReservedCodeCacheSize=256m';

This changes the value of the property "java.env.DENODO_OPTS_START" and invokes the script :file:`{<DENODO_HOME}/bin/regenerateScripts`. If you changed this property directly on the file, the change does not apply until you execute this script and restart.

You need to restart the Virtual DataPort server to apply this change.

.. rubric:: Example 2

Execute this to allocate 2 gigabytes of memory for the web container of Denodo and to change other settings of the Java Virtual Machine:

.. code-block:: vql

   WEBCONTAINER SET 'java.env.DENODO_OPTS_START' = '-mx2g -Dorg.apache.tomcat.util.buf.UDecoder.ALLOW_ENCODED_SLASH=true -Dorg.apache.catalina.connector.CoyoteAdapter.ALLOW_BACKSLASH=true';

You need to restart the web container to apply any change to a property of the web container.