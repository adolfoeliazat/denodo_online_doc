=========
Full Mode
=========

When a view uses the Full cache mode, the data of the view is always
retrieved from the cache database instead of from the source.

The main benefit of this mode over the partial cache is that complex
operations (joins, unions, group by …) involving several views (even
from different data sources) can be delegated to the cache database.
Therefore, the performance of these operations is significantly
improved.

With this cache mode, you have to execute a special query (“Cache
preload query”) to fill the cache of the view. The results of this query
are inserted in the cache and any query to this view will be based on
these data.

The “Cache preload query” is a regular query with the parameter
``'cache_preload' = 'true'`` in the ``CONTEXT`` clause. For example:

.. code-block:: sql

   SELECT *
   FROM internet_inc
   CONTEXT (
         'cache_preload' = 'true'
       , 'cache_invalidate' = 'all_rows'
       , 'cache_wait_for_load' = 'true'
       , 'cache_return_query_results' = 'false')

The “Cache preload query” has to meet the following conditions:

#. It has to project all the fields of the view.
   I.e.: ``SELECT * FROM ...``
#. It cannot have aggregation clauses or aggregation
   functions. I.e. ``GROUP BY`` or ``HAVING`` are forbidden. It can have
   selection conditions (``WHERE`` clause)
#. It cannot have subqueries in the ``WHERE`` clause.
#. It has to be executed over the view that has the full cache 
   mode enabled; not over its underlying views nor its derived views.

You will have to execute these queries periodically. To do this, we
recommend using the Denodo Scheduler because it provides a wizard where
you can select the views whose cache you want to preload, how often and
select the parameters used in these queries.

The following sections describe two ways to
preload the cache of a view with “Full” cache:

#. Loading the cache invalidating the existing data. This is the most common way of doing it.
   See :ref:`below <Loading the Cache Invalidating the Existing Data>`.
