===================
Custom Data Sources
===================

Virtual DataPort allows creating wrappers ad-hoc for data sources for
which no standard connector is available. To do so, two Java classes
must be created to implement the required behavior (see section :ref:`CUSTOM
Wrappers`). After this, package these classes in a jar file and import it into Virtual DataPort.

Once the jar file has been imported, create a custom data source referencing the main class of the wrapper with the statement CREATE DATASOURCE CUSTOM.

.. code-block:: bnf
   :caption: Syntax of the CREATE DATASOURCE CUSTOM statement
   :name: Syntax of the CREATE DATASOURCE CUSTOM statement

   CREATE [ OR REPLACE ] DATASOURCE CUSTOM <name:identifier>
       [ FOLDER = <literal> ]
       CLASSNAME = <class name:literal>
       [ CLASSPATH = <classPath:literal> [ <classPath:literal> ]* ]
       [ JARS = <jar name:literal> [, <jar name:literal>]* ]
       [ TRANSFER_RATE_FACTOR = <double value> ]
       [ DESCRIPTION = <literal> ]

To modify a custom data source use ALTER DATASOURCE CUSTOM.

.. code-block:: bnf
   :caption: Syntax of the ALTER DATASOURCE CUSTOM statement
   :name: Syntax of the ALTER DATASOURCE CUSTOM statement

   ALTER DATASOURCE CUSTOM <name:identifier>
       [ CLASSNAME = <class name:literal> ]
       [ CLASSPATH = <classPath:literal> ]
       [ JARS = <jar name:literal> [, <jar name:literal>]* ]
       [ TRANSFER_RATE_FACTOR = <double value> ]
       [ DESCRIPTION = <literal> ]


|

Explanation of some of the parameters of these statements:

-  ``name``. Name to be given to the data source in Virtual DataPort.
-  ``CLASSNAME``. Name of the class implementing the specific wrapper
   for the source. See section :ref:`CUSTOM Wrappers`.
-  ``CLASSPATH``. (optional) Additional classpath required for running
   the wrapper.
-  ``TRANSFER_RATE_FACTOR``: relative measure of the speed of the network connection between the Denodo server and the data source. Use the default value (e.g. 1 for JDBC databases located on premises) if the data source is accessible through a conventional 100 Mbps LAN. Use higher values for faster networks and lower values for data sources accessible through a WAN.
   
   The cost optimizer uses this value when evaluating the cost of an execution plan. The default value is usually correct so you should not specify this parameter unless you have a deep knowledge of the cost optimizer. 
