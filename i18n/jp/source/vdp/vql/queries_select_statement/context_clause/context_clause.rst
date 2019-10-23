==============
CONTEXT Clause
==============

The ``CONTEXT`` clause is used to modify certain configuration
preferences to execute a specific query, without overriding the values
configured by default.
   
In general, the ``CONTEXT`` clause receives *key-value* pairs (separated
by commas), where the *key* is the name of the execution characteristic
to be modified and the *value* indicates the new value for said
characteristic. Both key and value are literals, so they must be set
with quotation marks or double quotation marks. The name of the key is
not case-sensitive, while in the case of the value it depends on the property.


.. code-block:: bnf
   :caption: Syntax of the CONTEXT clause
   :name: Syntax of the CONTEXT clause

   <context information> ::=
       'cache' = { 'on' | 'off' }                   // 'on' by default
     | 'cache_atomic_operation' = { 'true' | 'false' } // 'true' by default
     | 'cache_invalidate' = 
       { 'matching_rows' | 'all_rows' }   // 'matching_rows' by default
     | 'cache_invalidate_block_size' = <literal>          // 10000 by default
     | 'cache_load_on_error' = { 'true' | 'false' } // 'false' by default
     | 'cache_preload' = { 'true' | 'false' }       // 'false' by default
     | 'cache_return_query_results' = { 'true' | 'false' }  // 'true' by default
     | 'cache_wait_for_load' = { 'true' | 'false' }    // 'false' by default
     | 'costoptimized' = { 'on' | 'off' }           // 'on' by default
     | 'data_movement_bulk_load' = { 'on' | 'off' } // 'on' by default
     | 'data_movement_clean_resources' = { 'true' | 'false' } // 'true' by default
     | 'data_movement_clean_resources_on_error' = { 'true' | 'false' } // 'true' by default
     | 'i18n' = <literal> // e.g. 'es_euro', ...
     | 'nodelegateviews' = <literal>
     | 'queryTimeout' = <literal>
     | 'simplify' = { 'on' | 'off' } // 'on' by default
     | 'swap' = { 'on' | 'off' }
     | 'swapsize' = <number>
     | 'var <var name>' = <literal>
     | USERNAME = <literal>
     | PASSWORD = <literal> [ ENCRYPTED ]
     | DOMAIN = <literal>
     | DATAMOVEMENTPLAN = <data movement plan>
     | MPPMOVEMENTPLAN = <MPP movement plan>
     | QUERYPLAN = <query plan>
     | SUBQUERYPLAN = <subquery plan>
     | VIEWPROPERTIES = <view properties>
   
   <data movement plan> ::=
     [ <view name:identifier> : <data movement view plans> ]+
   
   <data movement view plans> ::=
       <data movement view plan>
     | [ ( [ <data movement view plan> ] ) ]+
   
   <data movement view plan> ::= 
       JDBC <data source name:identifier> 
     | OFF
   
   <MPP movement plan> ::=
     [ <view name:identifier> : <MPP movement view plans> ]+
  
   <MPP movement view plans> ::=
       <MPP movement view plan>
     | [ ( [ <MPP movement view plan> ] ) ]+
  
   <MPP movement view plan> ::= 
       ON
     | OFF
   
   <subquery plan> ::= 
     { ANY | HASH | MERGE | NESTED } { ORDERED | REVERSEORDER | ANY }
   
   <view properties> ::= 
     [ <view name:identifier> : ( <view property> [, <view property> ]* ) ]+
   
   <view property> ::= 
     'begindelimiter' = <literal>

.. 

   <query plan> ::= (see :ref:`QUERYPLAN syntax`)


-  ``cache``. If ``off``, the execution engine will deactivate the cache
   engine for this query and thus, it will retrieve the data from the
   sources.
   
   If ``on``, the execution engine will execute the query with the cache
   configuration of the views.
   
   The default value is ``on``.
   
   The section :ref:`Using the Cache` explains in detail how to configure the
   cache of a view.

