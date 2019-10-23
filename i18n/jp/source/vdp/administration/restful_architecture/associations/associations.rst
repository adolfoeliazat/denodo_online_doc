============
Associations
============

.. toctree::
   :hidden:
   
   why_associations_are_useful.rst
   creating_an_association.rst
   multiplicity_of_associations.rst
   referential_integrity_in_associations.rst
   role_preconditions.rst

An association represents a relationship between two views. It is
defined by two endpoints and a list of mappings. Each endpoint is
associated with a view and has a “role name”, a description and a
multiplicity. The list of mappings defines the relation between the
fields of the two views and allows traversing the association. A mapping
is defined by a pair of expressions, each one expressed over the fields
of one of the views.

Optionally, you can mark the association as a *Referential constraint*
and set a *Role precondition*. When you define an association as a
referential constraint, the association is considered a foreign key
constraint.

For example, the ``customer`` view can be linked with the ``order`` view
with cardinality \*. This means that every customer is related with zero
or more orders.

In a way, an association is like a join view because it links two views.

At runtime, users can navigate through associations using navigational
queries (``SELECT_NAVIGATIONAL``) or the RESTful Web service. See more about navigational queries in the section :doc:`Navigational Queries </vdp/vql/restful_architecture/restful_architecture>` of the VQL Guide.
