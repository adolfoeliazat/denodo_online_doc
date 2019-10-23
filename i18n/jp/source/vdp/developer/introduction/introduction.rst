============
Introduction
============

.. toctree::
   :hidden:

   examples/examples

**Denodo Virtual DataPort** is a **global solution** for integrating
heterogeneous and distributed data sources.

Virtual DataPort the data that are relevant to the company,
regardless of their origin, format and level of structure. It
incorporates these data into its data system in real time or with
configurable preloads and facilitates the construction of services of
high strategic and functional value for both corporate and business use.

Virtual DataPort is based on a client-server architecture, where clients
issue statements to the server written in VQL (Virtual Query Language),
a SQL-like language used to query and define data views (see the :doc:`/vdp/administration/index` and the 
:doc:`/vdp/vql/index`).

This document introduces product users to the mechanisms available for
client applications to use Denodo Virtual DataPort, making the most of
its data integration facilities.

Client applications can access Virtual DataPort in several ways:

* Using the JDBC interface (`Java Database Connectivity <https://www.oracle.com/technetwork/java/javase/jdbc/index.html>`_).
  Virtual
  DataPort provides a JDBC driver that client applications can use for
  this purpose (see section :ref:`Access Through JDBC`)

* Using the ODBC interface (`Open Database Connectivity <https://support.microsoft.com/kb/110093>`_). Virtual DataPort provides an
  ODBC interface and an ODBC driver for ODBC clients (see section
  :ref:`Access Through ODBC`)

* Using an ADO.NET Data Provider (see section :ref:`Access Through an
  ADO.NET Data Provider`).

* Using the SOAP and REST Web Service interfaces:


  -  Access through the generic RESTful Web Service interface. This
     service is automatically deployed on
     http://localhost:9090/denodo-restfulws.
  -  With the Virtual DataPort administration tool, you can publish SOAP
     or REST Web services that publish the contents of one or more views.
     See more about this in the section 
     :doc:`Publication of Web services <../../administration/publication_of_web_services/publication_of_web_services>` 
     of the Administration Guide.
