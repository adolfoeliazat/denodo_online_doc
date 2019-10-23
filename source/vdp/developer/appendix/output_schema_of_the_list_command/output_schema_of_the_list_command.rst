=================================
Output Schema of the LIST Command
=================================

This section lists the output of the of the ``LIST`` commands when
executed from a JDBC or an ODBC client.

.. note:: When these commands are executed from the “VQL Shell” of the
   Administration Tool, they return information that should only be used
   for debugging purposes and may change in the future.

The output schema of all the ``LIST`` commands (``LIST WRAPPERS``,
``LIST DATASOURCES`` …) has only one column: ``name``, except for the
commands

-  ``LIST JARS``: returns the columns: ``JAR_NAME``, ``FUNCTIONS_TYPE``,
   ``FUNCTIONS``. The values of the columns ``FUNCTIONS_TYPE``,
   ``FUNCTIONS`` are ``NULL`` if the jar does not contain a custom
   function. Otherwise, the output contains a row for each type of function
   of the jar:

   - ``FUNCTIONS_TYPE``: indicates the type of the function: ``CONDITION``
     or ``AGGREGATE``.
   - ``FUNCTIONS``: names of the functions contained in the jar.

-  ``LIST FUNCTIONS CUSTOM``: contains the columns ``NAME``, ``TYPE`` and
   ``SYNTAX``. The command returns a row for each signature of each
   function. The ``TYPE`` column indicates if the function is an
   aggregation function or a condition function.