-  ``cache_atomic_operation``. By default, there are two tasks regarding
   the cache engine that are performed atomically: marking data as
   invalid in the cache; and marking as valid data that has been stored
   in the cache database. If this parameter is ``false``, these two
   operations are not atomic.
   
   See more about this parameter in the section :ref:`Caching Very Large Data
   Sets` of the Administration Guide.

   Default value: ``true``.

-  ``cache_invalidate``. The behavior of this parameter changes depending
   on the cache mode of the view.

   -  For views with “Partial” cache, when this parameter is
      ``matching_rows``, the “Query pattern” associated with the executed
      query is invalidated and the data is retrieved from the source and
      stored in the cache.
      
      For example, let us say that we execute the following queries:
      
      1. ``SELECT * FROM V``: the result is obtained from the source and
         then, stored in cache.
      2. ``SELECT * FROM V``: the result is obtained from the cache.
      3. ``SELECT * FROM V CONTEXT  ('cache_invalidate' = 'matching_rows')``:
         even if the cached data has not reached the “Time to Live”, it is
         invalidated. Then, the result is obtained from the source and cached.
         
         For views with “Partial” cache, do not use the parameter
         ``('cache_invalidate' = 'all_rows')`` The section :ref:`Cache Module` of
         the Administration Guide explains the concept of “Query pattern”.
   
   -  For views with “Full” cache, the cache is not loaded automatically.
      Instead, the cache has to be loaded with the results of the queries
      that have the parameter ``cache_preload`` in its ``CONTEXT``.
      
      If ``cache_invalidate`` is ``all_rows``, the content of the cache of
      the view is deleted before caching the result of the query.
      
      If ``cache_invalidate`` is ``matching_rows``, only the rows of the
      cache that match the ``WHERE`` condition of the query are
      invalidated.
      
      If ``off``, the result of the query is cached without deleting the
      existing data.
      
      See more about this parameter in the section :ref:`Loading the Cache
      Invalidating the Existing Data` of the Administration Guide.

   Default value: ``matching_rows``.

-  ``cache_invalidate_block_size``. When the cached data for a view is
   invalidated in a non-atomic way (``'cache_atomic_operation' = 'no'``
   is in the ``CONTEXT`` clause), the cached rows are invalidated
   in blocks. This parameter sets the size of these blocks.
   
   See more about this parameter in the section :ref:`Caching Very Large Data
   Sets` of the Administration Guide.
   
   Default value: ``10000``.

-  ``cache_load_on_error``. By default, the result of queries that fail
   is not cached. If ``true``, the result of the queries that fail is
   cached anyway.
   
   For example, let us say that you execute a union view and one of the
   branches of the union fails but you still want to cache the result
   obtained from the other branches of the union. To do this add this
   parameter to the ``CONTEXT`` clause with the value ``true``.
   
   Default value: ``false``.

-  ``cache_preload``. The cache has to be loaded manually when the cache
   mode of a view is “Full” or “Partial with explicit loads”. If the
   value of this parameter is ``true``, the results of this query will be
   inserted in cache.
   
   Only use when the cache mode of a view is “Full” or “Partial with
   explicit loads”.
   
   Default value: ``false``.

-  ``cache_return_query_results``. If ``false``, the query is processed
   entirely but it does not return any data.
   
   Use this parameter to speed up the process of loading the cache of a
   view.
   
   See more about this in the section :ref:`Full Mode` of the Administration
   Guide.
   
   Default value: ``true``.

-  ``cache_wait_for_load``. If ``true``, the query does not finish until
   the data is completely stored in cache. This parameter only works when
   loading the cache of a view whose cache mode is “Partial with explicit
   loads” or “Full“.
   
   If ``false``, the query finishes when the client has received all the
   rows, even if they have not been stored in cache yet.
   
   See more about this parameter in the section :ref:`Full Mode` of the
   Administration Guide.
   
   Default value: ``false``.

-  ``costoptimized``. If ``off``, the Execution Engine disables the
   “Cost-based optimization” to calculate the execution plan of the
   query.
   
   See more about this in the section :ref:`Cost-based Optimization` of the
   Administration Guide.
   
   Default value: ``on``.

