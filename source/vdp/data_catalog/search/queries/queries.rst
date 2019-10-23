=======
Queries
=======

Depending on the kind of search (catalog or content), you can make different queries.

-  The :ref:`Catalog search <data_catalog_catalog_search>` allows for simple and inclusive queries:

   -  Simple: it does not allow complex conditions with operands (!, AND, OR).
   -  Inclusive: the search results will contain the elements whose names contain the searched text. For instance, if there is a view *CUSTOMER* and another one *DV_CUSTOMER*, 
      a search by *customer* will include both views in its result (as *DV_CUSTOMER* contains the searched text).
    
-  The :ref:`Content search <Content Search>` allows more complex queries and its syntax depends on the :ref:`configured search engine <Search Configuration>`: 

   -  *Scheduler Index*: they keywords must follow the :ref:`Apache Lucene Search Syntax`.
   -  *Elasticsearch*: they keywords must follow the Query String Syntax, according to the version of the Elasticsearch server:
    
      -  `Elasticsearch 2.x <https://www.elastic.co/guide/en/elasticsearch/reference/2.3/query-dsl-query-string-query.html#query-string-syntax>`_.
      -  `Elasticsearch 5.x <https://www.elastic.co/guide/en/elasticsearch/reference/5.x/query-dsl-query-string-query.html#query-string-syntax>`_.
      -  `Elasticsearch 6.x <https://www.elastic.co/guide/en/elasticsearch/reference/6.x/query-dsl-query-string-query.html#query-string-syntax>`_.
    
   .. note:: Both *Scheduler Index* and *Elasticsearch* do not allow to specify the fields to search in. Searches are performed in their default search field.