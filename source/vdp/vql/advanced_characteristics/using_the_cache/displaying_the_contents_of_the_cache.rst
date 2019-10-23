====================================
Displaying the Contents of the Cache
====================================

For each query whose data is cached, the Server stores a Query Pattern
that later is used to know if a query can be “solved” with the data
stored in the cache. The Denodo stored procedure ``CACHE_CONTENT``
returns information about the Query Patterns stored in the cache. See
the section :doc:`/vdp/administration/cache_module/cache_module` of the Administration Guide for more
information about Query Patterns.



.. code-block:: bnf
   :caption: Syntax of the CACHE_CONTENT procedure
   :name: Syntax of the CACHE_CONTENT procedure

   CACHE_CONTENT( <database:string>, <view:string> )


The output of this procedure has the following fields that describe the
Query Pattern:

-  QueryPatternId: the ID of the Query Pattern.
-  DatabaseName: the name of the database.
-  ViewName: the name of the view.
-  NumCondition: Number of conditions.
-  Conditions: List of conditions.
-  ProjectedFields: List of projected fields. If blank, it means that
   all the fields of the view were projected.
-  ExpirationDate: Expiration date of the Query Pattern.
-  Status of the Query Pattern. Its values can be ``Valid``, ``Invalid``
   or ``Processing``.

.. code-block:: sql
   :name: Sample invocation of the CACHE_CONTENT procedure
   :caption: Sample invocation of the CACHE_CONTENT procedure

   CALL CACHE_CONTENT('admin', 'incidences')

-  If the input parameter ``view`` is ``NULL``, the procedure returns
   information about all the Query Patterns of ``database``.
-  If the input parameter ``database`` is ``NULL``, the procedure
   returns information about the Query Patterns of the database you are
   currently connected to.
-  This procedure can be executed only by administrators or local
   administrators of the database you are obtaining its Query Patterns
   of.