-  ``DATAMOVEMENTPLAN``. This parameter defines the data movements of the
   execution of the query.
   
   The section :ref:`Data Movement` of the Administration Guide explains what
   a Data movement is and its subsection “Examples” contains several
   examples that use the ``DATAMOVEMENTPLAN`` parameter.

-  ``MPPMOVEMENTPLAN``. This parameter defines the data movements of views to a massive parallel processing database.

   The section :ref:`Parallel Processing` explains what this is and examples of how to use this parameter.

-  ``data_movement_bulk_load``. If ``off`` and the execution engine is going to perform a data movement for this query, the execution engine will not use the bulk load API of the target database.

   Default value: ``on``.

-  ``data_movement_clean_resources``, ``data_movement_clean_resources_on_error`` and ``data_movement_clean_resources_on_error``: the section :ref:`Options of the CONTEXT Clause that Control a Data Movement` of the administration guide explains how these properties affect data movements.

-  ``formatted``. By default, Virtual DataPort does not preserve the
   formatting of the ``CREATE VIEW`` statements. To preserve it, add the
   parameter ``'formatted' = 'yes'`` to the ``CONTEXT`` clause of the
   statement.

   This feature is useful if you have a very long ``CREATE VIEW`` statement
   and you formatted it in a way that is easier to read and you want to
   keep this format.
   
   The administration tool automatically adds this parameter when you
   manually edit the VQL of a derived view.

-  ``i18n``. Internationalization configuration for the results of the
   query. This parameter takes the name of a valid internationalization
   configuration as a value (e.g. ``es_euro``).
   
   Example: the following statement obtains all rows from view ``V``
   setting the ``us_pst`` internationalization configuration only for
   this query:

   .. code-block:: sql
   
     SELECT * 
     FROM V 
     CONTEXT ('i18n' = 'us_pst')

-  ``noDelegateViews``. List of views that will not be delegated to the
   data source, in the execution of the query.
   
   There are scenarios where a data combination can be delegated to a
   source but we do not want to do so (e.g. bad performance/limited
   resources of the source). In these scenarios, it is useful to specify
   if we do not want to delegate a certain view.
   
   For example, we have a view ``incidents`` that is the join of the JDBC
   base views ``internet_inc`` and ``phone_inc`` that were created over
   the same data source.
   
   The query ``SELECT * FROM incidents`` will result in sending the JOIN
   query to the database:
   ``SELECT * FROM phone_inc INNER JOIN internet_inc...``
   
   If use execute
   ``SELECT * FROM incidents CONTEXT('nodelegateviews' = 'incidents')``
   Virtual DataPort will send two queries to the database:
   ``SELECT * FROM phone_inc`` and ``SELECT * FROM internet_inc``.

-  ``QUERYPLAN``. This allows different characteristics of the query
   running plan to be specified in run time. See section :ref:`Dynamic Choice of
   Join Strategy` for more details. This option requires having ``WRITE``
   privileges over the view.

-  ``QUERYTIMEOUT``. Maximum time (in milliseconds) the Server will wait
   for a query to finish. After this period, the Server will cancel the
   query.
   
   All the clients that connect to Virtual DataPort via JDBC or ODBC
   establish a default timeout for the queries. This parameter changes
   the timeout of the query, without having to change the default query
   timeout parameter of the connection.
   
   If 0, the query will not be cancelled.

-  ``simplify``. If ``on``, the Execution engine enables the Automatic
   simplification for this query. If ``off``, it disables this, for this
   query.
   
   See more about this in the section :ref:`Automatic Simplification of
   Queries` of the Administration Guide.
   
   Default value: ``on``.

-  ``SUBQUERYPLAN``. In views with subqueries, by adding the
   ``SUBQUERYPLAN`` parameter to the ``CONTEXT`` clause of the subquery,
   you can modify the query plan of the subquery. See more about this in
   the section :ref:`Subqueries in the WHERE Clause of the Query`.

-  ``swap``. This indicates whether swapping is enabled for the query. This
   parameter must take the ``ON`` value to indicate that the swapping
   of intermediate results is permitted, while the query is being run. The
   ``OFF`` value indicates the opposite. See section :ref:`Configuring
   Swapping Policies` for more details.

