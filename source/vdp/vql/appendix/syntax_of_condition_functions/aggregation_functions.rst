=====================
Aggregation Functions
=====================

Aggregation functions are used in ``SELECT`` statements to return one
single value for every group of tuples obtained as result of a grouping
operation.

These functions receive as a parameter an expression indicating the name
of the field to which it is applied. This parameter can optionally
be preceded by one of two modifiers: ``ALL`` or ``DISTINCT``. These
modifiers affect the semantics of certain aggregation functions,
applying them to all tuples in a group or only to those with a different
value for the field in question.

.. contents:: Aggregation functions supported by Virtual DataPort
   :depth: 1
   :local:
   :backlinks: none
   :class: threecols

AVG
=================================================================================

.. rubric:: Description

The ``AVG`` function returns the average of the non-null values of a
field of the table.

If all the values of the field are ``NULL``, the function returns
``NULL``.

.. rubric:: Syntax

.. code-block:: bnf

   AVG( <expression> ) : double

-  ``expression``. Required. The type of the expression has to be ``int``,
   ``long``, ``float``, ``double`` or ``decimal``.

.. rubric:: Examples

.. rubric:: Example 1

Consider the following view ``ITEMS``:



+--------------------------------------+--------------------------------------+
| item                                 | price                                |
+======================================+======================================+
| A                                    | 3.45                                 |
+--------------------------------------+--------------------------------------+
| B                                    | 9.99                                 |
+--------------------------------------+--------------------------------------+
| C                                    | 4.99                                 |
+--------------------------------------+--------------------------------------+





.. code-block:: sql

   SELECT AVG(price) AS average_price
   FROM ITEMS





+--------------------------------------------------------------------------+
| average\_price                                                           |
+==========================================================================+
| 6.1433333333333335                                                       |
+--------------------------------------------------------------------------+

.. rubric:: Example 2

Consider the following view ``ITEMS_2``:



+--------------------------------------+--------------------------------------+
| item                                 | price                                |
+======================================+======================================+
| A                                    | 3.45                                 |
+--------------------------------------+--------------------------------------+
| B                                    | 9.99                                 |
+--------------------------------------+--------------------------------------+
| C                                    | 4.99                                 |
+--------------------------------------+--------------------------------------+
| D                                    | NULL                                 |
+--------------------------------------+--------------------------------------+





.. code-block:: sql

   SELECT AVG(price) AS average_price
   FROM ITEMS_2





+--------------------------------------------------------------------------+
| average\_price                                                           |
+==========================================================================+
| 6.1433333333333335                                                       |
+--------------------------------------------------------------------------+


COUNT
=================================================================================

.. rubric:: Description

The ``COUNT`` function returns the number of tuples of the result of a
selection operation.

If the parameter is ``*``, it returns the number of tuples.

If the parameter is a field, it returns the number of non-null
values

It is applicable to any type of field.

This function can be used in queries without a ``GROUP BY`` clause, but
in that case, it can only be used with ``*``.

.. rubric:: Syntax

.. code-block:: bnf

   COUNT( <count parameter> )
   
   <count parameter> ::=
       <field name:identifier>
     | *
       
-  ``count parameter``. Required. It can be either a field name or ``*``.

.. rubric:: Examples

Consider the following view ``ITEMS``:



+--------------------------------------+--------------------------------------+
| item                                 | price                                |
+======================================+======================================+
| A                                    | 3.45                                 |
+--------------------------------------+--------------------------------------+
| B                                    | 9.99                                 |
+--------------------------------------+--------------------------------------+
| C                                    | 4.99                                 |
+--------------------------------------+--------------------------------------+
| D                                    | NULL                                 |
+--------------------------------------+--------------------------------------+

.. rubric:: Example 1



.. code-block:: sql

   SELECT COUNT(*)
   FROM items





+--------------------------------------------------------------------------+
| count                                                                    |
+==========================================================================+
| 4                                                                        |
+--------------------------------------------------------------------------+

.. rubric:: Example 2



.. code-block:: sql

   SELECT COUNT(price)
   FROM items





