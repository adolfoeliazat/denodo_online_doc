==============
CACHE_CONTENT
==============

.. rubric:: Description

The stored procedure ``CACHE_CONTENT`` returns information about the
cached contents.

For each query whose data is cached, the Server stores a Query Pattern
that later is used to know if a query can be “solved” with the data
stored in the cache.

The stored procedure ``CACHE_CONTENT`` returns information about the
Query Patterns stored in the cache. See the section :ref:`Cache Module` of
the Administration Guide for more information about Query Patterns.

.. rubric:: Syntax

.. code-block:: bnf

   CACHE_CONTENT(
         databaseName : text
       , viewName : text
   )

-  ``databaseName``: name of the database. If ``null``, the procedure
   returns information about the Query Patterns of the database you are
   currently connected to.
-  ``viewName``: name of the view you want to obtain its fields. If
   ``null``, the procedure returns information about all the Query
   Patterns of ``databaseName``.

The procedure returns these fields:

-  ``querypatternid``: the ID of the Query Pattern.
-  ``databasename``: the name of the database.
-  ``viewname``: the name of the view.
-  ``numconditions``: number of conditions. It only applies to views with cache mode "partial". For cache mode "full", this is always 0.
-  ``conditions``: list of conditions of the query pattern. It only applies to views with cache mode "partial".
-  ``projectedfields``: list of projected fields. If ``null``, all the fields of the view were projected.
-  ``expirationdate``: for views with cache "partial", it shows the expiration date of the Query Pattern. For views with cache "full", it shows the date of the latest cache refresh.
-  ``status``: status of the Query Pattern. Its values can be:
   
   -  ``valid``: query pattern of a view with cache mode "partial" and whose data in the cache database are valid.
   -  ``invalid``: query pattern of a view with cache mode "partial" and whose data in the cache database are invalid.
   -  ``processing``: query pattern of a view with cache mode "partial" and that the cache engine is currently inserting the data in the cache database.
   -  ``preload``: query pattern of a view with cache mode "full".
   -  ``temporary``: query pattern of a temporary table.

.. rubric:: Remarks

This procedure returns information about all the valid content in the cache database, including views whose cache mode was set to off but the data is still valid. I.e. if you set back the cache mode you had before, the cache engine will return the data already cached.

.. rubric:: Privileges Required

If the first parameter (``databaseName``) is ``null``, the user needs to be an
administrator.

If the first parameter (``databaseName``) is not ``null``, the user needs to be an
administrator or a local administrator of that database.
