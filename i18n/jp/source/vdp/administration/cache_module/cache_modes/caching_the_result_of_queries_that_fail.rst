=======================================
Caching the Result of Queries that Fail
=======================================

By default, the result of queries that fail is not cached, but you can
cache it if you want.

For example, let us say that you execute a union view and one of the
branches of the union fails but you still want to cache the result
obtained from the other branches of the union. To do this, add the
``cache_load_on_error`` parameter to the ``CONTEXT`` clause with the
value ``true``.

For example:

.. code-block:: sql

   SELECT * 
   FROM incidents 
   CONTEXT (
         'cache_preload' = 'true'
       , 'cache_invalidate' = 'all_rows'
       , 'cache_load_on_error' = 'true'
       , 'cache_wait_for_load' = 'true'
       , 'cache_return_query_results' = 'false')


