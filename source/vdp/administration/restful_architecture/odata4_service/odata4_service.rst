=================================================================================
Denodo OData 4.0 Service
=================================================================================

.. toctree::
   :hidden:
   
   odata4_service_serving_metadata.rst
   odata4_service_querying_data_basics.rst
   odata4_service_associations.rst
   odata4_service_querying_data_advanced.rst
   odata4_service_pagination.rst
   odata4_service_configuration.rst
   odata4_service_debug.rst
   odata4_service_requirements.rst
   odata4_service_limitations.rst

`OData <https://www.odata.org/>`_ (Open Data Protocol) is a REST-based protocol 
for querying and updating data using simple HTTP messages. It is an OASIS 
standard based on technologies such as HTTP, Atom/XML and JSON.

Virtual DataPort provides an OData 4.0 compliant interface, through is "Denodo OData service".

The OData Service is available at:
``https://denodo-server.acme.com:9090/denodo-odata4-service/denodo-odata.svc/<database name>/``.

.. note:: The segment "<database name>" is mandatory.

Features
========

The standard OData 4.0 defines three levels of conformance for OData services. 
The Denodo OData 4.0 Service conforms to the *Intermediate Conformance Level* by 
providing the following functionality:

* Read-only access to Denodo databases:

  * Metadata
  
  * Entities
  
  * Items of an entity
  
  * Properties of an item
  
  * Property values
  
  * Relationships between entities
  
* Format results in Atom and JSON

* Query options

  * $select
  
  * $filter
  
  * $expand
  
  * $orderby
  
  * $skip
  
  * $top
  
  * $count
  
* Parameter aliases

* Pagination

* HTTP Authentication

  * Basic
  
  * Kerberos

