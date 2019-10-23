======================
CLEAN_CACHE_DATABASE
======================

.. rubric:: Description

This procedure launches the Cache Maintenance task.

The section :ref:`Cache Maintenance Task` of the Administration Guide
explains what this task does.

.. rubric:: Syntax

.. code-block:: bnf

   CLEAN_CACHE_DATABASE( 
       database_name : text
   )

-  ``database_name``: name of the database.

It returns the following fields:

-  view\_name
-  deleted\_tuples

In the first row, “view\_name” is an empty string and
“deleleted\_tuples” is the number of “query patterns” removed from the
table “vdb\_cache\_querypattern”.

The following rows contain the name of a view and the number of rows
that the procedure has removed from the table that holds the cached data
of this view.

This procedure does not return the names of views that have cache
enabled but none of their rows expired or were marked as invalid.

.. rubric:: Privileges Required

Only administrators can invoke this procedure.