+--------------------------------------------------------------------------+
| count                                                                    |
+==========================================================================+
| 3                                                                        |
+--------------------------------------------------------------------------+


FIRST
=================================================================================

.. rubric:: Description

The ``FIRST`` function returns the first value of a field of each
group of values.

This function ignores the ``ALL``/``DISTINCT`` modifier.

.. rubric:: Syntax

.. code-block:: bnf

   FIRST ( <field name:identifier> )

-  ``field name``. Required. A field of the view.

.. rubric:: Examples

Consider the following view ``V``:



+--------------------------------------+--------------------------------------+
| A                                    | B                                    |
+======================================+======================================+
| group1                               | one                                  |
+--------------------------------------+--------------------------------------+
| group1                               | two                                  |
+--------------------------------------+--------------------------------------+
| group1                               | NULL                                 |
+--------------------------------------+--------------------------------------+
| group2                               | four                                 |
+--------------------------------------+--------------------------------------+

.. rubric:: Example 1



.. code-block:: sql

   SELECT FIRST(b)
   FROM v





+--------------------------------------------------------------------------+
| first                                                                    |
+==========================================================================+
| one                                                                      |
+--------------------------------------------------------------------------+

.. rubric:: Example 2



.. code-block:: sql

   SELECT a, FIRST(b)
   FROM v 
   GROUP BY a

+--------------------------------------+--------------------------------------+
| a                                    | first                                |
+======================================+======================================+
| group1                               | one                                  |
+--------------------------------------+--------------------------------------+
| group2                               | four                                 |
+--------------------------------------+--------------------------------------+


GROUP_CONCAT
=================================================================================

.. rubric:: Description

The ``GROUP_CONCAT`` function returns, for each group, a string with the
concatenation of all the field/fields values of each group.

.. rubric:: Syntax

.. code-block:: bnf

   GROUP_CONCAT( [ <row separator:text> [, <field separator:text> ] ], 
       <field name:identifier> [, <field name:identifier>]* )
   GROUP_CONCAT( <ignore null:boolean> , <row separator:text>, <field separator:text>, 
       <field name:identifier> [, <field name:identifier>]* )

-  ``ignore null``. Optional. If ``true`` and any of the fields
   ``field name`` of a row are ``NULL``, ``GROUP_CONCAT`` ignores all the
   fields of that row. If ``false``, no rows are ignored and ``NULL``
   values are treated as empty characters. The default value is
   ``true``.
-  ``row separator``. Optional. Literal used to separate the values of
   each row. Default value: ``,``.
-  ``field separator``. Optional. Literal used to separate the values of
   the fields of the same row. The default value is a whitespace.
-  ``field name``. Required. Field which contains the values to concatenate.

.. rubric:: Examples

Consider the following view ``V``:



+-------------------------+-------------------------+-------------------------+
| a                       | b                       | c                       |
+=========================+=========================+=========================+
| group1                  | 1                       | one                     |
+-------------------------+-------------------------+-------------------------+
| group1                  | 2                       | two                     |
+-------------------------+-------------------------+-------------------------+
| group1                  | NULL                    | three                   |
+-------------------------+-------------------------+-------------------------+
| group2                  | 4                       | four                    |
+-------------------------+-------------------------+-------------------------+

.. rubric:: Example 1



.. code-block:: sql

   SELECT A, GROUP_CONCAT(':', c)
   FROM v 
   GROUP BY a

+--------------------------------------+--------------------------------------+
| a                                    | group\_concat                        |
+======================================+======================================+
| group1                               | one:two:three                        |
+--------------------------------------+--------------------------------------+
| group2                               | four                                 |
+--------------------------------------+--------------------------------------+

.. rubric:: Example 2



.. code-block:: sql

   SELECT A, GROUP_CONCAT(':', ';', b, c)
   FROM v 
   GROUP BY a

+--------------------------------------+--------------------------------------+
| a                                    | group\_concat                        |
+======================================+======================================+
| group1                               | 1;one:2;two                          |
+--------------------------------------+--------------------------------------+
| group2                               | 4:four                               |
+--------------------------------------+--------------------------------------+

