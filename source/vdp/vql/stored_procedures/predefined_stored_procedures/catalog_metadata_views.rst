========================
CATALOG_METADATA_VIEWS
========================

.. rubric:: Description

The stored procedure ``CATALOG_METADATA_VIEWS`` returns information
of the fields of the views.
This information includes the type of the field, precision in case of
numbers, etc.

You can filter by view and/or database.

.. note:: We recommend using the procedure
   :ref:`CATALOG_VDP_METADATA_VIEWS` instead of this one because it provides
   more information. I.e. ``CATALOG_VDP_METADATA_VIEWS`` returns the
   description of the fields and an addition field ``column_vdp_type_name``
   that provides the name of type of the field.

.. rubric:: Syntax

.. code-block:: bnf

   CATALOG_METADATA_VIEWS ( 
         input_database_name : text
       , input_view_name : text
   )

-  ``input_database_name``: name of the database.
-  ``input_view_name``: name of the view you want to obtain its fields.

The procedure returns a row for each field of each view.

If ``input_database_name`` and ``input_view_name`` are ``null``, the procedure returns the
fields of all the views of all the databases.

If ``input_view_name`` is ``null``, the procedure returns the fields of all the
views of the ``input_database_name``.

The output schema has the following fields:

-  ``database_name``: name of the database that the view of the field
   belongs to.

-  ``view_name``: name of the view’s field.

-  ``view_type``: type of the view:

   -  ``0``: base view.
   -  ``1``: derived view.

-  ``column_name``: name of the field.

-  ``column_type_name``: name of the type of the field in the “source type
   properties” of the field. E.g. ``VARCHAR``, ``NCHAR``, ``INTEGER``,
   ``BIGINT``, etc.

   These are the names of the constants defined in the class
   `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_ of the JDBC API.
   
-  ``column_type``: number that represents the type of the field.

   The values of this field are defined in the class `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_ of the JDBC API. E.g. ``INT`` = ``4``, ``VARCHAR`` = ``12``, etc.
   
-  ``column_type_precision``: its meaning depends on the type of the field:

   -  For fields of type ``text``, it indicates the maximum length of the
      field.
   -  For numeric types, it indicates the precision. Therefore, for ``int``
      and ``long`` fields, this number is ``0``.

-  ``column_type_length``: maximum length in bytes of the values of this
   field.

-  ``column_type_scale``: number of fractional digits that a value of this
   field could store.

.. rubric:: Privileges Required

This procedure only returns information about the views on which the
user has the Metadata privilege. The implications of this are the following:

-  If the user is an administrator, the procedure will return
   information about all the views of all the databases.
-  The procedure will return information about the views of the
   databases on which the user is a local administrator.
-  The procedure will return information about the views of the
   databases on which the user has Connect and Metadata privileges.

Note that this procedure does not return a “privileges error”. For
example, let us say that:

-  A user executes ``SELECT * FROM CATALOG_METADATA_VIEWS()`` (i.e.
   obtain information about all the fields of all the views of all
   databases)
-  This user only has Connect and Metadata privileges on the database
   ``testing``.

In this scenario, the procedure only returns information about the views
of the ``testing`` database and not about the views of the other
databases.



.. rubric:: Example

.. code-block:: sql
   
   SELECT column_name, column_type_name, column_type_length
   FROM CATALOG_METADATA_VIEWS()
   WHERE input_database_name='customer360' and input_view_name = 'invoice';

The result is:

.. csv-table:: 
   :header: "column_name", "column_type_name", "column_type_length"

   "InvoiceId", "INTEGER", 11
   "CustomerId", "INTEGER", 11
   "InvoiceDate", "TIMESTAMP", 23
   "BillingAddress", "VARCHAR", 70
   "BillingCity", "VARCHAR", 40
   "BillingState", "VARCHAR", 40
   "BillingCountry", "VARCHAR", 40
   "BillingPostalCode", "VARCHAR", 10
   "Total", "DECIMAL", 12
