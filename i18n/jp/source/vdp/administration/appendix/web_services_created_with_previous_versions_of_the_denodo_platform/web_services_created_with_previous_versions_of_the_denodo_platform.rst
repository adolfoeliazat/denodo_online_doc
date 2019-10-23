==================================================================
Web Services Created with Previous Versions of the Denodo Platform
==================================================================

.. toctree::
   :hidden:
   
   publishing_web_services.rst
   xslt_transformations_of_web_services_created_with_previous_versions.rst
   soap_over_jms_on_web_services_created_with_previous_versions.rst
   authentication_in_web_services_created_with_previous_versions.rst
   types_conversion_table_for_rest_soap_web_services_created_with_previous_versions.rst
   invoking_web_services_created_with_previous_versions.rst

.. warning:: This section is about the Web services created with previous
   version of the Denodo Platform.

In the previous versions of the Denodo Platform, there was only one type
of Web service, which combined the SOAP and REST types. In the current
version of the Platform, the Web services have been divided into two:
the SOAP ones and the REST ones because the REST Web services have
changed from an “operation-oriented” approach to a “RESTful” approach.

The Web services created with the previous versions of the Platform
still work, but they are deprecated. This appendix explains how to
manage the old Web services.

These Web services provide several representations of the data:

-  SOAP Web Services (`Simple Object Access Protocol - SOAP - version 1.1 <https://www.w3.org/TR/2000/NOTE-SOAP-20000508/>`_)
-  REST Web Services that use HTTP directly as the transport protocol
   and return data encoded in XML
-  JSON Web Services. Similar to the REST-style Web services, although
   the output is in JSON format (`JavaScript Object Notation <http://www.json.org/>`_)
-  RSS Web Services. Similar to the REST-style Web services, although
   the output is produced in RSS format (`Really Simple Syndication Format - RSS - version 2.0 <http://www.rssboard.org/rss-specification>`_).
-  HTML Web Services. The output is an HTML table that displays the
   response to the query

The following sections describe the publication process of these Web
Services.
