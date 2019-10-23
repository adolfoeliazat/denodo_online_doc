============================
GENERATE_STATS_FOR_FIELDS
============================

.. rubric:: Description

The stored procedure ``GENERATE_STATS_FOR_FIELDS`` gathers *and* stores
the statistics of the fields of a view (minimum value, maximum value,
etc. of each field) and the number of rows of the view. It works as the
procedure ``GENERATE_STATS`` (see section :ref:`GENERATE_STATS`), with the
difference that it only obtains the statistics of the fields you
indicate.

If possible, use the procedure ``GENERATE_SMART_STATS_FOR_FIELDS``,
which can query the system tables of the database to obtain these
statistics instead of executing a regular ``SELECT`` statement.

.. rubric:: Syntax

.. code-block:: bnf

   GENERATE_STATS_FOR_FIELDS( 
         viewname : text, 
       , fields : array
   )

-  ``viewname``: the procedure will gather the statistics of the view
   with this name (with the same case)
-  ``fields``: array of field names that the procedure will obtain
   its statistics for. When you specify only some fields of the view,
   the procedure does not delete the statistics of the other fields of
   the view that have already been gathered.
   
   If ``null``, the procedure gathers the statistics of all the fields
   of the view.
   
   If an empty array (i.e. ``{}``), the procedure gathers the number of
   rows of the view, but does not obtain the statistics of any field.

.. rubric:: Privileges Required

Only users that have the Write privilege on the view can execute this
procedure. This means, the following users can execute this procedure:

-  Administrators or administrators of this database.
-  Users that have the Connect and Write privileges on this database.
-  Users that have the Connect privilege on this database and Write
   privilege on this view.

.. rubric:: Examples

.. rubric:: Example 1

.. code-block:: vql

   CALL GENERATE_STATS_FOR_FIELDS (
       'internet_inc', 
       { ROW('iinc_id') , ROW('taxid'), ROW('summary') } )
   
This statement gathers and stores the statistics for the fields
``iinc_id``, ``taxid`` and ``summary`` of the view ``internet_inc``.

The second parameter is an array so you have to use the construct
``{ ROW(...), ROW(...),...}`` to define the array of field names. You
can see more details about this construct in the section :ref:`Conditions
with Compound Values`.

.. rubric:: Example 2

.. code-block:: vql

   CALL GENERATE_STATS_FOR_FIELDS ('internet_inc', { } )

This statement gathers and stores the number of rows returned by the
view, but does not obtain any statistics for the fields of the view.

