============================================
Pushing Down GROUP BY Views Below JOIN Views
============================================

When you execute a GROUP BY over a join view, Denodo may be able to push
down the GROUP BY under the join view if these conditions are met:

#. The data of each branch of the join is obtained from different
   sources.
#. There is an association established between the two joined views.
   This association has to be a referential constraint and the condition
   mapping of the association is an “equals condition” between the
   fields of the primary key of one view and the fields of the foreign
   key of the other view.

If these conditions are met, a query like this one:

 

.. code-block:: sql

   SELECT p.id, p.name, s.date, SUM(amount)
   FROM
       product AS p 
   INNER JOIN 
       sale AS s 
   ON (p.id = s.product_id)
   GROUP BY p.id, p.name, s.date

Will be transformed into this one:

.. code-block:: sql

   SELECT p.id, p.name, sg.date 
   FROM
   ( 
       SELECT s.product_id, s.date, SUM(amount) amount
       FROM sale AS s
       GROUP BY s.product_id, s.date) sg
   INNER JOIN
       product AS p 
   ON (p.id = sg.product_id)
   )

You can see that the GROUP BY is pushed down below the JOIN. This will
allow to push down the GROUP BY operation to the database. This
transformation will usually result in a huge reduction in the number of
rows that Denodo has to retrieve from the sales view.

To be able to apply this transformation, first you have to create an
association between the views ``sale`` and ``product`` with this
configuration:

-  Map the fields ``sale.product_id`` with ``product.product_id``.
-  Mark the association as a “Referential constraint” to indicate that
   there is primary key-foreign key relationship between these views.
-  Mark the end point ``product`` as “Principal” (making the ``sales``
   endpoint as “Dependent”)
-  Set the multiplicity of the ``sale`` end point to \*, which means
   that for each product, there are zero, one, or more rows on the
   ``sale`` view with the ``product_id`` of that product.

This transformation is specially tailored to business intelligence
scenarios where there are usually very big facts tables joined with
relatively small dimensions tables.

.. important:: Virtual DataPort does not enforce referential integrity
   of the data. That is, that Virtual DataPort does not enforce that each
   row of the dependent view has a match in the principal view of the
   association. It is the responsibility of the source to keep this
   integrity and of the user to define the associations correctly.

   If the associations are defined incorrectly, the optimizer
   could apply the optimization described in this section when it should
   not be applied.