As the field ``B`` is ``NULL`` in the third row, ``GROUP_CONCAT``
ignores all the fields of that row. That is, it ignores the value
``three``.

.. rubric:: Example 3



.. code-block:: sql

   SELECT a, GROUP_CONCAT(false, ':', ';', b, c)
   FROM v 
   GROUP BY a


+--------------------------------------+--------------------------------------+
| a                                    | group\_concat                        |
+======================================+======================================+
| group1                               | 1;one:2;two:;three                   |
+--------------------------------------+--------------------------------------+
| group2                               | four                                 |
+--------------------------------------+--------------------------------------+

As the parameter ``ignoreNulls`` is ``false``, ``GROUP_CONCAT`` does not
ignore the value of the field ``C`` of the third row (``three``),
even if the value of the field ``B`` of that row is ``NULL``. In this
case, ``NULL`` values are treated like empty characters.


LAST
=================================================================================

.. rubric:: Description

The ``LAST`` function returns the last value of a field of each
group of values.

This function ignores the ``ALL``/``DISTINCT`` modifier.

.. rubric:: Syntax

.. code-block:: bnf

   LAST ( <field name:identifier> )

-  ``field name``. Required. Field name of the view.

.. rubric:: Examples

Consider the following view ``V``:

+--------------------------------------+--------------------------------------+
| a                                    | b                                    |
+======================================+======================================+
| group1                               | one                                  |
+--------------------------------------+--------------------------------------+
| group1                               | two                                  |
+--------------------------------------+--------------------------------------+
| group1                               | NULL                                 |
+--------------------------------------+--------------------------------------+
| group2                               | four                                 |
+--------------------------------------+--------------------------------------+

.. rubric:: Example 1



.. code-block:: sql

   SELECT LAST(b)
   FROM v

+--------------------------------------------------------------------------+
| last                                                                     |
+==========================================================================+
| four                                                                     |
+--------------------------------------------------------------------------+

.. rubric:: Example 2

.. code-block:: sql

   SELECT a, LAST(b)
   FROM v 
   GROUP BY a

+--------------------------------------+--------------------------------------+
| a                                    | last                                 |
+======================================+======================================+
| group1                               | NULL                                 |
+--------------------------------------+--------------------------------------+
| group2                               | four                                 |
+--------------------------------------+--------------------------------------+


LIST
=================================================================================

.. rubric:: Description

The ``LIST`` function returns an array with all the values of a
specified field.

It has the same behavior as the function ``NEST`` (section :ref:`NEST`) when
invoked with a single field as argument.

.. rubric:: Syntax

.. code-block:: bnf

   LIST ( <field name:identifier> )

-  ``field name``. Required. Field of the view.

.. rubric:: Examples

Consider the following view ``V``:



+--------------------------------------+--------------------------------------+
| a                                    | b                                    |
+======================================+======================================+
| group1                               | one                                  |
+--------------------------------------+--------------------------------------+
| group1                               | two                                  |
+--------------------------------------+--------------------------------------+
| group1                               | NULL                                 |
+--------------------------------------+--------------------------------------+
| group2                               | four                                 |
+--------------------------------------+--------------------------------------+

.. rubric:: Example 1



.. code-block:: sql

   SELECT LIST(b)
   FROM v





+--------------------------------------------------------------------------+
| list                                                                     |
+==========================================================================+
| Array { one, two, NULL, four }                                           |
+--------------------------------------------------------------------------+

.. rubric:: Example 2

.. code-block:: sql

   SELECT a, LIST(b)
   FROM v 
   GROUP BY a


+--------------------------------------+--------------------------------------+
| a                                    | list                                 |
+======================================+======================================+
| group1                               | Array { one, two, NULL }             |
+--------------------------------------+--------------------------------------+
| group2                               | Array { four }                       |
+--------------------------------------+--------------------------------------+


MAX
=================================================================================

.. rubric:: Description

The ``MAX`` function returns the highest value of a field for each
group of values.

This function ignores the ``ALL``/``DISTINCT`` modifier.

