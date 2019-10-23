==================
Three-valued Logic
==================

Virtual DataPort is fully conformant with the three-valued logic defined
in the SQL standard.

The three-valued logic defines that a boolean expression can return one of these values:

1. *True*
#. *False*
#. *Unknown*, which is represented with the ``NULL`` value.

Consider the following rules when dealing with ``NULL`` values:

-  A comparison with a ``NULL`` value returns *unknown*.
-  A comparison with an *unknown* value returns *unknown*. That means that a function applied over a function that returns *unknown*, also returns *unknown*.
-  Conditions with *unknown* always returns false. That means that in queries with a WHERE and/or HAVING clause, only the rows for which the condition returns *true* are added to the result.
-  To evaluate if a value is or is not ``NULL``, use the operators ``is null`` or ``is not null`` respectively. Do not use ``= null`` nor ``<> null`` because these will never return ``true``.

.. rubric:: Examples

.. rubric:: Example 1

Let us say we execute this query:

.. code-block:: sql

   SELECT *
   FROM view_1 
   WHERE NOT (a = b)

Let us say that in one of the rows of "view_1", ``a`` is ``NULL`` and ``b`` is ``1``. For this row, the result of ``(a = b)``
is *unknown* because ``a`` is ``NULL``. The result of evaluating NOT
(*unknown*) is also *unknown*. Therefore, this row is not added to the
result of the query.

.. rubric:: Example 2

.. code-block:: sql

   SELECT * 
   FROM employee INNER JOIN department
   ON employee.dept_id = department.id
   
This query does not return the employees whose column "dept_id" is NULL because a comparison with NULL returns unknown.

.. rubric:: Example 3

Query to obtain all the customers whose state is not registered.

.. code-block:: sql

   SELECT *
   FROM customer
   WHERE state is null

Note the use of the operator ``is null``.

.. rubric:: Example 4
   
Query to obtain all the customers whose state is registered.

.. code-block:: sql

   SELECT *
   FROM customer
   WHERE state is not null

Note the use of the operator ``is not null``.


.. rubric:: Example 5
   
Example of query that does not return what you may expect.

.. code-block:: sql

   SELECT *
   FROM customer
   WHERE state <> null

This query will always return 0 rows. The reason is that a comparison with a "null" value is never evaluated to ``true``.