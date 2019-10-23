==============
CATALOG_VIEWS
==============

.. note:: This stored procedure is deprecated and it may be removed in future
   major versions of the Denodo Platform. Use the procedure :ref:`GET_VIEWS` instead of this one; "GET_VIEWS" can search on any database, not just in the one you are connected to and it returns the same information.

.. rubric:: Description

The stored procedure ``CATALOG_VIEWS`` returns a list of the base views
and derived views of the Virtual DataPort database you are connected to.
You can filter the result by several parameters: view name, view type,
etc.

.. rubric:: Syntax

.. code-block:: bnf

   CATALOG_VIEWS (
         name : text 
       , usercreator : text
       , lastusermodifier : text
       , initcreatedate : date
       , endcreatedate : date 
       , initlastmodificationdate : date
       , endlastmodificationdate : date
       , viewtype : { 0 | 1 | 2 | 3 }
       , swapactive : { 0 | 1 | 2 }
       , cachestatus : { 0 | 1 | 2 | 3 | 4 | 5 } 
       , description : text
   )

-  ``usercreator`` (optional): owner of the element.
-  If you do not want to filter by a parameter, pass ``null`` to it.
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
-  ``viewtype``: if not ``null``, the procedure returns the views of a
   certain type. The valid values are: 
   
   -  0: only return base views.
   -  1: only return derived views.
   -  2: only return interface views.
   -  3: only return materialized tables.

-  ``swapactive``: if not ``null``, the procedure returns the views
   with a certain swap status. The valid values are ``0`` (views with
   the swap status set to Default), ``1`` (swap set to On) and ``2``
   (swap set to Off).
-  ``cachestatus``: if not ``null``, the procedure returns the views
   with a certain cache mode. The valid values are ``0`` (cache mode is
   Off), ``1`` (Partial Exact), ``2`` (Partial), ``3`` (Full), ``4``
   (Partial Exact Preload) and ``5`` (Partial Preload).

.. rubric:: Privileges Required

This procedure only returns information about the views on which the
user has the Metadata privilege. The implications of this are the following:

-  If the user is an administrator or an administrator of the database,
   the procedure will return information about all the views.
-  The procedure will return information about all the views over which
   the user has the Metadata privilege.

.. rubric:: Example

.. code-block:: vql

   SELECT * 
   FROM CATALOG_VIEWS()
   WHERE viewtype = 1;

The procedure returns all the derived views of the database.