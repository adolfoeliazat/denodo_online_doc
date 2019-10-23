=======================
Avoiding SQL Injections
=======================

A SQL injection attack consists on running a non-authorized SQL query by
inserting it as an input value on a client application.

The guidelines to avoid this type of attacks to Virtual DataPort are the
following:


#. On the client applications that use the Denodo JDBC driver, avoid that
   the queries sent to Virtual DataPort include an injected SQL query. To
   achieve this, follow the best practices of the JDBC API to avoid this
   problem. More specifically, generate all the queries using prepared
   statements with parameters (e.g.
   ``SELECT id, name FROM view WHERE field = ?``).

   The methods of the JDBC API to set the value of the parameters prevent
   that malicious queries can be inserted into the query sent to Virtual
   DataPort.


#. You could be at risk of a SQL injection if there is a JDBC base view
   built from a SQL query that has interpolation variables (e.g.
   ``SELECT... FROM ... WHERE field = @INPUT``) and one of these conditions
   is met:

   a. At least one interpolation variable is not surrounded by simple
      quotes.
   b. Or, at least one of the fields associated to an interpolation
      variable was marked as a “SQL fragment” when the view was created. A
      field is interpreted as a SQL fragment if in the
      ``CREATE WRAPPER JDBC`` statement of the base view, the field has
      the modifier ``SQLFRAGMENT``.

   In this case, to avoid a SQL injection, make sure that the views used by
   external clients do not project the fields associated to these
   interpolation variables.
