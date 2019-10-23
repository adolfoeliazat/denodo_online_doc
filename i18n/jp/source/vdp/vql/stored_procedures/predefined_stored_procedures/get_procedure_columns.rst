=============================
GET_PROCEDURE_COLUMNS
=============================

.. rubric:: Description

The stored procedure ``GET_PROCEDURE_COLUMNS`` returns information
about the fields of the stored procedures created by the user. It does not return information about the out-of-the-box procedures.

The procedure returns a row for each field of each procedure.

You can filter by procedure and/or database.

.. rubric:: Syntax

.. code-block:: bnf

   GET_PROCEDURE_COLUMNS (
         input_database_name : text
       , input_procedure_name : text
   )

-  ``input_database_name``: name of the database.
-  ``input_procedure_name``: name of the procedure you want to obtain its fields.

-  The procedure evaluates the parameter ``input_procedure_name`` with the operator LIKE instead of equals. This means that in the value of this parameter, you can use the wildcard operators you use with LIKE (``%`` and ``_``).

- If ``input_database_name`` and ``input_procedure_name`` are ``null``, the procedure returns the fields of all the procedures of all the databases.

- If ``input_procedure_name`` is ``null``, the procedure returns the fields of all the procedures of the ``input_database_name``.

|

The procedure returns these fields:


-  ``database_name``: name of the database that the procedure of the parameter
   belongs to.

-  ``procedure_name``: name of the procedure.

-  ``column_name``: name of the procedure’s parameter.

-  ``column_vdp_type``: name of the type in Virtual DataPort: int, text, float, etc.

-  ``column_sql_type``: name of the type of the parameter in the “source type
   properties” of the field. E.g. ``VARCHAR``, ``NCHAR``, ``INTEGER``,
   ``BIGINT``, etc.

   These are the names of the constants defined in the class
   `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_ of the JDBC API.
   
-  ``column_sql_type_code``: integer that represents the type of the parameter in the
   “source type properties” of the parameter.

   The values of this field are defined in the class `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_ of
   the JDBC API. E.g. ``INT`` = ``4``, ``VARCHAR`` = ``12``, etc.
   
-  ``column_type``: the possible values are:

   -  IN: input parameter.
   -  OUT: output parameter.
   -  INOUT: input and output parameter.
   
-  ``column_is_nullable``: true if the value of this field can be ``null``; false otherwise.  

.. rubric:: Privileges Required

The results of this procedure change depending on the privileges granted to the user that runs it. If the user is not an administrator user consider that this procedure only returns information about the procedures on which the
user has the Metadata privilege. The implications of this are the following:

-  If the user is an administrator, the procedure will return
   information about all the procedures of all the databases.
-  The procedure will return information about the procedures of the
   databases on which the user is a local administrator.
-  The procedure will return information about the procedures of the
   databases on which the user has Connect and Metadata privileges.

Note that this procedure does not return a “privileges error”. For
example, let us say that:

-  A user executes ``SELECT * FROM GET_PROCEDURE_COLUMNS()``
   (i.e. obtain information about all the fields of all the procedures of all
   databases)
-  This user only has Connect and Metadata privileges on the database
   ``testing``.

In this scenario, the procedure only returns information about the procedures
of the ``testing`` database and not about the procedures of the other
databases.
