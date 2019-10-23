==========================
GET_USER_DEFINED_TYPES
==========================

.. rubric:: Description

The stored procedure ``GET_USER_DEFINED_TYPES`` returns information about each type. Each row represents a type.

Note that when you create a view with compound fields (registers and arrays), the administration tool automatically creates a type for the compound fields.


If you want to obtain the structure of a specific type, use the procedure :ref:`GET_TYPE_ATTRIBUTES`.

.. rubric:: Syntax

.. code-block:: bnf

   GET_USER_DEFINED_TYPES (
         input_database_name : text
       , input_type_name : text
   )

-  If you invoke the procedure using ``CALL`` and do not want to filter by a parameter, pass ``null``.

-  The procedure evaluates the parameter ``input_type_name`` with the operator LIKE instead of equals. This means that in the value of this parameter, you can use the wildcard operators you use with LIKE (``%`` and ``_``).

-  If ``input_database_name`` and ``input_type_name`` are ``null``, the procedure returns all the types defined by a user.

-  If ``input_type_name`` is ``null``, the procedure returns all the types defined by a user on the database ``input_database_name``.

|

The procedure returns these fields:

-  ``database_name``: name of the database that the type belongs to.
-  ``type_name``: name of the type.
-  ``vdp_type``: it can be either ``register`` or ``array``.
-  ``sql_type``: name of the type of the type according to the JDBC API. It can be either ``STRUCT`` or ``ARRAY``.
-  ``sql_type_code``: integer that represents the type of type on the JDBC API. ``2002`` for registers, ``2003`` for arrays.

.. rubric:: Privileges Required

The results of this procedure change depending on the privileges granted to the user that runs it.

If the user is not an administrator user, consider the following:

-  If the parameter ``input_database_name`` is not ``null``, the procedure returns an error if the user does not have ``CONNECT`` and ``EXECUTE`` privileges over this database.
-  If the query does not pass a value to ``input_database_name``, the procedure will only return information about the types created on the database over which the user has ``CONNECT`` and ``EXECUTE`` privileges.

.. rubric:: Example

.. code-block:: sql
   
   SELECT database_name, type_name, vdp_type, sql_type, sql_type_code
   FROM GET_USER_DEFINED_TYPES()
   WHERE input_database_name='chinook'

The result is:

.. csv-table:: 
   :header: "database_name", "type_name", "vdp_type", "sql_type", "sql_type_code"
   
   "chinook", "_register_text", "register", "STRUCT", "2002"
   "chinook", "_register_ArtistId_Name", "register", "STRUCT", "2002"
   "chinook", "_array_register_text", "array", "ARRAY", "2003"
   "chinook", "_array_register_ArtistId_Name", "array", "ARRAY", "2003"
   "chinook", "relation_link", "register", "STRUCT", "2002"