#. Loading the cache incrementally. See :ref:`Loading the Cache
   Incrementally`.

Loading the Cache Invalidating the Existing Data
=================================================================================

When the query has the parameters
``('cache_preload' = 'true',  'cache_invalidate' = 'all_rows')`` in the
``CONTEXT`` clause of the query, the Cache Engine marks as invalid all
the cached data of the queried view and then, stores the result of the
query in the cache database.

For example, this query invalidates all the cached rows for the view
``customer`` and then, caches the result of the query:

.. code-block:: sql

   SELECT *
   FROM customer
   CONTEXT (
     'cache_preload' = 'true'
   , 'cache_invalidate' = 'all_rows'
   , 'cache_wait_for_load' = 'true'
   , 'cache_return_query_results' = 'false')



Loading the Cache Incrementally
=================================================================================

In some scenarios, when a view uses the Full cache mode, you may need to
load the cache of the view by executing more than one query. For
example, because doing it with a single query would put too much load on
the source. In this scenario, you cannot use the parameter
``'cache_invalidate' = 'all_rows'`` because it invalidates all the view's
cache.

Virtual DataPort provides two other alternatives to
``'cache_invalidate' = 'all_rows'``:

#. Adding the parameters
   ``('cache_preload' = 'true', 'cache_invalidate' = 'matching_rows')``: the
   Cache Engine invalidates the cached rows that match the ``WHERE``
   clause of the query and then, caches the result of the query.
   Adding these parameters to the query is equivalent to executing the
   view from the “Execute” dialog of the view and selecting the “Store
   results in cache” and the “Invalidate existing results” check boxes.

   For example, let us say that you have a table ``order`` whose cache
   mode is full and you need to update the data cached for this view in
   order to have the most recent records for the current year. However,
   you do not want to invalidate the cached data for the previous years
   because it has not changed and retrieving it all again it would take
   too much time. With the query below, you achieve this: it only
   returns the orders whose date is the current year and as it has the
   parameter ``'cache_invalidate' = 'matching_rows'``, the Cache Engine
   only invalidates the rows that match the condition of the query and
   caches the data obtained from the source.

   .. code-block:: sql

      SELECT *
      FROM order
      WHERE order_date >= TRUNC( CURRENT_DATE(), 'Y')
      CONTEXT (
      'cache_preload' = 'true'
      , 'cache_invalidate' = 'matching_rows'
      , 'cache_wait_for_load' = 'true'
      , 'cache_return_query_results' = 'false')


#. Adding the parameter ``('cache_preload' = 'true')`` to the ``CONTEXT``
   clause of the query but not ``'cache_invalidate'``: the result of the
   query is stored in the cache database, without invalidating the
   current cached data. By using this parameter and executing several
   queries, the cache can be loaded incrementally.

   Adding this parameter is equivalent to executing the view from the
   “Execute” dialog of the view, selecting the “Store results in cache”
   check box and clearing the “Invalidate existing results” one. When
   doing this, you need to make sure that the results of the queries you
   use to load the cache incrementally, do not overlap. For example, if
   you execute the following queries:

   .. code-block:: sql

      SELECT *
      FROM phone_inc
      WHERE pinc_id > 0
      CONTEXT (
      'cache_preload' = 'true'
      , 'cache_wait_for_load' = 'true'
      , 'cache_return_query_results' = 'false')


   And,

   .. code-block:: sql

      SELECT *
      FROM phone_inc
      WHERE pinc_id > -1
      CONTEXT (
      'cache_preload' = 'true'
      , 'cache_wait_for_load' = 'true'
      , 'cache_return_query_results' = 'false')



The cache will store the data returned by both queries. Then, if you
execute the query ``SELECT * FROM phone_inc``, the result will be union
of these queries, which will contain the same rows twice.



The Full Cache Mode at Runtime
=================================================================================

When querying a view with Full cache enabled, the data is always
retrieved from the cache, even if no cache preload queries have been
executed. If the cache of the view has not been preloaded, the queries to this view will return 0 rows.

In terms of performance, the main benefit of using the Full cache mode
is that if you combine two views with cache Full enabled, the query is
delegated to the cache. This may result in a faster execution time. This
optimization is not available when using the Partial cache mode.

For example, if you enable the full cache mode on two views whose data comes from different data sources, and you execute a JOIN between them, the join will be delegated to the cache
database instead of executing it in the Server.

.. important:: When this cache mode is enabled, the cache engine does
   *not* check the cache preload queries. For example, if the query was loaded with this
   query:

   .. code-block:: sql

      SELECT *
      FROM employee WHERE id = 1
      CONTEXT ('cache_preload' = 'true', 'cache_invalidate' = 'all_rows')

   And later execute the query

   .. code-block:: sql

      SELECT *
      FROM employee

   The data is retrieved from the cache, which means that you will only
   obtain the rows that match the condition ``id = 1``.

|

The cache engine does the following when a query has the parameter ``'cache_preload' = 'true'``:

1. The cache engine checks that the table that is supposed to store the cache data of this view exists. It also checks that the table has the appropriate schema. This table is created when you enable the full or partial cache mode.

2. As soon as the query begins returning rows, the cache engine begins inserting them into the cache table of the view, with the state "processing". Each cache table has a column that indicates the status of the row: processing, valid or invalid.

3. Once all the rows are inserted in the cache table, the cache engine starts a transaction on the cache database. In this transaction, the cache engine executes one or two queries, in this order:

   i.  If the query has the parameter ``'cache_invalidate'``, it will execute an UPDATE statement to change the rows with status "valid" to "invalid".

       If the query has ``'cache_invalidate' = 'all_rows'``, the status of all the "valid" rows switches to "invalid".

       If the query has ``'cache_invalidate' = 'matching_rows'``, the status of the "valid" rows that match the WHERE condition of the query switches to "invalid". If the query does not have a WHERE clause, ``matching_rows`` is equivalent to ``all_rows``.

   ii. An UPDATE statement that changes the status of the "processing" rows to "valid".

4. The cache engine commits the transaction and from now on, the queries that hit this view will see the new data.

This process ensures that the queries that hit a view with full cache mode always return valid data, even if another query hits the view while its cache is being loaded.

If a query hits the view while data is being inserted on the cache table, the data is valid because the cache engine always returns the rows with status "valid". The rows that have been inserted so far have the status "processing", not "valid."

If a query hits the view while the status of the rows is being changed from "processing" to "valid", the data returned is valid because this UPDATE is performed inside a transaction. Therefore, this query will still return the "old" rows, but will not return partially modified rows.

If ``cache_wait_for_load`` is ``false`` or not present, as soon as all the rows of the result have been returned to the client, the client is notified that the query finished, even if the data has not been completely inserted in the cache database. If ``cache_wait_for_load`` is ``true``, the client is notified that the query finished after all the steps listed above finish.
