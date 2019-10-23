=================================
Mapping the Result of MDX Queries
=================================

Although we recommend creating Multidimensional base views graphically,
you can also create them with an MDX query.

MDX (Multidimensional Expressions) is a query language for
multidimensional databases, as SQL is a query language for relational
databases.

The result of an MDX query is formed by **axes**, which are similar to
the *dimensions* of a multidimensional structure.

Virtual DataPort maps these axes into a relational structure by creating
one field for each value of the ON COLUMNS axis and one field for each
dimension of the ON ROWS axis. If a dimension of the ON ROWS axis is
hierarchical, there will be a column for each level of this dimension.

.. note:: Virtual DataPort only supports MDX queries with the ONROWS and
   ONCOLUMNS axes.

.. warning:: The MDX query has to include the members of the parent
   levels of those levels that appear in the result. Otherwise, the result
   may not be correct.

 

.. code-block:: none
   :caption: Sample MDX query
   :name: Sample MDX query

   SELECT 
       [Measures].MEMBERS ON COLUMNS,
       NON EMPTY CROSSJOIN ([Region].MEMBERS, [Time].MEMBERS) ON ROWS
   FROM [$Sales]