.. rubric:: Syntax

.. code-block:: bnf

   MAX ( <expression> )

-  ``expression``. Required. Expression of type ``date`` (deprecated type), ``intervaldaysecond``, ``intervalyearmonth``, ``text``, ``time``, ``timestamp``, ``timestamptz`` or any of the numeric data types.

The return type is the type of the input expression.

When the field is of text type, the function compares the Unicode value of each character of the value. 

.. rubric:: Examples

Consider the following view ``V``:


+--------------------------------------+--------------------------------------+--------------------------------------+
| a                                    | b                                    | c                                    |
+======================================+======================================+======================================+
| group1                               | 1                                    | Virtual DataPort                     |
+--------------------------------------+--------------------------------------+--------------------------------------+
| group1                               | 2                                    | Data Catalog                         |
+--------------------------------------+--------------------------------------+--------------------------------------+
| group1                               | NULL                                 | ITPilot                              |
+--------------------------------------+--------------------------------------+--------------------------------------+
| group2                               | 4                                    | denodo                               |
+--------------------------------------+--------------------------------------+--------------------------------------+

.. rubric:: Example 1

.. code-block:: sql

   SELECT MAX(b)
   FROM v


+--------------------------------------------------------------------------+
| max                                                                      |
+==========================================================================+
| 4                                                                        |
+--------------------------------------------------------------------------+

.. rubric:: Example 2



.. code-block:: sql

   SELECT a, MAX(b)
   FROM v 
   GROUP BY a


+--------------------------------------+--------------------------------------+
| a                                    | max                                  |
+======================================+======================================+
| group1                               | 2                                    |
+--------------------------------------+--------------------------------------+
| group2                               | 4                                    |
+--------------------------------------+--------------------------------------+


.. rubric:: Example 3

.. code-block:: sql

   SELECT MAX(c)
   FROM v


+--------------------------------------------------------------------------+
| max                                                                      |
+==========================================================================+
| denodo                                                                   |
+--------------------------------------------------------------------------+

In this example, the result is "denodo" because the first letter has the highest Unicode value of all the values: d = 100, V = 86, etc.

MEDIAN
=================================================================================

.. rubric:: Description

The ``MEDIAN`` function returns the middle number of a field for each
group of values, taken as the average of the two middle numbers when the group has an even number of values.

This function does not take into account ``NULL`` values to calculate
the result.

This function ignores the modifiers ``ALL`` and ``DISTINCT``.

.. rubric:: Syntax

.. code-block:: bnf

   MEDIAN ( <expression> )

-  ``expression``. Required. Expression of type ``date`` (deprecated type) , ``time``, ``timestamp``, ``timestamptz`` or any of the numeric data types.

.. rubric:: Examples

Consider the following view ``employee``:

+-----+------------+-------------+-------------------+
| id  | first_name | start_date  | salary            |
+=====+============+=============+===================+
| 01  |Jason       | 25-JUL-96   | 1234.56           |
+-----+------------+-------------+-------------------+
| 02  |Alison      | 21-MAR-76   | 6661.78           |
+-----+------------+-------------+-------------------+
| 03  |James       | 12-DEC-78   | 6544.78           |
+-----+------------+-------------+-------------------+
| 04  |Celia       | 24-OCT-82   | 2344.78           |
+-----+------------+-------------+-------------------+
| 05  |Robert      | 24-OCT-82   | 2334.78           |
+-----+------------+-------------+-------------------+
| 05  |Jason       | 30-JUL-87   | 2224.50           |
+-----+------------+-------------+-------------------+

.. rubric:: Example 1

.. code-block:: sql

   SELECT MEDIAN(salary), MEDIAN(start_date)
   FROM employee

+-----------------+--------------------+
| median(salary)  | median(start_date) |
+=================+====================+
| 2339.78         |1983-06-05          |
+-----------------+--------------------+

.. rubric:: Example 2

.. code-block:: sql

   SELECT first_name, MEDIAN(salary), MEDIAN(start_date)
   FROM employee
   GROUP BY first_name

