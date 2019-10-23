====================================
Data Source Configuration Properties
====================================

The configuration properties of the data sources, also called *source
configuration*, specify certain characteristics of the underlying
sources such as the operations they support. Knowing the capabilities of
the data sources is important for optimization reasons since it allows
Virtual DataPort to delegate to the data source as much processing as
possible to optimize response times and minimize traffic through the
network.

.. note:: Usually, users do not need to edit these settings since
   Virtual DataPort automatically uses suitable configurations for most
   common data sources.

The properties of each data source can be configured by opening the data
source, clicking on **Edit** and then, on **Source configuration**.

Each property has a default value (marked with *(default)* next to its
value). The default value is different depending on the database and it
reflects the capabilities of each source.

The configurable properties are the following:

-  **Allow FOR UPDATE clause**. If *yes*, it indicates that the source
   supports the ``SELECT... FOR UPDATE`` clause.
-  **Allow literal as parameter**. If *yes*, it indicates that the
   source allows the literals to be indicated as parameters of the
   prepared statement created to execute the view. If *no*, the
   Server generates the query with the literals in it.
-  **Delegate all operators**. If *yes*, it indicates that the source
   allows all operators to be delegated.
-  **Delegate array literal**. If *yes*, it indicates that the
   source allows array compound constants to be delegated.
-  **Delegate compound field projection**. If *yes*, it indicates that
   the source allows projections on compound fields to be delegated.
-  **Delegate FETCH clause**. If *yes*, the ``FETCH`` clause of the
   queries can be delegated to the source.
-  **Delegate OFFSET clause**. If *yes*, the ``OFFSET`` clause of the
   queries can be delegated to the source.
-  **Delegate OFFSET clause restriction**. It indicates if this database has any limitations regarding the use of the clause ``OFFSET`` in the queries.

   The possible values of this property are:
   
   -  **NONE**: the database does not have any restriction regarding ``OFFSET``.
   -  **FETCH**: the database only supports ``OFFSET`` if the query also has ``FETCH``.
   -  **ORDER_BY**: the database only supports ``OFFSET`` if the query also has ``ORDER BY``.
   -  **FETCH_ORDER_BY**: the database only supports ``OFFSET`` if the query also has the
      ``FETCH`` and ``ORDER BY`` clauses.
   -  **NO_ORDER_BY**: the database only supports ``OFFSET`` if the query does not have ``ORDER BY``.
   -  **FETCH_NO_ORDER_BY**: the database only supports ``OFFSET`` if the query also has ``FETCH`` but it does not have ``ORDER BY``.

   If a user executes a query that involves delegating a query to this database and these restrictions are not being met, the Execution Engine will not delegate the clause ``OFFSET`` to the database and it will be performed by the Execution Engine.

   This property is ignored if *Delegate OFFSET clause* is set to *no*.

-  **Delegate GROUP BY clause**. If *yes*, it indicates that the source
   allows the ``GROUP BY`` clause to be delegated.
-  **Delegate HAVING clause**. If *yes*, it indicates that the
   ``HAVING`` clause of the queries can be delegated to the source.
-  **Delegate INNER JOIN**. If *yes*, it indicates that the
   ``INNER JOIN`` operator can be delegated to the source.
-  **Delegate INTERSECT**. If *yes*, it indicates that the
   ``INTERSECT`` operator can be delegated to the source.
-  **Delegate invalid number literals as NULL**. If *yes*, the Server
   checks that the operands of the conditions delegated to the source
   have compatible types.

   For example, let us say that we have a view ``V1`` with an ``int``
   field ``f1`` and that, at the source, the type of this field is
   ``NUMERIC``.

   If the property is *no*, the query
   ``SELECT * FROM v1 where f1 = '4c'`` will be delegated to the source
   and probably fail.

   If the property is set to *yes*, the Server will detect that ``f1``
   and ``'4c'`` are incompatible and will delegate ``NULL`` instead:
   ``SELECT * FROM v1 where f1 = NULL``

   This enhancement only works in conditions with the operators ``=``,
   ``<>``, ``<``, ``>``, ``<=``, ``>=``, ``in`` and ``between``.
-  **Delegate JOIN**. If *yes*, it indicates that the ``JOIN`` operator
   can be delegated to the source.
-  **Delegate left function**. If *yes*, it indicates that the
   conditions with functions on the left part can be delegated to the
   source.
-  **Delegate left literal**. If *yes*, it indicates that the conditions
   with constants on the left part can be delegated to the source.
-  **Delegate literal expression**. If *yes*, it indicates that literal expressions can be delegated to the source.   
-  **Delegate MINUS**. If *yes*, it indicates that the
   ``MINUS`` operator can be delegated to the source.
-  **Delegate mixed aggregate expression**. If *yes*, it indicates that the aggregation expressions delegated to the database can include scalar functions, literals and fields. If *no*, the aggregation function will only include fields but not expressions.
-  **Delegate natural OUTER JOIN**. If *yes*, it indicates that the
   natural ``OUTER JOIN`` operator can be delegated to the source.
