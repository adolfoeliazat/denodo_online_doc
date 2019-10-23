===========================================
Subqueries in the WHERE Clause of the Query
===========================================

Subqueries can be present either in the ``FROM`` clause or in the
``WHERE`` clause of the query. This section explains how to use them in
the ``WHERE`` clause.

The :ref:`Syntax of the SELECT statement` (definition of
``<subselect condition>``) contains the definition of the operators you
can use to compare the output of a subquery.

The comparison conditions ``ALL``, ``ANY`` and ``IN`` a value to a list
or subquery. They must be preceded by ``<``, ``<=``, ``=``, ``<>``,
``>=``, ``>`` and followed by a subquery.

.. rubric:: Example 1

Obtain all the rows of the ``incident`` view whose ``taxid`` is *any* of
the ``taxid`` of the view ``flat_revenue`` that have a revenue greater
than 2,500.

The following two queries are equivalent. In the first one we use the
operator ``IN`` and in the other, we show how to use ``= ANY``.

.. code-block:: sql

   SELECT * FROM incident 
   WHERE taxid IN 
       (SELECT taxid
        FROM flat_revenue 
        WHERE revenue > 2500)
   
   SELECT * FROM incident 
   WHERE taxid = ANY
       (SELECT taxid
        FROM flat_revenue 
        WHERE revenue > 2500)

As in other databases, ``ANY`` can be used with other operators as well.
E.g, ``taxid > ANY``..., ``taxid < ANY``..., etc.

.. rubric:: Example 2

Obtain the rows of ``internet_inc`` whose id matches the id of a row of
``phone_inc``.

.. code-block:: sql
   :emphasize-lines: 3

   SELECT * 
   FROM internet_inc AS a
   WHERE EXISTS
       (SELECT b.PINC_ID 
        FROM PHONE_INC AS b 
        WHERE a.iinc_id = b.pinc_id)


In “Example 2” we are using the alias of the main query
(``internet_inc AS a``) in the ``WHERE`` clause of the subquery.

At runtime, the queries that contain subqueries are converted into
semijoins. A semijoin is a relational operation that returns all the
rows of the left-side query that have a matching row in the right-side
query. As with regular joins, the Server selects a semijoin method
depending on the query.

The available methods for executing semijoins of subqueries are the
following:

#. **Merge semijoin**: processes rows that are already sorted by the
   join attributes. Whenever possible, Virtual DataPort selects this
   algorithm because is almost always the most efficient and with the
   lowest memory footprint.
   
   This method can only be selected when both sides of the join are sorted by the join attributes. If the data from both sides is obtained from JDBC or ODBC data sources, Virtual DataPort will retrieve the data sorted by the fields of the join attributes. To do this, it adds the clause ORDER BY to the query sent to the database. 
   
#. **Hash semijoin**: the subquery is executed and its results are
   stored in a hash table. Then, the Server begins processing the
   results of the main query and looks for matches in the hash table.
   After merge semijoin, this is the most efficient algorithm, although
   not always can be used.
#. **Nested semijoin**: the subquery is executed once for each row of
   the main query’s results.

Virtual DataPort does not use statistics to choose the execution method
of the semijoins.

As with regular joins, the query can override the algorithm selected by
the Server to execute the subquery’s semijoin. To do this, add the
``SUBQUERYPLAN`` modifier to the ``CONTEXT`` clause of the subquery you
want to modify its query plan. The :ref:`Syntax of the CONTEXT clause`
contains the syntax of this modifier.

Note that the ``SUBQUERYPLAN`` modifier has to be indicated in the
``CONTEXT`` of the subquery you want to modify and not in the
``CONTEXT`` of the main query.

For example,

.. code-block:: sql

   SELECT * FROM incidents 
   WHERE taxid IN 
       (SELECT taxid
        FROM flat_revenue 
        WHERE revenue > 2500 CONTEXT (SUBQUERYPLAN = NESTED ORDERED)) 

In this example, the query forces the Server to use the algorithm nested
semijoin.
