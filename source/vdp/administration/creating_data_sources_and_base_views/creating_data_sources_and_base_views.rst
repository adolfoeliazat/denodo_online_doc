====================================
Creating Data Sources and Base Views
====================================

.. toctree::
   :hidden:
   
   jdbc_sources/jdbc_sources.rst
   odbc_sources/odbc_sources.rst
   soap_web_service_sources/soap_web_service_sources.rst
   multidimensional_database_sources/multidimensional_database_sources.rst
   path_types_in_virtual_dataport/path_types_in_virtual_dataport.rst
   xml_sources/xml_sources.rst
   json_sources/json_sources.rst
   delimited_file_sources/delimited_file_sources.rst
   excel_sources/excel_sources.rst
   web_sources_www/web_sources_www.rst
   google_search_sources/google_search_sources.rst
   aracne_sources/aracne_sources.rst
   ldap_sources/ldap_sources.rst
   bapi_sources/bapi_sources.rst
   salesforce_sources/salesforce_sources.rst
   custom_sources/custom_sources.rst
   viewing_the_schema_of_a_base_view/viewing_the_schema_of_a_base_view.rst
   data_source_configuration_properties/data_source_configuration_properties.rst
   source_refresh/source_refresh.rst
   working_with_blob_fields_of_base_views/working_with_blob_fields_of_base_views.rst

To retrieve data from a repository of data (a database, a SOAP web service, an XML file, etc.), first you need to create a data source in Denodo. A data source represents one repository of data. Then, you need to create one or more base views over each data source. The base views represent the entities in the source. For example, each base view of a JDBC data source is associated to a table or view of the database; each base view of a SOAP data source represents an operation of the web service, etc.

The easiest way to do this is using the administration tool. You can also do it using VQL (Denodo's extension of SQL), however it is more cumbersome and prone to errors.

The following subsections explain how to use the administration tool to create a data source of each type:

-  :ref:`JDBC databases <JDBC Sources>`
-  :ref:`ODBC sources <ODBC Sources>`
-  :ref:`SOAP web services <SOAP Web Service Sources>`
-  :ref:`Multidimensional databases <Multidimensional Database Sources>`
-  :ref:`XML files <XML Sources>`
-  :doc:`JSON files </vdp/administration/creating_data_sources_and_base_views/json_sources/json_sources>`
-  :ref:`Delimited files <Delimited File Sources>` like CSV files
-  :ref:`Excel files <Excel Sources>`.
-  :ref:`Denodo ITPilot wrappers<Web Sources (WWW)>` that extract data from web sites
-  :ref:`Google Search<Google Search Sources>` appliances
-  :ref:`LDAP servers <LDAP Sources>`
-  :ref:`SAP BAPIs <BAPI Sources>`
-  :ref:`Salesforce.com <Salesforce Sources>`
-  :ref:`Custom sources developed using the Denodo API <Custom Sources>`
 
After creating the necessary data sources, you can create :ref:`derived views<Creating Derived Views>` that combine and transform the data obtained from the sources.
