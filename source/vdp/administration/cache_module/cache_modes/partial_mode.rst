============
Partial Mode
============

When the Partial mode is enabled, the cache only stores some of the
tuples of the view, unlike in the Full cache mode where all the data of
the view is meant to be cached.

At runtime, when a user queries a view with this cache mode, the Server
checks if the cache contains the data required to answer the query. If
it does not have this data, the Server queries the data source.

When the Time to live (TTL) of the data is reached, the cache
invalidates the cached data of the view so the next query will hit the
data source.

This mode has the following options:

-  Explicit loads
-  Match exact queries only

Explicit Loads
=================================================================================

If this option is selected, the cache has to be loaded explicitly.

When this option is *not* selected, the response of a query to this view
is stored in the cache automatically.

When you select this option, you have to add the parameter
``'cache_preload' = 'true'`` to the ``CONTEXT`` clause of the query, so
the result is stored in the cache. Otherwise, the Server will retrieve
the data from the data source.

For example:

-  A user sets the cache of the view ``V`` to **Partial** and selects
   **Explicit loads**.
-  The user executes the query ``SELECT * FROM V`` twice.
   In both executions, the data is obtained from the data source because
   the cache of the view has not been loaded explicitly.
-  The user executes the query

   .. code-block:: sql

      SELECT *
      FROM V
      CONTEXT('cache_preload' = 'true')

-  The user executes the query ``SELECT * FROM V``.
   Now the Server obtains the data from the cache.
-  After TTL (Time to Live) seconds of executing the query, the Server
   marks the cached data as invalid. Afterward, if a user queries this
   view, the data will be retrieved from the source until the cache of
   this view is loaded again.

The section :ref:`Recommended Parameters for Queries that Load the Cache`
lists the recommended parameters of a cache preload query.



Match Exact Queries Only
=================================================================================

If this option is selected, the cache stores the result of each query.
Then, if the *same* query is executed and the entries of this query in
cache have not expired (the TTL has not been reached), the data returned
to the client is retrieved from the cache and not from the data source.

When this option is *not* selected, the cache module tries to solve the
queries with the data present in the cache, even if the query has not
been executed previously.

Let us see an example of what happens you enable or disable this option:


-  A user sets the cache of the view ``V`` to **Partial**.


-  The user executes the query ``SELECT * FROM V``.
   The result of this query is retrieved from the data source and stored in
   the cache.


-  If **Match queries only** was selected when enabling the cache:

   -  The user executes the same query again.
      In this case, the result is retrieved from the cache instead of from
      the data source.
   -  The user executes the query ``SELECT * FROM V where field1 = 1.0``.
      In this case, the Server retrieves the data *from the data source*
      and not from the cache. Although the view involved is the same as in
      the previous step, the condition is different. Therefore, the content
      of the cache is not used.


-  If **Match queries only** was *not* selected:

   -  The user executes the query
      ``SELECT * FROM V WHERE field1 = a``. The cache module detects that
      the result of this query is a subset of the first query that is
      already stored in cache. Therefore, it does not need to access the
      source to return the result.
      Note that if the option “Match queries only” was selected, the Server
      would access the data source to retrieve the result of the query
      because the queries are different.


-  After TTL (Time to Live) seconds of having executed the query
   ``SELECT * FROM V``, the Server marks the cache of this query as
   invalid. Afterward, if a user executes the same query again, the cache
   module will find that the entries in the cache for this query are
   invalid. Thus, the Server will retrieve the data from the data source
   and not from the cache.


.. important:: The “Match queries only” option should be selected
   when caching data from certain types of *non-relational* sources. For
   example, you should enable this option when caching data from a WWW
   (ITPilot) base view that retrieves data from a website that only
   returns the first 100 results of a query. In this scenario, the result
   of the query

   .. code-block:: sql

      SELECT *
      FROM V
      WHERE field1 = a and field2 = b

   .. partial_mode-a

   may not be a subset of the result of

   .. code-block:: sql

      SELECT *
      FROM V
      WHERE field1 = 1
