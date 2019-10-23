=============================================
Pushing Down GROUP BY Views Below UNION Views
=============================================

When you execute a GROUP BY over a union view, Denodo may be able to
push down the group by under the union view if you are projecting the
aggregation functions COUNT, MIN, MAX or SUM.

For example, let us say that you have defined a view ``order`` that is
the UNION of the views ``Orders_Spain`` and ``Orders_France``. The data
of these two views is obtained from two different databases.

This query:

.. code-block:: sql

   SELECT o.product_id, SUM (amount) AS total
       FROM order o
       GROUP BY o.product_id
    
Will be transformed into this one: 

.. code-block:: sql

   SELECT o.product_id, SUM (amount) AS total
            FROM
   (
       SELECT os.product_id, SUM (amount) AS total
       FROM orders_Spain os
       GROUP BY os._productid)
     UNION (
       SELECT of.product_id, SUM (amount) AS total
       FROM orders_France of
       GROUP BY of.id) o
   GROUP BY o.product_id

You can see that the GROUP BY is pushed down below the UNION. This will
allow to push down the GROUP BY operation to the database. This
transformation will usually result in a huge reduction in the number of
rows that Denodo has to retrieve from the underlying data sources.

This transformation is specially tailored to business intelligence
scenarios, which usually involve very big facts tables.
