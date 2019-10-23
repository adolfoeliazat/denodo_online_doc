=================
GET_ASSOCIATIONS
=================

.. rubric:: Description

The stored procedure ``GET_ASSOCIATIONS`` returns information about all the associations, or the associations in which a view is involved, or the associations published by a web service.

Each row represents an association.

.. rubric:: Syntax

.. code-block:: bnf

   GET_ASSOCIATIONS (
         input_database_name : text
       , input_name : text
       , input_type : element type       
   )
   
   <element type> ::=           
       | 'views'
       | 'webservices'       

-  ``input_database_name``: database of the view or web service for which you want to obtain its associations.
-  ``input_name``: name of the view or web service for which you want to obtain its associations. If ``null``, it does not filter by name.
-  ``input_type`` (mandatory): type of the element for which you want to obtain its associations.

   If ``webservices``, the procedure returns the associations published by a web service.
   
   If ``views``, the procedure returns the association in which a view is involved.

If ``input_type`` is ``webservices``, ``input_database_name`` and ``input_name`` are mandatory.

The procedure returns one row per associations, with these fields:

-  ``database_name``: name of database where the element belongs to.
-  ``name``: name of the element.
-  ``association_name``: name of the association. 
-  ``association_description``: description of the association. 
-  ``left_view_name``: name of the view of the left endpoint of the association.
-  ``left_view_database``: database name of the view of the left endpoint of the association.
-  ``left_role``: role of the view of the left endpoint of the association.
-  ``left_multiplicity``: multiplicity of the left endpoint of the association.
-  ``right_view_name``: name of the view of the right endpoint of the association.
-  ``right_view_database``: database name of the view of the right endpoint of the association.
-  ``right_role``: role of the view of the right endpoint of the association.
-  ``right_multiplicity``: multiplicity of the right endpoint of the association.
-  ``mappings``: comma-separated list of the condition mappings of the association.
-  ``valid``: true if it is a valid association. 


.. rubric:: Privileges Required

The procedure only returns information about an association if the user has the METADATA privilege over both endpoints of the association.

.. rubric:: Examples

.. rubric:: Example 1

.. code-block:: sql

    SELECT *
    FROM GET_ASSOCIATIONS()
    WHERE input_database_name = 'customer_report'
        AND name = 'invoice'
        AND input_type = 'views'

Obtains all the associations of the view "invoice" in the database "customer_report". 

.. rubric:: Example 2

.. code-block:: sql

   SELECT *
   FROM GET_ASSOCIATIONS()
   WHERE input_database_name = 'customer_report'
       AND input_type = 'views'


Obtains all the associations of the database "customer_report". 

.. rubric:: Example 3

.. code-block:: sql

   SELECT *
   FROM GET_ASSOCIATIONS()
   WHERE name = 'invoice'
       AND input_type = 'views'


Obtains all the associations of the views named "invoice" of any database. 

.. rubric:: Example 4

.. code-block:: sql

   SELECT *
   FROM GET_ASSOCIATIONS()
   WHERE input_type = 'views'  

Obtains all the associations of the server. 

.. rubric:: Example 5

.. code-block:: sql

   SELECT *
   FROM GET_ASSOCIATIONS()
   WHERE input_database_name = 'customer_report'
       AND name = 'invoice'
       AND input_type = 'webservices'


Obtains all the associations published by the web service "invoice" of the database "customer_report". It only returns information about REST web services (SOAP web services do not publish associations).