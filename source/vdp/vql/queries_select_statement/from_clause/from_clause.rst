===========
FROM Clause
===========


.. toctree::
   :hidden:

   join_operation.rst
   intersect_operation.rst
   minus_operation.rst
   flatten_view_flattening_data_structures.rst
   subqueries_in_the_where_clause_of_the_query.rst

Specification of the origin view is carried out using the ``FROM``
clause. In said clause the name of the relation - or relations - from
which data are to be extracted is indicated. It is possible to specify
aliases for the relations in the ``FROM`` clause. Aliases can be used in
the other clauses in the ``SELECT`` statement and will facilitate the
creation of Join conditions. If an alias is indicated for a relation in
the ``FROM`` clause, the name of the relation should not be used in the
rest of the ``SELECT`` statement to prefix fields of same; the alias
should always be used.

It is possible to use subqueries in the ``FROM`` clause. The subquery
must be included between brackets.

**Example** The following statement uses a subquery that carries out a
``UNION`` operation between the ``internet_inc`` and
``phone_inc`` views:

.. code-block:: sql

   SELECT * 
   FROM (
     SELECT * 
     FROM internet_inc 
     
     UNION 
     
     SELECT * 
     FROM phone_inc
   )
   WHERE taxid = 'B78596011'

If several relations are listed in the ``FROM`` clause without
separating them from the ``JOIN`` clause, then their cross product
will be performed. The following subsection deals with the different
join operations available.

The ``FROM`` clause may also contain calls to Denodo stored procedures.
The results returned by the calling up of a procedure will be dealt with
in this case like the tuples of a view. See section :doc:`/vdp/vql/stored_procedures/stored_procedures`
for more details.

.. note:: Virtual DataPort allows executing ``SELECT`` statements
   without the ``FROM`` clause. E.g. ``SELECT 2+2``

