============
TRACE Clause
============

Using the ``TRACE`` clause of the ``SELECT`` command, the server will
return detailed information about the execution process of a query.

The trace of a statement provides a detailed examination of its
execution plan. This plan can be modeled as a tree, where each node
represents an intermediate view involved in the execution of a query or
an access to a data source via a wrapper.

The ``TRACE`` clause shows the most relevant parameters for each node on
the query execution tree. The Administration Tool allows examining the trace information using much more
user-friendly graphical interface.

Among the parameters displayed by the ``TRACE`` clause are:

-  *Node type*. If the node is a view, this indicates the type of view
   (base view, union, join, selection, intersect, etc.). If it is an access
   to a source (wrapper), this indicates the type of data source (JDBC, Web
   Service, Web, etc.).

-  *Execution time*. Time spent completely executing the node and all its
   children.

-  *Start time*. The exact moment at which node processing begins in the
   execution plan.

-  *End of query time*. The exact moment at which node processing (and that
   of all its children) ends in the execution plan.

-  *Time until the first tuple of results was obtained*. Time spent until
   the node receives the first tuple to be processed.

-  *Number of tuples processed*. Number of tuples processed by the node.

-  *Status*. This indicates whether the node was correctly executed or
   whether an error occurred.

-  *Advanced parameters*. These provide further details on each node type.
   For example:

   -  In the case of wrapper-type nodes, the exact sub-queries executed on
      each data source and the connection data used to access each one are
      indicated.
   -  For each view-type node, it is indicated whether the cache has been
      used, whether swapping has been necessary, etc. are indicated.
   -  A parameter of particular interest for optimization reasons is “No
      Delegation Cause”. In the views defined based on tables from the same
      JDBC or ODBC data source, Virtual DataPort will try to delegate the entire
      process to the source database, obtaining all the tuples from the
      view through a single query. This strategy may save a significant
      amount of execution time in complex views. When Virtual DataPort is
      unable to delegate the entire process of a certain query to a source
      database, it will indicate a reason in this parameter. For example,
      the query may use an expression that includes a function that is not
      supported by the source database, which will force Virtual DataPort
      to post-process the results obtained. In light of the reason where
      the processing could not be delegated, it may be possible to rewrite
      the view so that it can be delegated.

-  *Error conditions*. The trace also indicates any errors produced during
   node execution.

As an example, the figure :ref:`below <Execution trace>` shows the execution trace of the following query:

.. code-block:: sql

   SELECT * 
   FROM INTERNET_INC TRACE

``INTERNET_INC`` is a base view created on a table of the same name
accessible via a JDBC data source.



.. code-block:: vql
   :caption: Execution trace
   :name: Execution trace

   BASE PLAN (
       name = INTERNET_INC
       startTime = Wed Jan 10 17:50:01 850 GMT+01:00 2007
       endTime = Wed Jan 10 17:50:04 063 GMT+01:00 2007
       responseTime = Wed Jan 10 17:50:04 053 GMT+01:00 2007
       numRows = 4
       state = OK
       completed = true
       fields = [IINC_ID, SUMMARY, TTIME, TAXID, SPECIFIC_FIELD1, SPECIFIC_FIELD2]
       search conditions = []
       filter conditions = []
       numOfFilteredTuples = 0
       numOfDuplicatedTuples = 0
       numOfSwappedTuples = 0
       swapping = false
   
       JDBC WRAPPER (
           name = internet_inc
           startTime = Wed Jan 10 17:50:02 070 GMT+01:00 2007
           endTime = Wed Jan 10 17:50:04 063 GMT+01:00 2007
           responseTime = Wed Jan 10 17:50:04 063 GMT+01:00 2007
           numRows = 4
           state = OK
           completed = true
           searchConditions = []
           orderByFields = []
           projectedFields = [IINC_ID, SUMMARY, TTIME, TAXID, SPECIFIC_FIELD1, SPECIFIC_FIELD2]
           JDBC ROUTE (
               name = internet_inc#0
               startTime = Wed Jan 10 17:50:03 782 GMT+01:00 2007
               endTime = Wed Jan 10 17:50:04 063 GMT+01:00 2007
               responseTime = Wed Jan 10 17:50:04 063 GMT+01:00 2007
               numRows = 4
               state = OK
               completed = true
               SQLSentence = SELECT t0.iinc_id, t0.summary, t0.ttime, t0.taxId, t0.specific_field1, t0.specific_field2 FROM test_vdb.internet_inc t0
               parameters = []
               DBUri = jdbc:mysql://localhost/test_vdb
               userName = vdb
               connectionTime = 0
               cachedStatus = false
           )
       )
   )


We **strongly** recommend using the Administration Tool to analyze the
execution trace of queries, because it displays the trace in a graphic.
See more information about this in the section :ref:`Execution Trace of a
Statement` of the Administration Guide.
