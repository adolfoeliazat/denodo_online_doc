================
XML Data Sources
================

Virtual DataPort allows using XML as data sources. To define an XML data
source, it is necessary to specify the access path to the XML document
and, optionally, the access path to the file containing the schema or
DTD of same.

-  ``SCHEMA`` or ``DTD`` (optional): Path to the file that contains the
   metadata of the data source XML file. It may be an XML Schema or a
   DTD. If it is not specified, Virtual DataPort will try to infer an
   appropriate schema by analyzing the XML document structure indicated
   in the next parameter.
-  ``ROUTE``: path to the XML file that represents the data source. See
   more information about paths to data files in the section :ref:`Specifying
   Paths in Virtual DataPort`.
-  ``FILTER``: List of filters that will be applied to a file before
   processing it. They can be applied to the XML file and the XML Schema
   or DTD (see section :doc:`/vdp/vql/generating_wrappers_and_data_sources/valid_conversions_between_types_in_wrappers_and_vdp_types/filters`)


.. code-block:: bnf
   :caption: Syntax of the CREATE DATASOURCE XML statement
   :name: Syntax of the CREATE DATASOURCE XML statement

   CREATE [ OR REPLACE ] DATASOURCE XML <name:identifier>
       [ FOLDER = <literal> ]
       [ { SCHEMA | DTD } <route> [ <route_filters> ] ]
       ROUTE <route> [ <route_filters> ]
       [ VALIDATE = { TRUE | FALSE } ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]

   <route_filters> ::= FILTER ( <filter> [, <filter> ]* )
   
.. 

   <route> ::= (see :ref:`Syntax to set the path in a DF, JSON or XML data source`)
   
   <filter > ::= (see :ref:`Syntax to set a filter in a DF, JSON or XML data source`)
   

This statement also allows the ``OR REPLACE`` clause. In this case, if
there is already a data source with the same name, its definition is
replaced with the new one.

The syntax of the modification statement of an XML data source is shown
below.



.. code-block:: bnf
   :caption: Syntax of the ALTER DATASOURCE XML statement
   :name: Syntax of the ALTER DATASOURCE XML statement

   ALTER DATASOURCE XML <name:identifier>
       [ { SCHEMA | DTD } <route> [ <route_filters> ] ]
       [ ROUTE <route> <route_filters> ]
       [ VALIDATE = { TRUE | FALSE } ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]

   <route_filters> ::= 
     FILTER ( <filter> [, <filter> ]* )

..

   <route> ::= (see :ref:`Syntax to set the path in a DF, JSON or XML data source`)

   <filter> ::= (see :ref:`Syntax to set a filter in a DF, JSON or XML data source`)

|

Explanation of some of the parameters of these statements:  

-  ``TRANSFER_RATE_FACTOR``: relative measure of the speed of the network connection between the Denodo server and the data source. Use the default value (e.g. 1 for JDBC databases located on premises) if the data source is accessible through a conventional 100 Mbps LAN. Use higher values for faster networks and lower values for data sources accessible through a WAN.