+------------+-----------------+--------------------+
| first_name | median(salary)  | median(start_date) |
+============+=================+====================+
| Alison     | 6661.78         | 1976-03-21         |
+------------+-----------------+--------------------+
| Celia      | 2344.78         | 1982-10-24         |
+------------+-----------------+--------------------+
| James      | 6544.78         | 1978-12-12         |
+------------+-----------------+--------------------+
| Jason      | 1729.53         | 1992-01-26         |
+------------+-----------------+--------------------+
| Robert     | 2334.78         | 1984-01-15         |
+------------+-----------------+--------------------+


MIN
=================================================================================

.. rubric:: Description

The ``MIN`` function returns the lowest value of a field for each
group of values.

This function ignores the ``ALL``/``DISTINCT`` modifier.

.. rubric:: Syntax

.. code-block:: bnf

   MIN ( <expression> )

-  ``expression``. Required. Expression of type ``date`` (deprecated type), ``intervaldaysecond``, ``intervalyearmonth``, ``text``, ``time``, ``timestamp``, ``timestamptz`` or any of the numeric data types.

When the field is of text type, the function compares the Unicode value of each character of the value. 

.. rubric:: Examples

Consider the following view ``V``:

+--------------------------------------+--------------------------------------+--------------------------------------+
| a                                    | b                                    | c                                    |
+======================================+======================================+======================================+
| group1                               | 1                                    | Virtual DataPort                     |
+--------------------------------------+--------------------------------------+--------------------------------------+
| group1                               | 2                                    | Data Catalog                         |
+--------------------------------------+--------------------------------------+--------------------------------------+
| group1                               | NULL                                 | ITPilot                              |
+--------------------------------------+--------------------------------------+--------------------------------------+
| group2                               | 4                                    | denodo                               |
+--------------------------------------+--------------------------------------+--------------------------------------+

.. rubric:: Example 1

.. code-block:: sql

   SELECT MIN(b)
   FROM V

+--------------------------------------------------------------------------+
| min                                                                      |
+==========================================================================+
| 1                                                                        |
+--------------------------------------------------------------------------+

.. rubric:: Example 2

.. code-block:: sql

   SELECT a, MIN(b)
   FROM v 
   GROUP BY a

+--------------------------------------+--------------------------------------+
| a                                    | min                                  |
+======================================+======================================+
| group1                               | 1                                    |
+--------------------------------------+--------------------------------------+
| group2                               | 4                                    |
+--------------------------------------+--------------------------------------+

.. rubric:: Example 3

.. code-block:: sql

   SELECT MIN(c)
   FROM v


+--------------------------------------------------------------------------+
| min                                                                      |
+==========================================================================+
| Data Catalog                                                             |
+--------------------------------------------------------------------------+

In this example, the result is "Data Catalog" because the first letter has the lowest Unicode value of all the values: D = 68, d = 100, V = 86, etc.

NEST
=================================================================================

.. rubric:: Description

The ``NEST`` function returns an array with the values of the selected
fields. Its result is inverse to the result of the FLATTEN views (see
section :ref:`FLATTEN View (Flattening Data Structures)` for more
information about FLATTEN views).

.. rubric:: Syntax

.. code-block:: bnf

   NEST( <field name:identifier> [, <field name:identifier> ]*):array
   NEST(*)

-  ``field name``. The name of a field. Using ``(*)`` is equivalent to
   pass all the fields of the view to the function.

.. rubric:: Example

Consider the view ``V``:

