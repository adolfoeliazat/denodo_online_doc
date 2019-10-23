====================
RESTful Architecture
====================

.. toctree::
   :hidden:
   
   primary_keys_of_views/primary_keys_of_views.rst
   associations/associations.rst
   restful_web_service/restful_web_service.rst
   odata4_service/odata4_service.rst
   

This section describes the REST architectural style of the Denodo
Platform.

The main feature of this architecture is the RESTful Web Service, 
which provides access to the views created in
the Virtual DataPort server.

This service depends on the following features:

-  Primary keys of views: see section :ref:`Primary Keys of Views`.
-  Associations between views: see section :doc:`./associations/associations`.
-  Navigational queries (``SELECT_NAVIGATIONAL``), which can traverse
   the associations defined between views. The RESTful Web service
   executes these queries to obtain the views requested by clients.
   See more about this in the section :doc:`Navigational Queries </vdp/vql/restful_architecture/restful_architecture>` of the
   VQL Guide.

The :ref:`Denodo OData 4.0 Service <Denodo OData 4.0 Service>` is also included 
with Virtual DataPort, and provides access to the views created in Virtual 
DataPort, with the OData (Open Data Protocol) format.
