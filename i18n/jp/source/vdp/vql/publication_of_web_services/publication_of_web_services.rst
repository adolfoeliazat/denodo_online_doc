===========================
Publication of Web Services
===========================


.. toctree::
   :hidden:

   creating_soap_web_services/creating_soap_web_services.rst
   modifying_soap_web_services/modifying_soap_web_services.rst
   creating_rest_web_services/creating_rest_web_services.rst
   modifying_rest_web_services/modifying_rest_web_services.rst
   connection_parameters/connection_parameters.rst
   web_services_authentication/web_services_authentication.rst
   embedded_web_container_management/embedded_web_container_management.rst
   deploying_and_exporting_soap_and_rest_web_services/deploying_and_exporting_soap_and_rest_web_services.rst

With Virtual DataPort you can publish views as a Web service to enable
external applications that cannot use the JDBC or ODBC interfaces, to
retrieve data from Virtual DataPort.

You can deploy these Web services to the Web Service container embedded
in the Denodo Platform, or generate a ``war`` file to deploy it in an
external J2EE applications server.

In Virtual DataPort there are two types of Web services:

-  SOAP Web service: set of operations that when invoked, the Service
   executes a specific a VQL query over a view.

-  REST Web service: set of views and associations. In a REST Web
   service, you do not define operations over these views. Instead, the
   Service follows the same RESTful approach as the Denodo RESTful Web
   service (see section :ref:`RESTful Web Service` of the Administration
   Guide).
   These Web services provide four different representations of the data.

   -  HTML
   -  XML
   -  JSON
   -  RSS

The main difference between the two types is that the SOAP Web services
are “operation-oriented” and the REST Web services are
“resource-oriented”.

Both types provide different authentication methods to validate users’
identity (see section :doc:`./connection_parameters/connection_parameters`).

This section describes how to create and deploy SOAP and REST Web
services using VQL statements.

In the previous versions of the Denodo Platform, there was only one type
of Web service, which combined the SOAP and REST types. In the current
version of the Platform, the Web services have been divided into two:
the SOAP ones and the REST ones because the REST Web services have
changed from an “operation-oriented” approach to a “RESTful” approach.

The Web services created with the previous versions of the Platform
still work, but they are deprecated. We have moved the sections
regarding the managing the old Web services to the appendix :doc:`Web
Services Created with Previous Versions of the Denodo Platform <../appendix/web_services_created_with_previous_versions_of_the_denodo_platform/web_services_created_with_previous_versions_of_the_denodo_platform>`.

.. note:: We *strongly* recommend you to define new Web services using
   the Virtual DataPort Administration Tool (see the section `Publication
   of Web services` of the Administration Guide), because the syntax of
   the ``CREATE REST WEBSERVICE`` and ``CREATE SOAP WEBSERVICE`` statements
   is very complex.
