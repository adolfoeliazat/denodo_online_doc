=================
CATALOG_ELEMENTS
=================

.. note:: This stored procedure is deprecated and it may be removed in the next major version
   of the Denodo Platform. Use the procedure :ref:`GET_ELEMENTS` instead of this one because "GET_ELEMENTS" can search on any database, not just in the one you are connected to and it returns the same information.
   
   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

.. rubric:: Description

The stored procedure ``CATALOG_ELEMENTS`` returns a list of the elements
(data sources, views, Web services, etc.) of the Virtual DataPort
database you are connected to. You can filter the result by several
parameters: element name, type, etc.

.. rubric:: Syntax

.. code-block:: bnf

   CATALOG_ELEMENTS (
         name : text 
       , type: 
           {   NULL
             | 'Folders'
             | 'DataSources'
             | 'StoredProcedures'
             | 'Wrappers'
             | 'Views'
             | 'WebServices'
             | 'Widgets'
             | 'Associations'
             | 'JMSListeners'
           }
       , usercreator : text
       , lastusermodifier : text
       , initcreatedate : date
       , endcreatedate : date
       , initlastmodificationdate : date
       , endlastmodificationdate : date
       , description : text
   )


-  If invoke this procedure with ``CALL`` and you do not want to filter by a parameter, pass ``null`` to it.

-  ``usercreator`` (optional): owner of the element.
-  When filtering by ``name``, ``usercreator`` and/or
   ``description``, the comparison is performed with the ``contains``
   operator. E.g.: if ``usercreator`` is ``adm``, the procedure
   will return all the elements which creator contains the string
   ``adm``.
-  When providing a value for ``initcreatedate`` and
   ``endcreatedate``, or ``initlastmodificationdate``
   and ``endlastmodificationdate``, the procedure returns the
   elements between these two intervals. I.e.: when you provide a value
   for ``initcreatedate`` and ``endcreatedate``, the procedure
   returns the elements created during these two dates.
   
   If ``initcreatedate`` is ``null``, the procedure returns all the
   elements that were created before ``endcreatedate``.
   
   If ``endcreatedate`` is ``null``, the procedure returns all the
   elements that were created after ``initcreatedate``.
   Searching by ``initlastmodificationdate`` and
   ``endlastmodificationdate`` works in the same way.

.. rubric:: Privileges Required

No privileges are required to execute this procedure.

.. rubric:: Examples

.. rubric:: Example 1

.. code-block:: sql

   SELECT resultName
       ,resultType
       ,resultSubtype
       ,resultUserCreator
       ,resultLastUserModifier
       ,resultCreateDate
       ,resultLastModificationDate
       ,resultDescription
   FROM CATALOG_ELEMENTS()
   WHERE type = 'WebServices'
       AND initCreateDate = TO_DATE('yyyy-MM-dd hh:mm:ss', '2017-01-01 00:00:00') 
       AND endCreateDate = TO_DATE('yyyy-MM-dd hh:mm:ss', '2017-12-31 23:59:59');
   
The procedure returns all the Web services that were created in the year
2017.
   
.. rubric:: Example 2

.. code-block:: vql

   SELECT * 
   FROM CATALOG_ELEMENTS()
   WHERE usercreator = 'jsmith';

Returns all the elements owned by the user "jsmith".