-  **Delegate NOT condition**. If *yes*, it indicates that the ``NOT``
   conditions can be delegated to the source.
-  **Delegate OR condition**. If *yes*, it indicates that the ``OR``
   conditions can be delegated to the source.
-  **Delegate ORDER BY**. If *yes*, it indicates that the ``ORDER BY``
   clause can be delegated to the source.
-  **Delegate projection**. If *yes*, it indicates that projections can
   be delegated to the source.
-  **Delegate register literal**. If *yes*, it indicates that register
   constants can be delegated to the source.
-  **Delegate right field**. If *yes*, it indicates that the conditions
   with fields on the right part can be delegated to the source.
-  **Delegate right function**. If *yes*, it indicates that the
   conditions with functions on the right part can be delegated to the
   source.
-  **Delegate right literal**. If *yes*, it indicates that the
   conditions with constants on the right part can be delegated to the
   source.
-  **Delegate SELECT DISTINCT**. If *yes*, it indicates that modifier ``DISTINCT`` of the clause ``SELECT`` can be delegated to the source.   
-  **Delegate selection**. If *yes*, it indicates that the conditions
   can be delegated to the source.
-  **Delegate subquery**. If *yes*, it indicates that the source can
   process queries with subqueries in them.
-  **Delegate UNION**. If *yes*, it indicates that the source allows
   ``UNION`` operators to be delegated.
-  **Nested join optimization syntax**. This property controls some
   aspects of the query that is delegated to the database, when the data
   of the view in the right side of a Nested join is obtained from this
   data source. See more about this in the section :ref:`Nested Join`.
-  **Supports modifier in aggregate function**. If *yes*, it indicates
   that the source supports the ``DISTINCT``/``ALL`` modifiers in
   aggregate functions.
-  **Supports batch inserts**. If *yes*, it indicates that the database
   supports processing ``INSERT`` requests in batches.
   Virtual DataPort inserts rows in batches when moving data from
   another data source into this one. See more about Data Movement in
   the section :ref:`Automatic Simplification of Queries`.

   This property does not affect ``INSERT`` requests sent to this data
   source because they are not executed in batches.

-  **Supports branch OUTER JOIN**. If *yes*, it indicates that the
   source supports ``LEFT OUTER JOIN`` and ``RIGHT OUTER JOIN``.

-  **Supports CLOBs in batch inserts**. If *yes*, it indicates that the
   database supports processing ``INSERT`` requests in batch, when one
   the type of one of the values is ``CLOB``.

   If *no*, the ``INSERT`` requests that contain a value of type
   ``CLOB`` will be executed one by one, in the same transaction.
   Virtual DataPort inserts rows in batches when moving data from
   another data source into this one. See more about Data Movement in
   the section :ref:`Automatic Simplification of Queries`.

   This property does not affect ``INSERT`` requests sent to this data
   source because they are not executed in batches.

   This property is ignored when “Supports batch inserts” is *no*.

-  **Supports Eq OUTER JOIN**. If *yes*, it indicates that the
   ``OUTER JOIN`` operator can be delegated to the source.

-  **Supports explicit CROSS JOIN**. If *yes*, it indicates that the
   explicit ``CROSS JOIN`` operator can be delegated to the source.

-  **Supports GROUP BY literals as parameters**. If *yes*, it indicates
   that the source allows the literals of the ``GROUP BY`` clause to be
   indicated as parameters of the prepared statement created to
   execute the query. Otherwise, the Server generates the query with the
   literals in it.

-  **Supports ORDER BY expressions**. If *yes*, this database
   supports executing queries with expressions in the ``ORDER BY`` clause.

-  **Supports right deep n-joins**. If *yes*, the query delegated to the source may have right deep n-joins with all the ON conditions at the end. If *no*, the query will have a subquery for each of the n-joins of the query sent by the client to Virtual DataPort.

-  **Supports full Eq OUTER JOIN**. If *yes*, it indicates that the
   source allows the full equality ``OUTER JOIN`` operator to be
   delegated.

-  **Supports full NotEq OUTER JOIN**. If *yes*, it indicates that the
   Full Not Equality ``OUTER JOIN`` operator can be delegated.

-  **Supports fusing in USING and natural JOIN**. If *yes*, it indicates
   that the source merges the same fields when running a natural
   ``JOIN`` or a ``JOIN`` with the ``USING`` clause.

-  **Supports JOIN ON condition**. If *yes*, it indicates that the
   ``JOIN...ON`` clause can be delegated to the source.

-  **Supports natural JOIN**. If *yes*, it indicates that the natural
   ``JOIN`` clause can be delegated to the source.

-  **Supports USING JOIN**. If *yes*, it indicates that the
   ``USING JOIN`` clause can be delegated to the source.
   See “Example 1” below.

-  **Supports binary ORDER BY collation**. If *yes*, the source executes
   ``ORDER BY`` operations using a binary collation.
   See section :ref:`ORDER BY Properties of the Source Configuration` for
   more details about this.

