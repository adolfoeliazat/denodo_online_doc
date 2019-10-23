=============
GET_ELEMENTS
=============

.. rubric:: Description

The stored procedure ``GET_ELEMENTS`` returns information about any element: data sources, views, Web services, etc.
You can filter the result by several parameters: element name, type, etc.

Each row represents an element.

If you only want to obtain information about views, it is better if you use the procedure :ref:`GET_VIEWS` because it provides more information than this one.

.. rubric:: Syntax

.. code-block:: bnf

   GET_ELEMENTS (
         input_database_name : text
       , input_name : text
       , input_type : element type
       , input_user_creator : text
       , input_last_user_modifier : text
       , input_init_create_date : date
       , input_end_create_date : date
       , input_init_last_modification_date : date
       , input_end_last_modification_date : date
       , input_description : text
   )

   <element type> ::=
         NULL
       | 'Folders'
       | 'DataSources'
       | 'StoredProcedures'
       | 'Wrappers'
       | 'Views'
       | 'WebServices'
       | 'Widgets'
       | 'Associations'
       | 'JMSListeners'

-  If you invoke the procedure using ``CALL`` and do not want to filter by a parameter, pass ``null``.

-  The procedure evaluates the input parameters with AND conditions. E.g. if you pass a value to ``input_database_name``, ``input_name`` and ``input_description``, the procedure will search return elements of this database *and* this name *and* that contains this description.

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
   elements between the two intervals. I.e.: when you provide a value
   for ``input_init_create_date`` and ``input_end_create_date``, the procedure
   returns the elements created during these two dates.

   If ``input_init_create_date`` is ``null``, the procedure returns all the
   elements that were created before ``input_end_create_date``.

   If ``input_end_create_date`` is ``null``, the procedure returns all the
   elements that were created after ``input_init_create_date``.
   Searching by ``input_init_last_modification_date`` and
   ``input_end_last_modification_date`` works in the same way.

|

The procedure returns these fields:

-  ``database_name``: name of database where the element belongs to.
-  ``name``: name of the element.
-  ``type``: type of the element. The values can be

   -  ``association``
   -  ``datasource``
   -  ``folder``
   -  ``storedProcedure``
   -  ``type``
   -  ``view``
   -  ``webService``
   -  ``widget``
   -  ``wrapper``

-  ``subtype``:  subtype of the element or an empty string if the element does not have a subtype. Elements that have a subtype and what subtypes they can have:

   -  ``view``: ``base``, ``derived``, ``interface`` or ``materialized``
   -  ``datasource``: ``arn``, ``custom``, ``df``, ``essbase``, ``gs``, ``jdbc``, ``json``, ``ldap``, ``odbc``, ``olap``, ``salesforce``, ``sapbwbapi``, ``saperp``, ``ws`` or ``xml``
   -  ``wrapper``: ``arn``, ``custom``, ``df``, ``essbase``, ``gs``, ``html``, ``jdbc``, ``json``, ``ldap``, ``odbc``, ``olap``, ``salesforce``, ``sapbwbapi``, ``saperp``, ``ws`` or ``xml``
-  ``user_creator``: owner of the element.
-  ``last_user_modifier``: user that modified the element for the last time. If the element was never modified, the value is the same as ``user_creator``.
-  ``create_date``: date when the element was created.
-  ``last_modification_date``: date when the element was modified for the last time. If the element was never modified, the value is the same as ``create_date``
-  ``description``: description of the element.
-  ``folder``: folder of the element in lowercase. If the element is not in any folder, the value is ``/``.
-  ``base_view_type``: the wrapper subtype of a base view or ``null`` if the element it is not a base view. The subtypes that they can have are: ``arn``, ``custom``, ``df``, ``essbase``, ``gs``, ``html``, ``jdbc``, ``json``, ``ldap``, ``odbc``, ``olap``, ``salesforce``, ``sapbwbapi``, ``saperp``, ``ws`` or ``xml``.


.. rubric:: Privileges Required

The results of this procedure change depending on the privileges granted to the user that runs it. If the user is not an administrator user, consider the following:

-  If the parameter ``input_database_name`` is not ``null``, the procedure returns an error if the user does not have CONNECT privileges over this database.
-  The procedure will only return information about the elements over which the user has ``METADATA`` privileges.

.. rubric:: Examples

.. rubric:: Example 1

.. code-block:: sql

   SELECT *
   FROM GET_ELEMENTS()
   WHERE input_database_name = 'customer_report' AND folder = '/base views'

Obtains all the elements of the database "customer_report" inside a particular folder. Note that ``folder`` is not an input parameter of the procedure. Therefore, the execution engine executes the procedure passing the parameter ``input_database_name``. The result is the information about all the elements in that database. Then, the execution engine filters this result to return only the folders whose name is "base views".

.. rubric:: Example 2

.. code-block:: sql

   SELECT *
   FROM GET_ELEMENTS()
   WHERE
       input_database_name = 'customer_report'
   AND (input_init_create_date = ADDDAY(CURRENT_DATE, -1));

Obtains all the elements of the database "customer_report" created since yesterday at 12 AM.

.. rubric:: Example 3

.. code-block:: sql

   SELECT *
   FROM GET_ELEMENTS()
   WHERE input_type = 'views' AND input_description = '%report%';

This query returns all the views whose description contains the word "report".
