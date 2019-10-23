====================================
Generating Wrappers and Data Sources
====================================


.. toctree::
   :hidden:

   creating_data_sources/creating_data_sources.rst
   creating_wrappers/creating_wrappers.rst
   valid_conversions_between_types_in_wrappers_and_vdp_types/valid_conversions_between_types_in_wrappers_and_vdp_types.rst
   query_wrapper_statements/query_wrapper_statements.rst

*Wrappers* are components responsible for offering the server overall
common interface for accessing the data sources. Each search method in a
base relation has an associated *wrapper*, which is in charge of
receiving the queries issued to the base relation, transforming them
into queries to the data source and obtaining the results, returning
them to the logical layer of Virtual DataPort in accordance with the
format specified by the base relation. Wrappers make the peculiarities
of obtaining data from the sources transparent for the server.

Virtual DataPort includes the following predefined types of *wrappers*:


-  *WWW*: This is used to incorporate wrappers for semi-structured sources
   created using Denodo ITPilot into the system. These sources can be
   accessed from the local file system, via Web, or via FTP. HTML websites
   are the most important type of sources this wrapper is used for,
   although it can also be used for other semi-structured sources such as
   PDF or Word files.

-  *JDBC*: Extract data from a remote database via JDBC.

-  *ODBC*: Extract data from a remote database via ODBC.

-  Multidimensional Database wrappers: Extract data from multidimensional
   databases. There is a different wrapper for each type of database:

   -  *SAPBWBAPI*: use the SAP JCo connector to connect to SAP BW and SAP
      BI.
   -  *ESSBASE*: extract data from Oracle Essbase 9 and 11.
   -  *OLAP*: extract data from Mondrian 3.x, Microsoft SQL Server Analysis
      Services or any other OLAP server, using their XMLA interface.

-  *WS* (Web Services): Extract data invoking operations defined by Web
   services.

-  *XML*: Extract data from XML files, optionally following a specific DTD
   or schema. These sources can be accessed via web, local file system or
   FTP.

-  *JSON*: Extract data from JSON files. These sources can be accessed via
   web, local file system or FTP.

-  *DF*: Extract data from flat text files that represent tuples using a
   regular format, such as using specific characters as tuple and field
   delimiters or disposing the tuple fields according to a regular
   expression. Amongst the files supported are files in CSV format. These
   sources can be accessed via web, local file system or FTP.

-  *GS* (Google Search): They provide access to indexes on non-structured
   data created using the search tool Google Search.

-  *LDAP*: They extract data from LDAP servers such as Microsoft Windows
   Server Active Directory.

-  *BAPI*: They invoke SAP BAPIs (Business Application Programming
   Interfaces) to extract data stored in SAP ERP and other SAP
   applications.

-  *SALESFORCE*: They retrieve data from Salesforce.com.

-  *CUSTOM*: Extract data from a source through a specific Java
   implementation. This type of *wrapper* allows *ad hoc* construction of a
   *wrapper program* for a specific type of source.


There are corresponding *data sources* elements for all wrappers, except
WWW-type ones, to encapsulate certain data on data source access and
configuration.

This section describes how to create and modify *wrappers* (and their
*data sources*) of any type in Virtual DataPort by using VQL.

.. note:: We strongly recommend you to use the Administration Tool to
   create data sources and wrappers.

The remainder of this section is structured as follows. The sections :ref:`Valid
Conversions Between Types in Wrappers and VDP Types` and :ref:`Specifying
Paths in Virtual DataPort` define aspects of general interest for the
rest of the section: valid conversions of types between wrappers and
base relations in Virtual DataPort and ways of specifying paths to
resources. The section :ref:`Creating Data Sources` specifies how to add to the
system data sources of the various available types. Finally, section :ref:`Creating Wrappers` shows how to create wrappers for each of these
source types.

