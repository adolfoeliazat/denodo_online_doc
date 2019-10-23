.. _vdp_admin_guide_odata4_service_pagination:

==========
Pagination
==========

When the service has to return a collection of entries whose 
size exceeds that configured at the ``server.pageSize`` property of its 
:doc:`configuration file <odata4_service_configuration>`, it will split the response 
into pages, returning only the first 1000 entries (the default value for page 
size).

The service will add to the response a *next link*, which will 
allow the client to request the next page of the results.

Next links include a ``$skiptoken`` parameter. They look like this:
``/denodo-odata.svc/movies/actor?$skiptoken=1000``

If the original request includes query parameters, the result will show the next 
link with these parameters and a ``$skiptoken`` as in the examples below.

Example request: ``/denodo-odata.svc/movies/actor?$top=2&$skip=1``.

Response::
  
  ... "@odata.nextLink":"/denodo-odata.svc/movies/actor?$top=2&$skip=1&$skiptoken=1000" ...

Besides using the configuration property ``server.pageSize``, users can request 
another page size using the ``odata.maxpagesize`` preference in the ``Prefer``
header.
