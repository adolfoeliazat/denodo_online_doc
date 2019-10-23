==================
Cache Invalidation
==================

The cache of a view can be invalidated using the ``ALTER VQL`` sentence
(see section :ref:`Modifying a Derived View`).

There are two types of cache invalidation:

#. “Full” invalidation, which removes all the cached data for a view.
#. “Partial” invalidation that only removes the cached data that
   verifies a specified condition.

Both invalidation types support the ``CASCADE`` option, which causes the
operation to propagate to the views participating in the definition of
the view specified in the invalidation sentence.

The next sentence would invalidate the cache for the ``sampleView`` view
and for all the views participating in the ``sampleView`` definition:

.. code-block:: vql

   ALTER VIEW sampleView
   CACHE INVALIDATE ON CASCADE;

The next sentence is a partial invalidation. Only the tuples matching
with the specified condition will be invalidated. In this case, the
``CASCADE`` option is not used and, therefore, the sentence will affect
the ``sampleView`` view only:

.. code-block:: vql
   
   ALTER VIEW sampleView
   CACHE INVALIDATE WHERE field1 = 'value';

The cache of views with “Full” cache cannot be invalidated on cascade.
In addition, if you invalidate on cascade the cache of a view, the cache
of the subviews whose cache mode is “Full”, will not be invalidated.

The stored procedure ``CACHE_CONTENT`` returns information about the
contents of the cache. The section :ref:`CACHE_CONTENT` provides more
details about this procedure, its syntax and the information it returns.

