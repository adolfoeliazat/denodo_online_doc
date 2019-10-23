===============
ORDER BY Clause
===============

The ``ORDER BY`` clause sorts the data returned by a query, by the
specified column list.



.. code-block:: bnf
   :caption: Syntax of the ORDER BY clause (SELECT statement)
   :name: Syntax of the ORDER BY clause (SELECT statement)

   <order by> ::= 
     ORDER BY <order by expression> [ ASC | DESC ]
     [, <order by expression> [ ASC | DESC ] ]*
   
   <order by expression> ::=
       <field name>
     | <expression>
     | <field position:integer>

Usually, Virtual DataPort processes the data obtained from the sources
asynchronously. I.e. it can begin sending results to clients without
having to wait to process all the data obtained from the sources.
However, if the query uses ``ORDER BY`` and this cannot be pushed down
to the source, the result of the query is generated synchronously. I.e.
the results are not available until all the data from the sources has
been obtained.

In the ``ORDER BY`` clause of a query you can indicate:

-  A name of a field or the alias of a field projected by the query
   (i.e. in the ``SELECT`` clause of the query).
-  An expression based on the name of the fields or aliases of the
   projected fields.
-  The position of the fields in the ``SELECT`` clause by which you want
   to sort the result.

The ``ASC`` and ``DESC`` modifiers are optional. If missing, the results
are sorted in an ascending order.

The following query obtains the employees ordered according to the
attribute ``pay`` in a descending order.

.. code-block:: sql

   SELECT * 
   FROM emp 
   ORDER BY pay DESC;

In the following query, the order by field is indicated with its
position in the ``SELECT`` clause:

.. code-block:: sql

   SELECT name, pay 
   FROM emp 
   ORDER BY 2 DESC;

The index of the first field is 1.

The following query uses an expression in the ``ORDER BY`` clause:

.. code-block:: sql

   SELECT * 
   FROM internet_inc
   ORDER BY lower(summary)
