==============
Physical Layer
==============

The physical layer abstracts the higher architecture layers of the
difficulties of interacting with the data sources. It also provides a
vision of the data sources according to a common metamodel.

These tasks are carried out through the so-called wrappers. A wrapper
extracts data from a source, interprets the results obtained and returns
them to the system in the format required by Virtual DataPort metamodel.
Furthermore, where permitted by the source, a wrapper can also insert,
update and/or delete information in a source.

The *wrappers* allow Virtual DataPort to process all the external
sources in a consistent manner, without concern for their specific
characteristics. Virtual DataPort directly provides the following types
of *wrappers*:

-  Relational database wrappers: they extract data from a Remote
   Database via JDBC or ODBC. Furthermore, where permitted by the
   source, a wrapper inserts, updates and/or deletes information in a
   source.
-  Multidimensional database wrappers: they extract data from
   multidimensional databases such as SAP BW, Mondrian, Oracle Essbase
   and Microsoft SQL Server Analysis Services.
-  Web services wrappers: they extract data invoking operations provided
   by SOAP Web services.
-  XML wrappers: they allow extracting data encapsulated in XML files.
   Those files can optionally be validated using a specific DTD or
   schema. XML documents can be accessed on the local drive or remotely
   accessed via protocols such as http or FTP. A common use of this type
   of wrapper is the importing of REST Web services (including RSS and
   ATOM web services).
-  JSON wrappers: they extract data from documents in JSON format
   (`JavaScript Object Notation <http://www.json.org/>`_). They are
   useful to access REST Web services returning their output in JSON
   format.
-  Delimited files wrappers: they extract data from delimited files in
   CSV format (Comma Separated Values) or similar. CSV documents can be
   accessed on the local drive or on remote locations via HTTP or FTP.
-  Excel wrappers: they extract data from Microsoft Excel files. There
   are several configuration parameters that allow you to extract data
   from one or more worksheets, only from a certain range, etc.
-  Web wrappers: they provide access to semi-structured data contained
   in websites. For instance, these wrappers can automate the process of
   querying a web form and extracting the results. These wrappers are
   generated with Denodo ITPilot.
-  Aracne wrappers (*deprecated*): see section :ref:`Denodo Aracne Wrappers <Denodo Aracne Wrappers>`.
-  Google Search wrappers: `Google Search <https://enterprise.google.com/search//>`_
   is Google’s corporate
   solution for crawling, indexing and searching websites. The indexes
   created using Google Search may be imported directly into Denodo
   Virtual DataPort for querying and combining them with structured and
   semi-structured data.
-  LDAP wrappers: they extract data contained in LDAP directories such
   as Microsoft Windows Server Active Directory.
-  BAPI wrappers: they invoke SAP BAPIs (Business Application
   Programming Interfaces) to extract data stored in SAP ERP and other
   SAP applications.
-  Salesforce.com wrappers: they extract data from a Salesforce.com. 
-  Custom wrappers: they extract data from a source through a Java
   implementation provided by the Virtual DataPort administrator.
   Virtual DataPort provides an API, so users can create their own
   wrappers for specific sources. The CUSTOM wrappers also allow
   inserting, updating and/or deleting data from the sources.

The Virtual DataPort Administration Tool creates all these wrappers
automatically, except the Web (WWW) ones, which are created with the
:ref:`ITPilot Wrapper Generation Tool <ITPilot Generation Environment Guide>`.
