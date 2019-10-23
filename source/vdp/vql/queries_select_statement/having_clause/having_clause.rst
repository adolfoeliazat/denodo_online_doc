=============
HAVING Clause
=============

The ``HAVING`` clause specifies filtering conditions on the results
returned by a query using the ``GROUP BY`` clause.

For example, continuing with the example from the previous section, to
obtain only the data corresponding to departments with more than 10
employees, the following query could be made:

.. code-block:: sql

   SELECT COUNT(*) AS numOfWorkers, department 
   FROM emp
   GROUP BY department
   HAVING COUNT(*) > 10