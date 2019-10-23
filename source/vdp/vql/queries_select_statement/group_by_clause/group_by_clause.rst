===============
Group BY Clause
===============

The ``GROUP BY`` clause allows grouping the results of a query by the
values of the attributes of the view and/or expressions, generating a
row for each group. The attributes and expressions with which the group
by operation is performed are specified in the ``GROUP BY`` clause.

If the query does not have the ``GROUP BY`` clause, but its ``SELECT``
clause uses aggregation functions, all the results obtained by the
``SELECT`` statement would form one single group.

When a query has the ``GROUP BY`` clause, the content of the ``SELECT``
clause is restricted: only the attributes specified in the ``GROUP BY``
clause can be specified in it. The other attributes of the view can be
used only as parameters of aggregation functions. When an aggregation
function is specified in the ``SELECT`` clause, you should indicate and
alias for the new attribute. Otherwise the Server generates an alias
automatically.

In a group-by view, derived attribute functions can also appear in the
``SELECT`` clause, although only applied to aggregation fields or
functions.

Use of Aggregation Functions
============================

An aggregation function is applied to the tuples belonging to a group
resulting from a ``GROUP BY`` operation and calculates an aggregated
value from same. The aggregation functions that exist in Virtual
DataPort are enumerated in the appendix :ref:`Aggregation Functions`.

The aggregation functions follow the general syntax of the predefined
functions (see section :ref:`Syntax Conventions`), but only the name of the
attribute subject to alteration is admitted as a parameter (nested
functions are not admitted either).

The modifiers ``ALL`` and ``DISTINCT`` can also be specified.

One exception is the ``COUNT()`` aggregation function that can receive
as a parameter the special character ``*`` to indicate that it
should return the number of tuples that belong to each group.

For example, given a relation ``emp`` representing the employees of a
company that contains an attribute ``department`` which indicates to
which department each employee belongs, to obtain the different
departments together with the number of employees that belong to each
one of them, the following query would be executed:

.. code-block:: sql

   SELECT count(*) AS numOfWorkers, department 
   FROM emp 
   GROUP BY department;

Or, using the alias of the field:

.. code-block:: sql 

   SELECT count(*) AS numOfWorkers, department AS dept_name
   FROM emp 
   GROUP BY dept_name;
