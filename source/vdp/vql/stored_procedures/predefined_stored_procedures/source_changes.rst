===============
SOURCE_CHANGES
===============

.. note:: This stored procedure is deprecated and it may be removed in the next major version
   of the Denodo Platform. Use the procedure :ref:`GET_SOURCE_CHANGES` instead of this one because "GET_SOURCE_CHANGES" can search on any database, not just in the one you are connected to and it returns the same information.
   
   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

.. rubric:: Description

The stored procedure ``SOURCE_CHANGES`` detects the differences between
the schema of a base view and its underlying data source.

For example, for a JDBC base view it detects the differences between the
schema of the table in the database and the schema of the base view.

.. note::
    
   We recommend using the procedure :ref:`GET_SOURCE_CHANGES` instead of this one because it can search for changes on base views of any database. "SOURCE\_CHANGES" only works over base views of the current database.

.. rubric:: Syntax

.. code-block:: bnf

   SOURCE_CHANGES ( 
       table_name : text 
   )

-  ``table_name``: name of the base view. If the view does not exist
   or is not a derived view, the procedure returns an error.

The procedure returns a row for each field of the view and also a row
for each field that is present in the source, but not in the base view.

If a field is an array or a register, there will also be a row for each
one of its subfields.

The output schema of this procedure has the following fields:

-  ``field``: name of the field in the view. If the row represents a new
   field, it is the name of the field in the source.

-  ``type``: new type of the field. If the field has not changed, this
   value is the same as “old\_type”. If the field has been removed from the
   source, this value is empty.

-  ``old_type``: old type of the field. If the type of the field has not
   changed, this value is the same as “type”. If the row represents a new
   field, this value is empty.

-  ``modification``: If it is empty, the field has not changed. Otherwise,
   it has one of these values:

   -  “New field”: when the row represents a field that has been added to
      the source.
   -  “Deleted field”: when the row represents a field that has been
      deleted from the source.
   -  “Type has changed”: the type of the field has changed.
   -  “Compound structure has changed”: one of the subfields of this field
      has changed.

-  ``level``: For first-level fields, the value is ``1``.


.. rubric:: Remarks

-  By default, this procedure returns an error if it does not find the source. For example, it returns an error if ``table_name`` is a JDBC base view and the table/view was deleted from the database, if ``table_name`` is a JSON base view and the JSON file does not exist, etc.

   If you do not want the procedure to fail when the source does not exist, execute the following statement from the VQL Shell (you need to be an administrator):
   
   .. code-block:: vql
   
      SET 'com.denodo.vdb.contrib.storedprocedure.SourceChangesProcedure.errorsAsResults'='true'
      
   Restart the Virtual DataPort server to apply the change.
   
   With this change, the procedure will return only one row, instead of one per field, and the ``modification`` field will hold the error.

.. rubric:: Privileges Required

Only users that have the Metadata privilege on the base view and Execute privilege
on the data source can execute this procedure. This means, the following users can execute this
procedure:

-  Administrators or administrators of this database.
-  Users that have the Connect privilege on this database, Metadata privilege on the base view
   and Execute privilege on the data source.

.. rubric:: Example

Let us suppose that ``internet_inc`` is a JDBC base view and that the
current schema of its table in the database has changed since the base
view was created:

-  The table has a new field ``CUSTOMER_ID``
-  The field ``SPECIFIC_FIELD1`` was removed from the table
-  The type of the field ``TTIME`` has changed to text

.. code-block:: vql

   SELECT field, type, old_type, modification, depth
   FROM SOURCE_CHANGES()
   WHERE table_name = 'internet_inc';

The result of invoking the previous query will be the following:

+----------------+----------------+----------------+----------------+----------------+
| field          | type           | old\_type      | modification   | depth          |
+================+================+================+================+================+
| IINC\_ID       | long           | long           |                | 1              |
+----------------+----------------+----------------+----------------+----------------+
| SUMMARY        | text           | text           |                | 1              |
+----------------+----------------+----------------+----------------+----------------+
| TTIME          | text           | date           | Type has       | 1              |
|                |                |                | changed        |                |
+----------------+----------------+----------------+----------------+----------------+
| TAXID          | text           | text           |                | 1              |
+----------------+----------------+----------------+----------------+----------------+
| SPECIFIC\_FIEL |                | text           | Deleted Field  | 1              |
| D1             |                |                |                |                |
+----------------+----------------+----------------+----------------+----------------+
| CUSTOMER\_ID   | long           |                | New Field      | 1              |
+----------------+----------------+----------------+----------------+----------------+
