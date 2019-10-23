===========
WITH Clause
===========

The ``WITH`` clause specifies a common table expression (CTE). A CTE is
like a temporary result set that is defined and used for the duration of
a SQL statement.

The main advantage of the CTEs is that they improve the readability and
maintenance of complex queries. The query can be divided into separate
“building blocks”, which can then be used to build more complex queries.

Virtual DataPort supports Common Table Expressions in ``SELECT`` and
``CREATE VIEW`` statements.

For example, the following statement finds the department with the
lowest total pay.

.. code-block:: sql

   WITH department_salary (deptno, totalpay) AS
       (SELECT deptno, SUM(salary)
        FROM EMP
        GROUP BY deptno)
   SELECT deptno
   FROM department_salary
   WHERE totalpay = (
       SELECT max(totalpay) 
       FROM department_salary
   )

