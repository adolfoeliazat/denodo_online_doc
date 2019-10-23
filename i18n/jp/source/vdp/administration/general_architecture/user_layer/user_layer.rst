==========
User Layer
==========

The user layer implements the interface between the client applications
and the Virtual DataPort server. Thus, its responsibility is to provide
client applications with means to establish a connection with the
Server, send queries and obtain the results.

When the logical layer returns a response to the query, this layer is
also responsible for providing it to the client application through the
connection interfaces defined to this effect. Denodo Virtual DataPort can be accessed via JDBC,
ODBC, SOAP, REST, JSON and RSS Web Services (see section :doc:`/vdp/administration/publication_of_web_services/publication_of_web_services`), JSR-168/286 Portlets and Microsoft Web Parts (see
section :ref:`Publication of Views as Widgets`) and the RESTful Web service (see section :doc:`/vdp/administration/restful_architecture/restful_architecture`).
