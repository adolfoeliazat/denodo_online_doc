===========================
Publication of Web Services
===========================

.. toctree::
   :hidden:
   
   publishing_soap_web_services/publishing_soap_web_services.rst
   publishing_rest_web_services/publishing_rest_web_services.rst
   xslt_transformations/xslt_transformations.rst
   web_services_authentication/web_services_authentication.rst
   how_web_ services_query_the_virtual_dataport_server/how_web_ services_query_the_virtual_dataport_server.rst
   web_service_container_status_table/web_service_container_status_table.rst
   invoking_the_exported_web_services/invoking_the_exported_web_services.rst
   
Virtual DataPort can publish views as Web services to enable external
applications that cannot use the JDBC or ODBC interfaces, to retrieve
data from Virtual DataPort.

You can deploy these Web services to the Web service container embedded
in the Denodo Platform, or generate a ``war`` file to deploy them in an
external J2EE applications server.

In Virtual DataPort, there are two types of Web services:


-  SOAP Web services (`Simple Object Access Protocol - SOAP - version 1.1 <https://www.w3.org/TR/2000/NOTE-SOAP-20000508/>`_): publish a set of
   operations that when invoked, the Service executes a specific a VQL
   query over a view or a Denodo stored procedure.


-  REST Web services: publish a set of views and associations. In a REST
   Web service, you do not define operations over these views. Instead,
   the Service follows the same RESTful approach as the Denodo RESTful
   Web service (see section :ref:`RESTful Web service`).
   
   The main difference with the SOAP Web services is that the SOAP ones
   are “operation-oriented” and the REST ones are “resource-oriented”.
   
   These Web services provide four different representations of the data:

   -  HTML
   -  XML
   -  JSON (`JavaScript Object Notation <http://www.json.org/>`_)
   -  RSS


This section describes how to create and deploy SOAP and REST Web
services.

Both types provide several authentication methods to validate the
credentials of the users (see section :doc:`./web_services_authentication/web_services_authentication`).

In the previous versions of the Denodo Platform, there was only one type
of Web service, which combined the SOAP and REST representations. In the
current version of the Denodo Platform, the Web services have been
divided into two: the SOAP ones and the REST ones. The reason is that
the REST Web services have changed from an “operation-oriented” approach
to a “RESTful” approach.

The Web services created with the previous versions of the Platform
still work, but they are deprecated. We have moved the sections about
the managing of the old Web services to the appendix :doc:`/vdp/administration/appendix/web_services_created_with_previous_versions_of_the_denodo_platform/web_services_created_with_previous_versions_of_the_denodo_platform`.

