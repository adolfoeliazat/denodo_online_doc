============================================================
Adding Variables to Selection Conditions (GETVAR and SETVAR)
============================================================

There are situations where we want to create an aggregation view with a
condition in it. That is, creating a view with a ``WHERE`` condition and
a ``GROUP BY``. The limitation of this is that the ``WHERE`` condition
is static and cannot be changed at runtime.

For example, if we have two views:

#. A base view CLIENT with these fields: name, income and state.
#. And a view ``WEALTHY_CLIENT_BY_STATE`` defined as:

   .. code-block:: sql
   
      CREATE VIEW WEALTHY_CLIENT_BY_STATE AS 
      SELECT state, COUNT(*)   
      FROM client 
      WHERE income > 1000000  
      GROUP BY state

There is a limitation in the second view: the limit of income to
consider a client wealthy is static. So, we have to know this limit
before creating the view. If we wanted to change this limit at runtime,
we could remove the ``WHERE`` condition and add the field ``income`` to
the ``GROUP BY`` fields. But then, we would be grouping by this field
and we might not want to do that. Besides, if ``income`` is not in the
output of the base view you cannot add ``income`` to the ``GROUP BY``.

To avoid this problem, you can use the function ``GETVAR`` in the
definition of the query. The syntax of this function is



.. code-block:: bnf
   :caption: Syntax of the function GETVAR
   :name: Syntax of the function GETVAR

   GETVAR('<name of the variable>', '<type of the variable>', '<default value>')


``GETVAR`` tries to obtain the value of the variable
``<name of the variable>`` from the ``CONTEXT`` of the query. If it does
not find it, it returns ``<default value>``.

.. important:: Always try to use view parameters instead of the
   functions ``GETVAR`` and ``SETVAR``. These functions cannot be pushed
   down to any source, which may worsen the performance of the queries to
   this view. On the other hand, there are scenarios where using these
   functions is much easier than using parameters. For example, use them
   when you want to use a value in many conditions of the views’ hierarchy
   and the performance of the queries to this view is not a problem.

   The section :ref:`Parameters of Derived Views` of the Administration Guide
   explains what view parameters are.

For example, you could define the view ``WEALTHY_CLIENT_BY_STATE`` like
this:

.. code-block:: vql
   :caption: Definition of a view with a variable in the selection condition (GETVAR) 
   :name:  Definition of a view with a variable in the selection condition (GETVAR)

   CREATE VIEW WEALTHY_CLIENT_BY_STATE AS
   SELECT state, COUNT(*)
   FROM client
   WHERE income >= GETVAR('_var_wealthy_client_income_limit', 'int',
   1000000)
   GROUP BY state

With this change, the limit of income is no longer static and we can
query the view defining this value at runtime:


.. code-block:: vql
   :caption: Invoking a view defined with a variable in the selection condition
   :name: Invoking a view defined with a variable in the selection condition

   SELECT * FROM WEALTHY_CLIENT_BY_STATE
   CONTEXT ('VAR _var_wealthy_client_income_limit' = '250000')


If we do not put a value for the variable in the ``CONTEXT`` of the
query, the value used in the selection condition is the
``<default value>`` of the ``GETVAR`` function: ``1000000``.

Another option is obtaining the value of a variable from another view at
runtime and putting this value in the ``CONTEXT`` with the function
``SETVAR``. The syntax of this function is:



.. code-block:: bnf
   :caption: Syntax of the function SETVAR
   :name: Syntax of the function SETVAR

   SETVAR('<name of the variable>', '<value of the variable')


E.g. we have a DF base view ``INCOME_LIMIT`` that returns one row with
the value that we want to use for the variable
``_var_wealthy_client_income_limit``.



.. code-block:: vql
   :caption: Invoking a view defining a variable in the selection condition
   :name: Invoking a view defining a variable in the selection condition

   SELECT WEALTHY_CLIENT_BY_STATE.*
   FROM
       (SELECT SETVAR('_var_wealthy_client_income_limit', limit)
       FROM INCOME_LIMIT WHERE type = 'wealthy')
   NESTED ORDERED JOIN
       WEALTHY_CLIENT_BY_STATE;


We execute a ``NESTED JOIN`` between the two views because in this type
of join, the left branch is executed first. That means that the Server
queries the view ``INCOME_LIMIT`` first and the function ``SETVAR`` puts
the value of the variable in the ``CONTEXT``. Then, when the right
branch is executed, ``GETVAR`` will find the value of the variable
``_var_wealthy_client_income_limit`` in the ``CONTEXT``.

.. note:: If the query of the “left side” branch of the join returns
   more than one row, the ``SETVAR`` function will only take into account
   the value of the field of the first row.

.. important:: The cache engine does not deal with variables. Therefore, you must not
   use them in queries that involve any view whose cache is enabled.
