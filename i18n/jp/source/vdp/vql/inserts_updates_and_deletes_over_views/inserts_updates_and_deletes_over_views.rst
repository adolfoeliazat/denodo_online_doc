=======================================
Inserts, Updates and Deletes Over Views
=======================================


.. toctree::
   :hidden:

   insert_statement/insert_statement.rst
   update_statement/update_statement.rst
   delete_statement/delete_statement.rst
   return_modified_rows/return_modified_rows.rst
   use_of_with_check_option/use_of_with_check_option.rst

The ``INSERT``, ``UPDATE`` and ``DELETE`` statements allow respectively
inserting, updating and deleting rows from a view. These statements
modify the data stored in the underlying data source.

However, as in any database management system, there are limitations on
the views that can be updated.

A base view created over one of the following types of data sources may
be updateable:

-  JDBC data sources
-  ODBC data sources
-  Salesforce data sources
-  Custom wrappers that were developed to be updateable

Base views created over a different type of data source are not
updateable.

JDBC, ODBC and Salesforce base views from query are not updateable.

Virtual DataPort decides if a view is updateable or not following the
rules of the SQL 92 standard. The main restrictions are:


-  The ``SELECT`` statement used in the definition of the view cannot have
   the clauses ``DISTINCT``, ``GROUP BY`` or ``HAVING``.

-  The ``FROM`` clause of the definition of the view can only refer to one
   view. Therefore, join, union, minus and intersect views are not
   updateable.

-  Flatten views are not updateable.

-  The value of the derived fields of views cannot be updated.

-  Views using aggregation functions are not updateable even if there is
   not ``GROUP BY`` clause.

-  Join views are not updateable.

-  A union view is updateable only if *all* the following conditions are
   met:

   a. The union view is a partitioned union (the section :ref:`Removing
      Redundant Branches of Queries (Partitioned Unions)` of the
      Administration Guide explains what a partitioned union is).
   b. The feature “automatic simplification of queries” is enabled.
   c. The ``WHERE`` condition of the ``UPDATE`` statement allows the
      optimizer to remove all the branches of the execution plan except
      one.
   d. And, the view left in place in the execution plan is updateable.

-  A view with input parameters is updateable if you provide the value of
   these parameters in the ``WHERE`` clause of the ``UPDATE`` and
   ``DELETE`` statements.

-  A derived view is not updateable if its underlying view is not
   updateable.

In addition to the restrictions imposed by the standard, there are
others:

-  Views involving a Denodo stored procedure are not updateable.
-  JDBC base views created over a stored procedure of a database are not updateable.
-  DF, JSON and XML base views are not updateable.
-  Although JDBC and ODBC base views are updateable, the base views
   created over a SQL query are not.
-  Although Salesforce base views are updateable, the base views created over
   a SOQL query are not.
-  Base views over custom wrappers are updateable if they implement the
   methods described in the :doc:`Developer Guide </vdp/developer/index>`.