-  **Supports PreparedStatement** (only available for data sources that use the *Generic* adapter). If *yes*, the data source will execute queries using a prepared statement. 
   If *no*, the data source will execute queries with regular statements. When setting this to *no* from the administration tool, the property *Allow literal as parameter*
   is automatically set to *no*. The reason is that regular statements cannot be parameterized. Default value: *yes*.

-  **Supports ORDER BY collation modifier**. If ``yes``, the source has
   support to indicate a collation modifier in the query.
   See section :ref:`ORDER BY Properties of the Source Configuration` for
   more details about this.

-  **Delegate binary ORDER BY collation**.

-  **Delegate aggregate functions list**. List of aggregation functions
   that can be delegated.

-  **Delegate scalar functions list**. List of scalar functions that can
   be delegated.

-  **Delegate operators list**. List of operators that can be delegated.
   The default list has the following operators: ``=``, ``<>``, ``<``,
   ``<=``, ``>``, ``>=``, ``between``, ``containsand``, ``containsor``,
   ``exists``, ``in``, ``is false``, ``is null``, ``is not null``,
   ``is true``, ``like`` and ``notin``.

-  **Block size**. It indicates the amount of data that the data source
   reads or writes in a single random I/O operation.

-  **Multi block read count**. It indicates how many consecutive blocks
   a database reads on a single I/O operation.

   The value of the parameters “Block size” and “Multi block read count” is used for the cost-based optimization.
   The section :ref:`Data Source I/O Parameters` provides more details about these parameters and how they are used.

ORDER BY Properties of the Source Configuration
=================================================================================

To perform ORDER BY operations over fields of type “text”, Virtual
DataPort uses a “binary” collation to compare the text values of the
result set and sort them. Binary collations compare strings using the
Unicode value of each character.

When Virtual DataPort pushes down an ORDER BY to a database to be able
to perform a merge join, the database has to perform the ORDER BY using
a binary collation as well. The reason is that the Execution Engine
expects the rows to be sorted using a binary collation. If they were
sorted with a different collation, the results may be incorrect when the
join conditions involve fields of type text.

The properties “Delegate ORDER BY collation modifier” and “Delegate
binary ORDER BY collation” control how Virtual DataPort pushes down the
ORDER BY clause to databases.

.. note::
   This section explains how the following properties affect the
   behavior of Virtual DataPort. However, *very rarely* you will need to
   modify their default value.

#. **Supports binary ORDER BY collation**: the default value is *yes* for
   databases that meet one of the following conditions, when executing an
   ORDER BY over fields of type text:

   a. By default, they use a binary collation to sort the data.
   b. Or, they support forcing a binary collation to perform the ORDER BY.

   When the Execution Engine selects a method to execute a join whose
   conditions involve fields of type text, it selects the method merge if
   the property “Supports binary ORDER BY collation” is *yes* in all the
   sources involved in the query. In that case, the Execution Engine adds
   the clause ``ORDER BY`` to the query pushed down to the databases.

   If the property “Supports binary ORDER BY collation” is *no* in at least
   one data source involved in the query, the Execution Engine does not
   select the merge method to perform the join. The reason is that Virtual
   DataPort needs to obtain the data from the database sorted with a binary
   collation.

   If the default value of this property is *no*, do not set it to *yes*.
   If the default value is *no*, it means that the source is not capable of
   sorting the data using a binary collation.

#. **Delegate ORDER BY collation modifier**: if *yes*, the ``ORDER BY``
   clause is pushed down with a collation modifier. If *no*, it is pushed
   down without any modifier.

   For instance, by default, the clause ``ORDER BY <field of type text>``
   is pushed down to Oracle with the modifier ``NLSSORT``. E.g.,

   .. code-block:: sql

      SELECT ...
      FROM ...
      ORDER BY NLSSORT( <field of type text>, 'NLS_SORT = binary') ASC

   If this property is *no*, ``ORDER BY`` is pushed down without this
   modifier. E.g.,

   .. code-block:: sql

      SELECT ...
      FROM ...
      ORDER BY column1 ASC, column2 ASC

   If the default value of this property is *yes*, setting it to *no* may
   lead merge joins that obtain data from this source to return incorrect
   results. The reason is that the merge join algorithm expects the input
   data to be sorted with a binary collation.

   Only set this property to *no* if the collation modifier is hurting the
   performance of the query and the collation used by the database sorts
   the data in the same way as the collation that Virtual DataPort tries to
   use when this property is *yes*.

   Do not set this property to *yes* if its default value *no*. If the
   default value is *no*, it means that the source is not capable of
   sorting the data using a binary collation.


#. **Delegate binary ORDER BY collation**: for JDBC data sources that have
   a default value for this property, you can change it. However, the
   default collation set for each adapter performs a binary collation, so
   you should not modify it.

   If this property does not have a default value, setting a value for this
   property does not have any effect.


The default value of these three properties is different depending on
the database adapter of the JDBC data source.

These properties only affect queries with an ``ORDER BY`` of text
fields. When sorting by other types of values, they are not important
because there are not different ways of sorting ``long`` or ``int``
values for example.
