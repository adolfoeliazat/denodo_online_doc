=============================
GET_VIEW_COLUMNS
=============================

The stored procedure ``GET_VIEW_COLUMNS`` returns information
about the fields of the views.
This information includes the type of field, length, precision in case of
numbers, etc.

The procedure returns a row for each field of each view.

You can filter by view and/or database.

This procedure is very similar to :ref:`CATALOG_METADATA_VIEWS` and internally works similarly. The only difference is that they return different fields and the parameter "input_view_name" of "GET_VIEW_COLUMNS" is evaluated with the LIKE operator.

.. rubric:: Syntax

.. code-block:: bnf

   GET_VIEW_COLUMNS (
         <input_database_name : text>
       , <input_view_name : text> )

-  ``input_database_name``: name of the database.
-  ``input_view_name``: name of the view you want to obtain its fields.

-  The procedure evaluates the parameter ``input_view_name`` with the operator LIKE instead of equals. This means that in the value of this parameter, you can use the wildcard operators you use with LIKE (``%`` and ``_``).

- If ``input_database_name`` and ``input_view_name`` are ``null``, the procedure returns the fields of all the views of all the databases.

- If ``input_view_name`` is ``null``, the procedure returns the fields of all the views of the ``input_database_name``.

|

The procedure returns these fields:


-  ``database_name``: name of the database that the view of the field
   belongs to.

-  ``view_name``: name of the view.

-  ``column_name``: name of the view's field.

-  ``column_vdp_type``: name of the type in Virtual DataPort: int, text, float, etc.

-  ``column_sql_type``: name of the type of the field in the “source type
   properties” of the field. E.g. ``VARCHAR``, ``NCHAR``, ``INTEGER``,
   ``BIGINT``, etc.

   These are the names of the constants defined in the class
   `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_ of the JDBC API.
   
-  ``column_sql_type_code``: integer that represents the type of the field in the
   “source type properties” of the field.

   The values of this field are defined in the class `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_ of
   the JDBC API. E.g. ``INT`` = ``4``, ``VARCHAR`` = ``12``, etc.

-  ``column_size``: its meaning depends on the type of the field:

   -  For fields of type ``text``, it indicates the maximum length of the
      field.
   -  For numeric types, the maximum precision. For values without decimals (``int``
      and ``long``), this number is ``0``.


-  ``column_decimals``: for fields of type ``decimal``, is the value of "Type decimals" of the “source type properties” of the field. 0, for other types of fields.

-  ``column_radix``: number of fractional digits that a value of this
   field could store.

-  ``column_is_primary_key``: true if this field is one of the fields of the primary key of the view; false otherwise.

-  ``column_is_nullable``: true if the value of this field can be ``null``; false otherwise. 


-  ``column_remarks``: description of the field. If the field does not
   have a description, the value is ``null``.


.. rubric:: Privileges Required

The results of this procedure change depending on the privileges granted to the user that runs it. If the user is not an administrator user consider that this procedure only returns information about the procedures on which the
user has the Metadata privilege. The implications of this are the following:

-  If the user is an administrator, the procedure will return
   information about all the views of all the databases.
-  The procedure will return information about the views of the
   databases on which the user is a local administrator.
-  The procedure will return information about the views of the
   databases on which the user has Connect and Metadata privileges.

Note that this procedure does not return a “privileges error”. For
example, let us say that:

-  A user executes ``SELECT * FROM GET_PROCEDURE_COLUMNS()``
   (i.e. obtain information about all the fields of all the views of all
   databases)
-  This user only has Connect and Metadata privileges on the database
   ``testing``.

In this scenario, the procedure only returns information about the views
of the ``testing`` database and not about the views of the other
databases.

.. rubric:: Example

.. code-block:: sql
   
   SELECT view_name, column_name, column_vdp_type, column_sql_type, column_is_primary_key
   FROM GET_VIEW_COLUMNS()
   WHERE input_database_name = 'chinook' 
       AND input_view_name = '%invoice%'

The result is:

.. csv-table:: 
   :header: "view_name", "column_name", "column_vdp_type", "column_sql_type", "column_is_primary_key"

   "invoiceline", "InvoiceLineId", "int", "INTEGER", "true"
   "invoiceline", "InvoiceId", "int", "INTEGER", "false"
   "invoiceline", "TrackId", "int", "INTEGER", "false"
   "invoiceline", "UnitPrice", "decimal", "DECIMAL", "false"
   "invoiceline", "Quantity", "int", "INTEGER", "false"
   "invoice", "InvoiceId", "int", "INTEGER", "true"
   "invoice", "CustomerId", "int", "INTEGER", "false"
   "invoice", "InvoiceDate", "date", "TIMESTAMP", "false"
   "invoice", "BillingAddress", "text", "VARCHAR", "false"
   "invoice", "BillingCity", "text", "VARCHAR", "false"
   "invoice", "BillingState", "text", "VARCHAR", "false"
   "invoice", "BillingCountry", "text", "VARCHAR", "false"
   "invoice", "BillingPostalCode", "text", "VARCHAR", "false"
   "invoice", "Total", "decimal", "DECIMAL", "false"
   
The result includes the fields of the views "invoiceline" and "invoice" because the input parameter "input_view_name" uses the wildcard "%".