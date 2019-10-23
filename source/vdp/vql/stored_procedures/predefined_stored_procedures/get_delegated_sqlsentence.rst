=========================
GET_DELEGATED_SQLSENTENCE
=========================

.. rubric:: Description

The stored procedure ``GET_DELEGATED_SQLSENTENCE`` returns the SQL statement that the Execution Engine will push down to a database when executing a given query.

In order to do this, this query has to the following conditions:

#. The query is a SELECT statement.
#. The query can be fully pushed down to a single JDBC data source.

.. rubric:: Syntax

.. code-block:: bnf

   GET_DELEGATED_SQLSENTENCE (
         vdp_query : text
   )

The procedure returns these fields:

-  ``database_name``: name of the database where de data source is located at.

-  ``datasource_name``: name of the data source that the SQL would be pushed down.

-  ``delegated_query``: SQL that the execution engine will execute in the underlying database of the data source.

The ``delegated_query`` is generated assuming the source configuration property *Allow literals as parameter* of the JDBC data source is *false*, regardless of its actual value.

.. rubric:: Usage notes

-  You can use the ``CONTEXT`` clause in the query passed to the procedure.

-  The cache mode of the views involved in the query affects the result. For example, the procedure will return an error if you pass a 
   query over a view that is a join of a JDBC base view and an XML base view. That is because this query does not meet the conditions listed above. However, 
   if the cache of this view is enabled, the procedure will return the SQL that the execution engine would push down to the cache data source.

.. rubric:: Examples

.. code-block:: sql

   SELECT * 
   FROM GET_DELEGATED_SQLSENTENCE()
   WHERE vdp_query = 'SELECT int_field, text_field FROM join_view CONTEXT(''cache''=''off'')'
   
Obtains the query that would be delegated to the source if the cache was disabled.