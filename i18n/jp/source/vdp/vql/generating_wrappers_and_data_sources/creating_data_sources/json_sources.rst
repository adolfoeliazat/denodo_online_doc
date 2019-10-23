============
JSON Sources
============

Virtual DataPort allows using JSON files as data sources.

To define a JSON data source you have to specify the access path to the
JSON document (``ROUTE`` clause). The path may include interpolation
variables to parameterize the access path depending on the
conditions of the query executed at runtime (see more about
interpolation variables in the section :ref:`Execution Context and Interpolation
Strings`).

To create a JSON data source, use the statement CREATE DATASOURCE JSON.

.. code-block:: bnf
   :caption: Syntax of the CREATE DATASOURCE JSON statement
   :name: Syntax of the CREATE DATASOURCE JSON statement

   CREATE [ OR REPLACE ] DATASOURCE JSON <name:identifier>
       [ FOLDER = <literal> ]
       ROUTE <route> [ <route_filters> ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]

   <route_filters> ::= 
     FILTER ( <filter> [, <filter> ]* )

..

   <route> ::= (see :ref:`Syntax to set the path in a DF, JSON or XML data source`)

   <filter> ::= (see :ref:`Syntax to set a filter in a DF, JSON or XML data source`)


To modify a JSON data source, use the statement ALTER DATASOURCE JSON.

.. code-block:: bnf
   :caption: Syntax of the ALTER DATASOURCE JSON statement
   :name: Syntax of the ALTER DATASOURCE JSON statement

   ALTER DATASOURCE JSON <name:identifier>
       [ ROUTE <route> [ <route_filters> ] ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]

   <route_filters> ::= 
     FILTER ( <filter> [, <filter> ]* )

.. 

   <filter> ::= (see :ref:`Syntax to set a filter in a DF, JSON or XML data source`)

   <route> ::= (see :ref:`Syntax to set the path in a DF, JSON or XML data source`)

|

Explanation of some of the parameters of these statements:  

-  ``OR REPLACE``: If present and a data source with the same name exists,
   the current definition is substituted with the new one.
-  ``TRANSFER_RATE_FACTOR``: relative measure of the speed of the network connection between the Denodo server and the data source. Use the default value (e.g. 1 for JDBC databases located on premises) if the data source is accessible through a conventional 100 Mbps LAN. Use higher values for faster networks and lower values for data sources accessible through a WAN.
