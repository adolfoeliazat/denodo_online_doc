====
DUAL
====

.. rubric:: Description

The stored procedure ``DUAL`` is a special procedure that only returns
one field called ``dummy`` of type ``text``. It does not have input
parameters and it returns an empty row.

Its main uses are:

-  Executing low-cost queries used as ping queries by external clients
   that access Virtual DataPort via the JDBC and ODBC interfaces.
-  Executing VQL functions.

.. rubric:: Syntax

.. code-block:: bnf

   Dual()

.. rubric:: Privileges Required

No privileges are required to execute this procedure.

.. rubric:: Examples

.. rubric:: Example 1

.. code-block:: sql

   SELECT NOW() 
   FROM DUAL()

This query returns the current time.

The function ``NOW()`` is described in the appendix :ref:`NOW`.

.. rubric:: Example 2

.. code-block:: sql

   SELECT 1 
   FROM DUAL()
   
This query is a very low-cost query that can be used as ping query by
external clients.
   
.. rubric:: Example 3

.. code-block:: sql

   SELECT 1

This query returns the same as the previous example. That is because the
Server assumes that a query that does not have a ``FROM`` clause is referencing the ``Dual()`` procedure.

