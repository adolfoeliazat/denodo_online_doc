==============
GET_VIEWS
==============

.. rubric:: Description

The stored procedure ``GET_VIEWS`` returns information about views. You can filter the result by several parameters: view name, view type,
etc. Each row represents a view.

If you want to obtain information any type of element, not just views, use the procedure :ref:`GET_ELEMENTS`.

.. rubric:: Syntax

.. code-block:: bnf

   GET_VIEWS (
         input_database_name : text
       , input_name : text 
       , input_user_creator : text
       , input_last_user_modifier : text
       , input_init_create_date : date 
       , input_end_create_date : date 
       , input_init_last_modification_date : date 
       , input_end_last_modification_date : date 
       , <input_view_type>
       , <input_swap_active>
       , <input_cache_status>
       , input_description : text
   )
   
   <input_view_type> ::=
         0  // Base views
       | 1  // Derived views
       | 2  // Interface views
       | 3  // Materialized tables
   
   <input_swap_active> ::=
         0  // Swapping status is "Default"
       | 1  // Swapping status is "On"
       | 2  // Swapping status is "Off"
  
   <input_cache_status> ::=
         0  // Cache mode is "Off"
       | 1  // Cache mode is "Partial exact"
       | 2  // Cache mode is "Partial"
       | 3  // Cache mode is "Full"
       | 4  // Cache mode is "Partial exact preload"
       | 5  // Cache mode is "Partial preload"

-  If you invoke the procedure using ``CALL`` and do not want to filter by a parameter, pass ``null``.

-  The procedure evaluates the input parameters with AND conditions. E.g. if you pass a value to ``input_database``, ``input_name`` and ``input_description``, the procedure will search return views of this database *and* this name *and* that contains this description.

-  The procedure evaluates the following parameters with the operator LIKE instead of equals:

   -  input_name
   -  input_user_creator
   -  input_last_user_modifier
   -  input_description
   
   This means that in the value of these parameters, you can use the wildcard operators you use with LIKE (``%`` and ``_``).
   The search by the description is case insensitive.

-  When providing a value for ``input_init_create_date`` and
   ``input_end_create_date``, or ``input_init_last_modification_date``
   and ``input_end_last_modification_date``, the procedure returns the
   views between the two intervals. I.e.: when you provide a value
   for ``input_init_create_date`` and ``input_end_create_date``, the procedure
   returns the views created during these two dates.
   
   If ``input_init_create_date`` is ``null``, the procedure returns all the
   views that were created before ``input_end_create_date``.
   
   If ``input_end_create_date`` is ``null``, the procedure returns all the
   views that were created after ``input_init_create_date``.
   Searching by ``input_init_last_modification_date`` and
   ``input_end_last_modification_date`` works in the same way.

|

The procedure returns these fields:

-  ``database_name``: name of database that the view belongs to.

-  ``name``: name of the view.

-  ``type``: the value is always "view".

-  ``user_creator``: owner of the view.

-  ``last_user_modifier``: user that modified the view for the last time. If the view was never modified, the value is the same as ``user_creator``.

-  ``create_date``: date when the view was created. 

-  ``last_modification_date``: date when the view was modified for the last time. If the view was never modified, the value is the same as ``create_date``

-  ``description``: description of the view.

-  ``view_type``: the possible values are:
   
   -  ``0``: if it is a base view
   -  ``1``: if it is a derived view
   -  ``2``: if it is an interface view
   -  ``3``: if it is a materialized table

-  ``swap_active``: the possible values are:
   
   -  ``0``: if the swapping status is "Default"
   -  ``1``: if the swapping status is "On"
   -  ``2``: if the swapping status is "Off"


-  ``cache_status``: the possible values are:
   
   -  ``0``: if the cache mode is "Off"
   -  ``1``: if the cache mode is "Partial exact"
   -  ``2``: if the cache mode is "Partial"
   -  ``3``: if the cache mode is "Full"
   -  ``4``: if the cache mode is "Partial exact preload"
   -  ``5``: if the cache mode is "Partial preload"

-  ``folder``: folder of the view in lowercase. If the view is not in any folder, the value is ``/``.

.. rubric:: Privileges Required

The results of this procedure change depending on the privileges granted to the user that runs it. If the user is not an administrator user consider that this procedure only returns information about the procedures on which the
user has the Metadata privilege. The implications of this are the following:

-  If the user is an administrator,
   the procedure can return information about all the views.
-  If the user is an administrator of one or more databases, the procedure can return information about all the views of that database.
-  The procedure will return information about all the views over which
   the user has the Metadata privilege.

.. rubric:: Examples

.. rubric:: Example 1

.. code-block:: sql

   SELECT * 
   FROM GET_VIEWS()
   WHERE input_database_name = 'customer_report' AND folder = '/base views'

Obtains all the views of the database "customer_report" inside a particular folder. Note that ``folder`` is not an input parameter of the procedure. Therefore, the execution engine executes the procedure passing the parameter ``input_database_name``. The result is the information about all the views in that database. Then, the execution engine filters this result to return only the folders whose name is "base views". 

.. rubric:: Example 2

.. code-block:: vql

   SELECT view_info.database_name, name, view_info.type as view_type, user_creator AS owner, index_name, view_index.type AS index_type, ordinal_position, column_name, asc_or_desc, filter_condition
   FROM GET_VIEWS() AS view_info
   LEFT OUTER JOIN GET_VIEW_INDEXES() AS view_index ON view_info.database_name = view_index.input_database_name
	   AND view_info.name = view_index.input_view_name
   WHERE view_info.input_database_name = 'customer360';

This query returns all the views and its indexes (if any) of the database "customer360".
The query has a ``LEFT OUTER JOIN`` of "GET_VIEWS" and "GET_VIEW_INDEXES" so 
views that do not have an index are added to the result as well.

.. rubric:: Example 3

.. code-block:: sql

   SELECT * 
   FROM GET_VIEWS()
   WHERE 
       input_database_name = 'customer_report' 
   AND (input_init_create_date = ADDDAY(CURRENT_DATE, -1));

Obtains all the views of the database "customer_report" created since yesterday at 12 AM. 

.. rubric:: Example 4

.. code-block:: sql

   SELECT * 
   FROM GET_VIEWS()
   WHERE input_description = '%report%';
   
This query returns all the views whose description contains the word "report".

