=====================
GET_TYPE_ATTRIBUTES
=====================

.. rubric:: Description

The stored procedure ``GET_TYPE_ATTRIBUTES`` returns information about the members of each type. Each row represents a member of a type.

Note that when you create a view with compound fields (registers and arrays), the administration tool automatically creates a type for the compound fields.

.. rubric:: Syntax

.. code-block:: bnf

   GET_TYPE_ATTRIBUTES (
         input_database_name : text
       , input_type_name : text
       , input_attribute_name : text
   )

-  If you invoke the procedure using ``CALL`` and do not want to filter by a parameter, pass ``null``.

-  The procedure evaluates the parameters ``input_type_name`` and ``input_attribute_name`` with the operator LIKE instead of equals. This means that in the value of these parameters, you can use the wildcard operators you use with LIKE (``%`` and ``_``).

-  If ``input_database_name`` and ``input_type_name`` are ``null``, the procedure returns all the members of all the types defined by a user.

-  If ``input_type_name`` is ``null``, the procedure returns all the members of all the types defined by a user on the database ``input_database_name``.

|

The procedure returns these fields:

-  ``database_name``: name of the database that the type belongs to.
-  ``type_name``: name of the type.
-  ``type``: it can be either ``register`` or ``array``.
-  ``attribute_name``: name of the member of the type.
-  ``attribute_vdp_type``: type of the member. It can be a "basic" type (int, float, text, etc.) or another compound type.\
-  ``attribute_sql_type``: integer that represents the type of the member according to the JDBC API, in the class `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_.
-  ``attribute_sql_type_code``: name of the type of the member according to the JDBC API, in the class `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_.
-  ``attribute_type_decimals``: for numeric types, this is the maximum number of decimals supported by this type. For other types, ``null``.
-  ``attribute_type_precision``: maximum number of fractional digits that a value of this type can store.

.. rubric:: Privileges Required

The results of this procedure change depending on the privileges granted to the user that runs it.

If the user is not an administrator user, consider the following:

-  If the parameter ``input_database_name`` is not ``null``, the procedure returns an error if the user does not have ``CONNECT`` and ``METADATA`` privileges over this database.
-  If the query does not pass a value to ``input_database_name``, the procedure will only return information about the members of the types created on the database over which the user has ``CONNECT`` and ``METADATA`` privileges.


.. rubric:: Example

.. code-block:: sql
   
   SELECT type_name, type, attribute_name, attribute_vdp_type, attribute_sql_type_code
   FROM GET_TYPE_ATTRIBUTES()
   WHERE input_database_name='chinook'

The result is:

.. csv-table:: 
   :header: "type_name", "type", "attribute_name", "attribute_vdp_type", "attribute_sql_type_code"
   
   "_register_text", "register", "value", "text", "12"
   "_register_ArtistId_Name", "register", "ArtistId", "int", "4"
   "_register_ArtistId_Name", "register", "Name", "text", "12"
   "_array_register_text", "array", "value", "text", "12"
   "_array_register_ArtistId_Name", "array", "ArtistId", "int", "4"
   "_array_register_ArtistId_Name", "array", "Name", "text", "12"
   "relation_link", "register", "rel", "text", "12"
   "relation_link", "register", "rel_db", "text", "12"
   "relation_link", "register", "title", "text", "12"
   "relation_link", "register", "href", "text", "12"