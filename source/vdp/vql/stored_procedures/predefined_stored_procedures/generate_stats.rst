===============
GENERATE_STATS
===============

.. rubric:: Description

The stored procedure ``GENERATE_STATS`` gathers *and* stores the
statistics of all the fields of a view (minimum value, maximum value,
etc. of each field) and the number of rows of the view.

To gather these statistics, it executes a ``SELECT`` statement with
several aggregation functions, in order to obtain statistics like the
average size of each field, number of distinct values, etc.

If you want to gather *and* store the statistics of only some fields of
the view, see the section :ref:`GENERATE_STATS_FOR_FIELDS`.

.. rubric:: Syntax


.. code-block:: bnf

   GENERATE_STATS ( 
       viewname : text
   )

-  ``viewname``: the procedure will gather the statistics of the view
   with this name (with the same case)

.. rubric:: Privileges Required

Only users that have the Write privilege on the view can execute this
procedure. This means, the following users can execute this procedure:

-  Administrators or administrators of this database.
-  Users that have the Connect and Write privileges on this database.
-  Users that have the Connect privilege on this database and Write
   privilege on this view.

.. rubric:: Example

.. code-block:: vql

   CALL GENERATE_STATS('internet_inc')