-  ``swapSize``. This parameter indicates the maximum size an intermediate
   result obtained by running this query can reach without swapping to
   disk. It is given the maximum size (in megabytes) as a parameter. It is
   only effective where the ``SWAP ON`` option has been specified. See
   section :ref:`Configuring Swapping Policies` for more details.

-  ``USERNAME``, ``PASSWORD`` and ``DOMAIN``. These three parameters are
   only taken into account for data sources created with the
   clause ``WITH PASS-THROUGH SESSION CREDENTIALS`` *and* of the type JDBC, web service, BAPI or multidimensional data sources. Use these options to
   query a view of the data source with other credentials than the ones you
   used to connect to the Server.

   Example: if ``view1`` has been created over a JDBC data source with the
   option ``WITH PASS-THROUGH SESSION CREDENTIALS`` and you execute

   .. code-block:: sql

      SELECT * 
      FROM view1
      CONTEXT(USERNAME = 'admin', PASSWORD = 'd4GvpKA5BiwoGUFrnH92DNq5TTNKWw58I86PVH2tQIs/q1RH9CkCoJj57NnQUlmvgvvVnBvlaH8NFSDM0x5fWCJiAvyia70oxiUWbToKkHl3ztgH1hZLcQiqkpXT/oYd' ENCRYPTED)

   the Server connects to the database of the view with the username
   ``admin`` and the password ``password``, ignoring the
   credentials provided by the user to connect to the Server.

   It is mandatory to add the token ``ENCRYPTED`` and enter the password encrypted. To encrypt the password, execute the statement ``ENCRYPT_PASSWORD``. For example:

   .. code-block:: vql

      ENCRYPT_PASSWORD 'my_secret_password';

   .. note:: ``DOMAIN`` is used only when the source is a web service with NTLM
      authentication.
  
-  ``var``. Use this parameter to set the values of the variables, which
   will be used when executing views that use the function ``GETVAR``.
   See section :ref:`Adding Variables to Selection Conditions (GETVAR and
   SETVAR)` to see more information about using context variables.
   
   Example: the following query obtains the clients with a max income of
   1000000.
   
   .. code-block:: vql
   
      SELECT * 
      FROM client 
      WHERE income > GETVAR('_var_client_income_limit', 'int', 500000) 
      CONTEXT('VAR _var_client_income_limit' = '1000000') 

-  ``VIEWPROPERTIES``. This enables you to indicate a series of properties
   for the views forming part of the query tree. This option requires
   having ``WRITE`` privileges over the view. Currently, only the
   ``begindelimiter`` parameter is supported. This parameter can be applied
   to DF base views (see section :doc:`/vdp/vql/generating_wrappers_and_data_sources/creating_data_sources/json_sources` for a description of these
   data sources and of the ``begindelimiter`` parameter) to dynamically
   choose the point from which to begin access to the delimited source file
   through a regular expression. If ``isdata`` is also specified, the
   delimiter will be considered to form part of the data.

   **Example**: let us say that ``V2`` is a DF base view created based on
   a data source of the delimited file type forming part of the ``V``
   definition tree, the following statement obtains the tuples from the
   delimited file from the first tuple matching the regular expression
   specified (in this case, any starting with the string ``05/24/2008``):
   
   .. code-block:: vql
   
      SELECT * 
      FROM V 
      CONTEXT (VIEWPROPERTIES = V2:('begindelimiter' = '05/24/2008(.*)' 'ISDATA'))



.. note:: The “View Properties” option is deprecated and should not be
   used in new applications. If you need to specify at runtime the value
   for the ``begindelimiter`` parameter of a delimited files data source,
   you can use interpolation variables in the value of such parameter (see
   section :ref:`Paths and Other Values with Interpolation Variables` of the
   Administration Guide).

.. note:: Apart from these properties, we can also set the values of the
   selection conditions’ variables of the views involved in the query. The
   appendix :ref:`Adding Variables to Selection Conditions (GETVAR and SETVAR)`
   explains what selection conditions with variables are.

