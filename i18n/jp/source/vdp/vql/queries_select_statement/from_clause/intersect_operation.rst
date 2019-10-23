===================
INTERSECT Operation
===================

The intersect operation returns the common elements of the result of two
or more input queries.

The syntax of this operation is the following:



.. code-block:: bnf
   :caption: Syntax of the INTERSECT operation
   :name: Syntax of the INTERSECT operation

   <query 1> INTERSECT <query 2> [ INTERSECT <query N> INTERSECT <query
   N+1> ]


For example, if we have three views with the following contents:

.. TODO: put these tables in the same row.

+-----------------------+
| view_a                | 
+===========+===========+
| **a**     | **b**     |    
+-----------+-----------+
| 1         | x         |     
+-----------+-----------+
| 2         | b         |    
+-----------+-----------+
| 4         | d         |     
+-----------+-----------+

+-----------------------+
| view_b                | 
+===========+===========+
| **col_1** | **col_2** |    
+-----------+-----------+
| 1         | a         |     
+-----------+-----------+
| 2         | z         |    
+-----------+-----------+
| 6         | f         |     
+-----------+-----------+

+-----------------------+
| view_c                | 
+===========+===========+
| **col_a** | **col_b** |    
+-----------+-----------+
| 1         | a         |     
+-----------+-----------+
| 2         | b         |    
+-----------+-----------+
| 5         | e         |     
+-----------+-----------+


And we execute this query

.. code-block:: sql

   SELECT *
   FROM (
       SELECT a
       FROM view_a
       
       INTERSECT
       
       SELECT col_1 AS a
       FROM view_b
       
       INTERSECT
       
       SELECT col_a AS a
       FROM view_c
       )


The result will be the following:

+---------+
| <query> |
+=========+
| **a**   |
+---------+
| 1       |
+---------+
| 2       |
+---------+

The result of this query is the common rows of the result of the three
queries. Note that the intersection operation takes into account the
result of each query, not the contents of each view.

The queries that participate in an ``INTERSECT`` query must adhere to
the following rules:

-  All the input queries must return the same schema. That is, the
   result of all the input queries must have the same number of fields
   with the same name.
-  By using brackets (``(`` and ``)``) you can indicate the
   order in which the intersections are performed. This order does not
   affect the results of the query, but can have a great impact over its
   performance.
   
   E.g. if the query is like this:
   ``<query 1> INTERSECT <query 2> INTERSECT <query 3>`` the Server
   performs the intersection between ``<query 1>`` and ``<query 2>`` and
   then, between this intermediate result and ``<query 3>``.
   
   If the query is like this:
   ``<query 1> INTERSECT (<query 2> INTERSECT <query 3>)`` the Server
   performs the intersection between ``<query 2>`` and ``<query 3>`` and
   then, between this intermediate result and ``<query 1>``.
