===========================================
Installing the Connector for Oracle Essbase
===========================================

In order to retrieve data from Oracle Essbase, you have to install its
connector.

The Oracle Essbase API provides two modes of connecting to Essbase
servers. The section :ref:`Multidimensional Data Sources to Oracle Essbase`
of the Virtual DataPort Administration Guide provides more information
about these modes.

The set of drivers you have to install, depend on the connection mode
used:

#. *Three-tier APS mode*: obtain the ``ess_japi.jar``.
#. *Embedded mode*: check the Administration Guide of Oracle Essbase to
   obtain the list of jars that a Java application needs to connect to
   Essbase in Embedded mode. Make sure that the version of the Essbase
   Administration Guide matches the version of your Essbase server
   because the jars required change depending on the release of Essbase
   you want to connect to.

After obtaining the appropriate jars, copy them to the Denodo
installation:

-  Copy the *Oracle Essbase version 9* drivers to the directory
   :file:`{<DENODO_HOME>}/lib/extensions/essbase-drivers/9`
-  Copy the *Oracle Essbase version 11* drivers to the directory
   :file:`{<DENODO_HOME>}/lib/extensions/essbase-drivers/11`
