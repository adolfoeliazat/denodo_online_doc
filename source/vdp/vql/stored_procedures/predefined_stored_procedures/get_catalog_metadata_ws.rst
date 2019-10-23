============================
GET_CATALOG_METADATA_WS
============================

.. rubric:: Description

The stored procedure ``GET_CATALOG_METADATA_WS`` returns information about the web service created in Denodo.

.. rubric:: Syntax

.. code-block:: bnf

   GET_CATALOG_METADATA_WS (
         input_database_name : text
       , input_ws_name : text
   )

-  If ``input_database_name`` and ``input_ws_name`` are ``null``, the procedure returns information about all the web services of all the databases.

-  If ``input_ws_name`` is ``null``, the procedure returns information about all the web services of that database.

|

For REST web services, the procedure returns one row of each view.
For SOAP web services, the procedure returns one row for each input parameter and one for each output parameter, of each operation.

The procedure returns one row per each field of  these fields:

-  ``database_name``: name of database of the web service.
-  ``ws_name``: name of the web service.
-  ``operation_name``: for SOAP web services, the name of the operation. For REST web services, the name of the view.
-  ``ws_type``: type of the element. The values can be:

   -  ``REST``
   -  ``SOAP``
   -  ``<empty>`` (for web services created in Denodo 4.7 version or earlier)

-  ``column_name``: name of the field.
-  ``column_type_name``: type of the field.
-  ``input``: 

   -  For SOAP web services: ``true`` if it is an input parameter; ``false`` if it is an output parameter.
   
   -  For REST web services: ``true`` (default); ``false`` if the field was marked as "No searchable" with the option "Remove from input".

-  ``output``: 

   -  For SOAP web services: ``false`` if it is an input parameter; ``true`` if it is an output parameter.
   
   -  For REST web services: ``true`` (default); ``false`` if the field was marked as "Do not output" with the option "Remove from output".
-  ``mandatory``: ``true`` if it is a mandatory input parameter; ``false`` if it is not or is an output parameter.
-  ``operator``: for input parameters of SOAP web services, the operator of this parameter; ``null`` for other fields.
-  ``visibility``: the type of the operation. 

   -  For SOAP web services:
      
      -  ``1`` if the parameter belongs to a query operation.
      -  ``10`` if the parameter belongs to an INSERT operation.
      -  ``11`` if the parameter belongs to an UPDATE operation.
      -  ``12`` if the parameter belongs to a DELETE operation.

   -  For REST web services the value is always ``1``.
-  ``is_fetch``: ``true`` if this row represents an input parameter of a SOAP web service to indicate the number of rows to fetch. This is a field added with the option "Add pagination" of the administration tool. ``false`` otherwise.
-  ``is_offset``: ``true`` if this row represents an input parameter of a SOAP web service to indicate the number of rows to skip of the result set. This is a field added with the option "Add pagination" of the administration tool. ``false`` otherwise.
-  ``is_order_by``: ``true`` if this row represents an input parameter of a SOAP web service to order the results returned by the service. This is a field added with the option "Add order by" of the administration tool. ``false`` otherwise.
-  ``schema_database``: name of database where the element published is located.
-  ``schema_name``: name of the view published. For SOAP web services, this can also be the name of a Denodo stored procedure.
-  ``schema_type``: type of the element published. Possible values:

   -  ``view``
   -  ``storedProcedure`` (only for SOAP web services)

.. csantos@2018/03/14: theoretically, the value can also be wrapper or query. However, publishing them is deprecated.

.. rubric:: Remarks

The procedure returns no results if ``input_database_name`` or ``input_ws_name`` do not exist.

.. rubric:: Privileges Required

The results of this procedure changes depending on the privileges granted to the user that runs it. 

-  Administrators and users with the role ``serveradmin`` can obtain information about all the web services.
-  If the user is an administrator of a database, the procedure will return information about all the services of that database.
-  The procedure will only return information about the web Services over which the user has METADATA privileges.

The procedure never fails because the user lacks the required privileges.

.. rubric:: Examples

.. rubric:: Example 1

.. code-block:: sql

   SELECT DISTINCT database_name, ws_name
   FROM GET_CATALOG_METADATA_WS()
   WHERE ws_type = 'SOAP';

Returns the SOAP web services of all the databases. As ``ws_type`` is not an input parameter of the procedure, the execution engine executes the procedure and then, filters out the services whose type is not "SOAP".

.. rubric:: Example 2

.. code-block:: sql

   SELECT *
   FROM GET_CATALOG_METADATA_WS('customer_report', 'customer_ws');

Obtains information of the web service "customer_ws" of the database "customer_report". If the user does not have the privilege METADATA granted over this web service, the procedure does not return any rows. 

.. rubric:: Example 3

.. code-block:: sql

   SELECT DISTINCT database_name, ws_name, ws_type
   FROM GET_CATALOG_METADATA_WS()
   WHERE
      schema_database = 'customer_report' AND schema_name = 'customer' AND schema_type = 'view';

This query returns all web Services that publish the view "customer" of the database "customer_report".
