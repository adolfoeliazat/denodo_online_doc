===========
Limitations
===========

This section lists the limitations of the OData service:

**Read-only access**

The access to Denodo databases via this service is read-only.

**Unavailable functions in the $filter system query option**

The service does not support the operator ``has``.

The service does not support these functions:

* ``fractionalseconds``, ``date``, ``time``, ``totaloffsetminutes``, 
  ``mindatetime`` and ``maxdatetime``
* ``isof`` and ``cast``
* ``geo.distance``, ``geo.length`` and ``geo.intersects``

Any requests containing these functions will return the error code: 
``501 Not Implemented``.

**Unavailable operators in the $filter system query option**

The service does not support lambda operators: ``any`` and 
``all``.

**Unavailable literals in the $filter system query option**

The service does not support the literal ``$it`` in 
expressions to refer to the current instance of the collection identified by 
the resource path.

The service does not support the literal ``$root`` in 
expressions to refer to resources of the same service.

**Navigation properties in the $select system query option**

The service does not allow navigation properties as selection clauses.

**Unavailable $search system query option**

The ``$search`` query parameter is not available.

**Unavailable expand option $levels**

The service does not support the ``$levels`` expand option 
that allows requesting recursive expands.

**Unavailable Cross-Join queries**

The service does not support the resource ``~/crossjoin``
to represent Cartesian products of entities.

**Resolving references via the resource $entity**

The service does not support the resource ``~/$entity``
that allows resolving entity references using the query option ``$id``.

**White spaces in the URL**

The service does not allow white spaces between function parameters and
before or after the equal sign (``=``) that is used to specify query options.

**Redundant keys for dependent entities**

The service does not allow the omission of redundant keys when there
are key properties determined by the parent in a relationship. You have to add
all key properties in each level.

**Order by with $expand system query option**

The service does not support the ``$orderby`` query option 
inside ``$expand`` system query option.

**Order by using complex properties**

The service does not support the order by clause using fields of 
registers. Therefore, the ``$orderby`` query option is not available for complex 
properties.

**Navigation using complex properties**

When an end point of an association is a field of a register, Virtual DataPort does not allow to traverse the association from that endpoint.

**Referencing expanded entities**

The service does not support referencing a single entity 
in a "to-one" relationship when the related data is requested using the 
``$expand`` query option and the selected representation for the result is JSON.

Example of this limitation:

.. code-block:: none

  /denodo-odata.svc/movies/city?$expand=country/$ref

Note that this request will fail because JSON is the default format.

**Admin user and Kerberos authentication**

The service does not disable user access to the ``admin`` user when 
using Kerberos authentication.
