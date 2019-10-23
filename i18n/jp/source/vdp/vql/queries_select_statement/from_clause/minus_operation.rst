===============
MINUS Operation
===============

The minus operation returns all the rows returned by the first statement
except the rows that are also returned by the other queries involved in
the operation.

The syntax of this operation is the following:



.. code-block:: bnf
   :caption: Syntax of the MINUS operation
   :name: Syntax of the MINUS operation

   <query 1> MINUS <query 2> [ MINUS <query N> MINUS <query N+1> ]


For example, if we have three views with the following contents:

+-----------------------+
| View_A                | 
+===========+===========+
| **A**     | **B**     |    
+-----------+-----------+
| 1         | x         |     
+-----------+-----------+
| 2         | b         |    
+-----------+-----------+
| 4         | d         |     
+-----------+-----------+

+-----------------------+
| View_B                | 
+===========+===========+
| **Col_1** | **Col_2** |    
+-----------+-----------+
| 1         | a         |     
+-----------+-----------+
| 2         | z         |    
+-----------+-----------+
| 6         | f         |     
+-----------+-----------+


+-----------------------+
| View_B                | 
+===========+===========+
| **Col_1** | **Col_2** |    
+-----------+-----------+
| 1         | a         |     
+-----------+-----------+
| 2         | b         |    
+-----------+-----------+
| 5         | e         |     
+-----------+-----------+


And we execute this query

.. code-block:: sql

   SELECT * FROM 
     (  SELECT A FROM View_A 
      MINUS 
        SELECT Col_1 AS A 
        FROM View_B 
      MINUS 
        SELECT Col_A AS A FROM View_C)

The result will be the following:

+---------+
| <query> |
+=========+
| **A**   |
+---------+
| 4       |
+---------+

The result of this query is the result of the first query without the
rows returned by the other queries involved.

The queries that participate in a ``MINUS`` query must adhere to the
following rules:

-  All the input queries must return the same schema. That is, they must
   have the same number of fields with the same name.
-  By using brackets (``(`` and ``)``) you can indicate the
   order in which the minus operations are performed. Note that the view
   order may affect the results of the view.
   
   E.g. if the query is like this:
   ``<query 1> MINUS <query 2> MINUS <query 3>`` The Server
   performs the minus operations between ``<query 1>`` and ``<query 2>``
   and then, between this intermediate result and ``<query 3>``.
   
   If the query is like this:
   ``<query 3> MINUS <query 2> MINUS <query 1>`` the Server
   performs the minus operations between ``<query 3>`` and ``<query 2>``
   and then, between this intermediate result and ``<query 1>``.
   Although in both queries the queries involved are the same, the
   result may be different.