+-------------------------+-------------------------+-------------------------+
| int\_sample             | text\_sample            | register\_sample        |
+=========================+=========================+=========================+
| 1                       | A                       | Register { hello ,      |
|                         |                         | how're you }            |
+-------------------------+-------------------------+-------------------------+
| 1                       | B                       | Register { hello, good  |
|                         |                         | bye }                   |
+-------------------------+-------------------------+-------------------------+
| 2                       | C                       | Register { another      |
|                         |                         | string, last string }   |
+-------------------------+-------------------------+-------------------------+

.. code-block:: sql

   SELECT int_sample, NEST(text_sample, register_sample) AS nest_sample
   FROM V
   GROUP BY int_sample;

+--------------------------------------+--------------------------------------+
| int\_sample                          | nest\_sample                         |
+======================================+======================================+
| 1                                    | Array [ A, Register { hello , how're |
|                                      | you }                                |
|                                      |                                      |
|                                      | B, Register { hello, good bye }      |
|                                      |                                      |
|                                      | ]                                    |
+--------------------------------------+--------------------------------------+
| 2                                    | Array [ C, Register { another        |
|                                      | string, last string } ]              |
+--------------------------------------+--------------------------------------+


STDEV
=================================================================================

.. rubric:: Description

The ``STDEV`` function returns the sample standard deviation of each
group of values.

This function does not take into account ``NULL`` values to calculate
the result.

.. rubric:: Syntax

.. code-block:: bnf

   STDEV( <expression> ) : decimal

-  ``expression``. Required. Any numeric data type.

.. rubric:: Example

Consider the view ``sales_by_region``:

+-------------------------+-------------------------+-------------------------+
| region                  | state                   | revenue                 |
+=========================+=========================+=========================+
| South                   | Alabama                 | 53168                   |
+-------------------------+-------------------------+-------------------------+
| South                   | Missisipi               | 5681                    |
+-------------------------+-------------------------+-------------------------+
| South                   | Tennessee               | 80166                   |
+-------------------------+-------------------------+-------------------------+
| Northeast               | New York                | 12945                   |
+-------------------------+-------------------------+-------------------------+
| Northeast               | Pennsylvania            | 69284                   |
+-------------------------+-------------------------+-------------------------+
| Northeast               | New Hampshire           | 53168                   |
+-------------------------+-------------------------+-------------------------+

.. code-block:: sql

   SELECT region, STDEV(revenue)
   FROM revenue
   GROUP BY region

+--------------------------------------+--------------------------------------+
| region                               | stdev                                |
+======================================+======================================+
| South                                | 37709.24377832752                    |
+--------------------------------------+--------------------------------------+
| Northeast                            | 29016.36924794922                    |
+--------------------------------------+--------------------------------------+


STDEVP
=================================================================================

.. rubric:: Description

The ``STDEVP`` function returns the population standard deviation of
each group of values.

This function does not take into account ``NULL`` values to calculate
the result.

.. rubric:: Syntax

.. code-block:: bnf

   STDEVP( <expression> ) : decimal

-  ``expression``. Required. Any numeric data type.


.. rubric:: Example

Consider the view ``sales_by_region``:


+-------------------------+-------------------------+-------------------------+
| region                  | state                   | revenue                 |
+=========================+=========================+=========================+
| South                   | Alabama                 | 53168                   |
+-------------------------+-------------------------+-------------------------+
| South                   | Missisipi               | 5681                    |
+-------------------------+-------------------------+-------------------------+
| South                   | Tennessee               | 80166                   |
+-------------------------+-------------------------+-------------------------+
| Northeast               | New York                | 12945                   |
+-------------------------+-------------------------+-------------------------+
| Northeast               | Pennsylvania            | 69284                   |
+-------------------------+-------------------------+-------------------------+
| Northeast               | New Hampshire           | 53168                   |
+-------------------------+-------------------------+-------------------------+


.. code-block:: sql

   SELECT region, STDEVP(revenue)
   FROM revenue
   GROUP BY region


+--------------------------------------+--------------------------------------+
| region                               | stdevp                               |
+======================================+======================================+
| South                                | 30789.46861437455                    |
+--------------------------------------+--------------------------------------+
| Northeast                            | 23691.76628188695                    |
+--------------------------------------+--------------------------------------+


SUM
=================================================================================

.. rubric:: Description

The ``SUM`` function returns the sum of all non-null values of a
field for each group of values.

.. rubric:: Syntax

.. code-block:: bnf

   SUM ( <expression> )

-  ``expression``. Required. Any numeric data type.

The return type is the type of the input expression.

.. rubric:: Examples

Consider the following view ``V``:

+--------------------------------------+--------------------------------------+
| a                                    | b                                    |
+======================================+======================================+
| group1                               | 1                                    |
+--------------------------------------+--------------------------------------+
| group1                               | 2                                    |
+--------------------------------------+--------------------------------------+
| group1                               | NULL                                 |
+--------------------------------------+--------------------------------------+
| group2                               | 4                                    |
+--------------------------------------+--------------------------------------+

.. rubric:: Example 1

.. code-block:: sql

   SELECT SUM(b)
   FROM v
   
+--------------------------------------------------------------------------+
| sum                                                                      |
+==========================================================================+
| 7                                                                        |
+--------------------------------------------------------------------------+

.. rubric:: Example 2

.. code-block:: sql

   SELECT a, SUM(b)
   FROM v 
   GROUP BY a

+--------------------------------------+--------------------------------------+
| a                                    | sum                                  |
+======================================+======================================+
| group1                               | 3                                    |
+--------------------------------------+--------------------------------------+
| group2                               | 4                                    |
+--------------------------------------+--------------------------------------+


VAR
=================================================================================

.. rubric:: Description

The ``VAR`` function returns the sample variance of each group of
values.

This function does not take into account ``NULL`` values to calculate
the result.

.. rubric:: Syntax

.. code-block:: bnf

   VAR( <expression> ) : decimal

-  ``expression``. Required. Any numeric data type.

.. rubric:: Example

Consider the view ``sales_by_region``:

+-------------------------+-------------------------+-------------------------+
| region                  | state                   | revenue                 |
+=========================+=========================+=========================+
| South                   | Alabama                 | 53168                   |
+-------------------------+-------------------------+-------------------------+
| South                   | Missisipi               | 5681                    |
+-------------------------+-------------------------+-------------------------+
| South                   | Tennessee               | 80166                   |
+-------------------------+-------------------------+-------------------------+
| Northeast               | New York                | 12945                   |
+-------------------------+-------------------------+-------------------------+
| Northeast               | Pennsylvania            | 69284                   |
+-------------------------+-------------------------+-------------------------+
| Northeast               | New Hampshire           | 53168                   |
+-------------------------+-------------------------+-------------------------+

.. code-block:: sql

   SELECT region, VAR(revenue)
   FROM revenue
   GROUP BY region

+--------------------------------------+--------------------------------------+
| region                               | var                                  |
+======================================+======================================+
| South                                | 1.4219870663333333E9                 |
+--------------------------------------+--------------------------------------+
| Northeast                            | 8.419496843333333E8                  |
+--------------------------------------+--------------------------------------+


VARP
=================================================================================

.. rubric:: Description

The ``VARP`` function returns the population variance of each group of
values.

This function does not take into account ``NULL`` values to calculate
the result.

.. rubric:: Syntax

.. code-block:: bnf

   VARP( <expression> ) : double

-  ``expression``. Required. Any numeric data type.

.. rubric:: Example

Consider the view ``sales_by_region``:

+-------------------------+-------------------------+-------------------------+
| region                  | state                   | revenue                 |
+=========================+=========================+=========================+
| South                   | Alabama                 | 53168                   |
+-------------------------+-------------------------+-------------------------+
| South                   | Missisipi               | 5681                    |
+-------------------------+-------------------------+-------------------------+
| South                   | Tennessee               | 80166                   |
+-------------------------+-------------------------+-------------------------+
| Northeast               | New York                | 12945                   |
+-------------------------+-------------------------+-------------------------+
| Northeast               | Pennsylvania            | 69284                   |
+-------------------------+-------------------------+-------------------------+
| Northeast               | New Hampshire           | 53168                   |
+-------------------------+-------------------------+-------------------------+

.. code-block:: sql

   SELECT region, VARP(revenue)
   FROM revenue
   GROUP BY region

+--------------------------------------+--------------------------------------+
| region                               | varp                                 |
+======================================+======================================+
| South                                | 9.479913775555555E8                  |
+--------------------------------------+--------------------------------------+
| Northeast                            | 5.612997895555555E8                  |
+--------------------------------------+--------------------------------------+
