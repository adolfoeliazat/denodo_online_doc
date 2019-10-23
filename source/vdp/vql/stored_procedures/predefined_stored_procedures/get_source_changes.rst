=====================
GET_SOURCE_CHANGES
=====================

.. rubric:: Description

The stored procedure ``GET_SOURCE_CHANGES`` detects the differences between
the schema of a base view and its underlying data source.

For example, for a JDBC base view it detects the differences between the
schema of the table in the database and the schema of the base view.

.. rubric:: Syntax

.. code-block:: bnf

   GET_SOURCE_CHANGES (
         db_name : text
       , table_name : text 
   )

-  ``db_name``: name of the Denodo database to which ``table_name`` belongs. If ``null``, it looks for ``table_name`` in the current database.

-  ``table_name``: name of the base view.

The procedure returns a row for each field of the view and also a row
for each field that is present in the source, but not in the base view.

If a field is an array or a register, it also returns one row for each
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

-  ``modification``: the possible values of this field are:

   - "" (an empty string): the field has not changed.
   -  “New field”: when the row represents a field that has been added to
      the source.
   -  “Deleted field”: when the row represents a field that has been
      deleted from the source.
   -  “Type has changed”: the type of the field has changed.
   -  "Properties have changed": one or more "source type properties" of the field have changed.
   -  “Compound structure has changed”: one of the subfields of this field
      has changed.

-  ``level``: For first-level fields, the value is ``1``.


.. rubric:: Remarks

-  By default, this procedure returns an error if it does not find the source. For example, it returns an error if ``table_name`` is a JDBC base view and the table/view was deleted from the underlying database. Or if ``table_name`` is a JSON base view and the JSON file does not exist, etc.

   If you do not want the procedure to fail when the source does not exist, execute the following statement from the VQL Shell (you need to be an administrator):
   
   .. code-block:: vql
   
      SET 'com.denodo.vdb.contrib.storedprocedure.SourceChangesProcedure.errorsAsResults'='true'
      
   Restart the Virtual DataPort server to apply the change.
   
   With this change, the procedure will return only one row, instead of one per field, and the ``modification`` field will hold the error.

-  This procedure returns an error if ``table_name`` is not a base view.

-  This procedure returns an error if ``db_name`` or ``table_name`` do not exist. 

.. rubric:: Privileges Required

Only users that have the Metadata privilege on the base view and Execute privilege
on the data source can execute this procedure. This means, the following users can execute this
procedure:

-  Administrators or administrators of this database.
-  Users that have the Connect privilege on this database, Metadata privilege on the base view
   and Execute privilege on the data source.

.. rubric:: Example

Let us suppose that ``internet_inc`` is a JDBC base view of the database ``customer360``
and that the current schema of its source table in the source database has changed
since the base view was created:

-  The table has a new field ``CUSTOMER_ID``
-  The field ``SPECIFIC_FIELD1`` was removed from the table
-  The type of the field ``TTIME`` has changed to text
-  The type of the field ``SUMMARY`` is the same but its properties have changed

.. code-block:: vql

   SELECT field, type, old_type, modification, depth
   FROM GET_SOURCE_CHANGES()
   WHERE db_name = 'customer360'
       AND table_name = 'incident';

The result of invoking the previous query will be the following:

+----------------+----------------+----------------+----------------+----------------+
| field          | type           | old\_type      | modification   | depth          |
+================+================+================+================+================+
| IINC\_ID       | long           | long           |                | 1              |
+----------------+----------------+----------------+----------------+----------------+
| SUMMARY        | text           | text           | Properties have| 1              |
|                |                |                | changed        |                |
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
