===============
Other Functions
===============


.. contents:: Other functions supported by Virtual DataPort
   :depth: 1
   :local:
   :backlinks: none
   :class: threecols

COALESCE
=================================================================================

.. rubric:: Description

The ``COALESCE`` function returns the first non-null argument.
``COALESCE`` is equivalent to the expression:

.. code-block:: vql

   CASE WHEN arg1 IS NOT NULL THEN arg1
        WHEN arg2 IS NOT NULL THEN arg2
        …   
   END

.. rubric:: Syntax

.. code-block:: bnf

   COALESCE( <field name:identifier>, <field name:identifier> [, <field name:identifier> ]*)

-  ``field name``. Text or field name which can be NULL.

.. rubric:: Examples

Consider the view called ``V``:

+-------------------------+-------------------------+-------------------------+
| a                       | b                       | c                       |
+=========================+=========================+=========================+
| 10.10                   | I am some text          | 21-ene-2005 0h 0m 0s    |
+-------------------------+-------------------------+-------------------------+
| -80.10                  | Text is $% needed       | 12-mar-2005 12h 30m 0s  |
|                         | always                  |                         |
+-------------------------+-------------------------+-------------------------+
| 20.50                   | Text for a living       | 01-feb-2006 16h 45m 0s  |
+-------------------------+-------------------------+-------------------------+
| 40.05                   | NULL                    | NULL                    |
+-------------------------+-------------------------+-------------------------+

.. rubric:: Example 1

.. code-block:: sql

   SELECT COALESCE(b, 'hello')
   FROM V

+--------------------------------------------------------------------------+
| coalesce                                                                 |
+==========================================================================+ 
| I am some text                                                           |
+--------------------------------------------------------------------------+
| Text is $% needed always                                                 |
+--------------------------------------------------------------------------+
| Text for a living                                                        |
+--------------------------------------------------------------------------+
| hello                                                                    |
+--------------------------------------------------------------------------+

.. rubric:: Example 2



.. code-block:: sql

   SELECT coalesce('hello', 'bye')
   FROM v

+--------------------------------------------------------------------------+
| coalesce                                                                 |
+==========================================================================+
| hello                                                                    |
+--------------------------------------------------------------------------+
| hello                                                                    |
+--------------------------------------------------------------------------+
| hello                                                                    |
+--------------------------------------------------------------------------+
| hello                                                                    |
+--------------------------------------------------------------------------+

.. rubric:: Example 3



.. code-block:: sql

   SELECT COALESCE(b, a)
   FROM V





+--------------------------------------------------------------------------+
| coalesce                                                                 |
+==========================================================================+
| I am some text                                                           |
+--------------------------------------------------------------------------+
| Text is $% needed always                                                 |
+--------------------------------------------------------------------------+
| Text for a living                                                        |
+--------------------------------------------------------------------------+
| 40.05                                                                    |
+--------------------------------------------------------------------------+


CONTEXTUALSUMMARY
=================================================================================

.. rubric:: Description

The ``CONTEXTUALSUMMARY`` function returns relevant text fragments of a
text, containing the word or sentence specified.

.. rubric:: Syntax

.. code-block:: bnf

   CONTEXTUALSUMMARY( <content:text>, <keyword:text>, [ <begin delim:text>,
       <end delim:text>, <fragment separator:text>, <fragment length:int>
       [, <max fragments number:int> [, <analyzer:text> ] ] ] )

-  ``content``: Required. Text that the most relevant fragments are to
   be extracted from.
-  ``keyword``: Required. Keyword used to extract the text fragments.
   The content of this argument can be a single word or a sentence.
-  ``begin delim``: Optional. Text to add as prefix of the keyword
   whenever it appears in the text. Default value is “”.
-  ``end delim``: Optional. Text to add as suffix of the keyword whenever
   it appears in the text. Default value is “”.
-  ``fragment separator``: Optional. Text to separate each text fragment
   of the result. Default value is “…”.
-  ``fragment length``: Optional. Approximate number of characters that
   will appear before and after the keyword occurrences inside of the
   text. Default value is 5.
-  ``max fragment number``: Optional. Maximum number of fragments to
   retrieve.
-  ``analyzer``: Optional. Analyzer used to search for keywords. By
   default, the Standard Analyzer (``std``) is used. This analyzer does
   not consider lemmatization or stopwords. Virtual DataPort also
   includes analyzers for English (``en``) and Spanish (``es``).

.. rubric:: Examples

.. rubric:: Example 1

.. code-block:: sql

   SELECT CONTEXTUALSUMMARY(content, 'Denodo', '<b>', '</b>', ' … ', 5, 1)
   FROM demo_arn_view;
   
This query will return fragments of text ``content`` where the “Denodo”
word appears.

.. rubric:: Example 2

Consider the following view ``text_summary_sample``:

+--------------------------------------------------------------------------+
| text\_sample                                                             |
+==========================================================================+
| A web service (also webservice) is defined by the W3C as a software      |
| system designed to support interoperable machine-to-machine interaction  |
| over a network. It has an interface described in a machine-processable   |
| format (specifically Web Services Description Language WSDL). Other      |
| systems interact with the web service in a manner                        |
+--------------------------------------------------------------------------+
| prescribed by its description using SOAP messages, typically conveyed    |
| using HTTP with an XML serialization in conjunction with other           |
| web-related standards.Web services are frequently just Internet          |
| Application Programming Interfaces (API) that can be accessed over a     |
| network, such as the Internet, and executed on a remote system hosting   |
| the requested services. Other approaches with nearly the same            |
| functionality                                                            |
+--------------------------------------------------------------------------+
| as web services are Object Management Group’s (OMG) Common Object        |
| Request                                                                  |
+--------------------------------------------------------------------------+
| Broker Architecture (CORBA), Microsoft’s Distributed Component Object    |
| Model                                                                    |
+--------------------------------------------------------------------------+
| (DCOM) or Sun Microsystems’s Java/Remote Method Invocation (RMI).        |
+--------------------------------------------------------------------------+





.. code-block:: sql

   SELECT contextualsummary(text_sample, 'system') 
   FROM text_summary_sample


+--------------------------------------------------------------------------+
| contextualsummary                                                        |
+==========================================================================+
| system                                                                   |
+--------------------------------------------------------------------------+
| system                                                                   |
+--------------------------------------------------------------------------+
|                                                                          |
+--------------------------------------------------------------------------+
|                                                                          |
+--------------------------------------------------------------------------+
|                                                                          |
+--------------------------------------------------------------------------+

.. rubric:: Example 3



.. code-block:: sql

   SELECT contextualsummary(text_sample, 'service') 
   FROM text_summary_sample


+--------------------------------------------------------------------------+
| contextualsummary                                                        |
+==========================================================================+
| service ... service                                                      |
+--------------------------------------------------------------------------+
|                                                                          |
+--------------------------------------------------------------------------+
|                                                                          |
+--------------------------------------------------------------------------+
|                                                                          |
+--------------------------------------------------------------------------+
|                                                                          |
+--------------------------------------------------------------------------+

.. rubric:: Example 4



.. code-block:: sql

   SELECT contextualsummary(text_sample, 'web', '\', '/', '--', 25) 
   FROM text_summary_sample


+--------------------------------------------------------------------------+
| contextualsummary                                                        |
+==========================================================================+
| A \web/ service (also-- (specifically \Web/ Services-- with the \web/    |
| service                                                                  |
+--------------------------------------------------------------------------+
| with other \web/-related                                                 |
+--------------------------------------------------------------------------+
| as \web/ services                                                        |
+--------------------------------------------------------------------------+
|                                                                          | 
+--------------------------------------------------------------------------+
|                                                                          |
+--------------------------------------------------------------------------+


GETSESSION
=================================================================================

.. rubric:: Description

The ``GETSESSION`` function provides information about the session
established with a Virtual DataPort server.

.. rubric:: Syntax

.. code-block:: bnf

   GETSESSION( <parameter : literal> )

-  ``parameter`` can be **one** of the following (the value returned by the function depends on this parameter):

   a. ``user``: the function returns the user name of the current user.
   #. ``useragent``: the function returns the user agent associated to the 
      current user's connection.
   #. ``database``: the function returns the name of the Virtual DataPort
      database that the client is connected to.
   #. ``i18n``: the function returns the name of the i18N
      (internationalization) configuration of the database that the client
      is connected to.
   #. ``roles``: the function returns an array with the names of the
      effective roles assigned to the current user. The effective roles are
      the roles assigned directly and indirectly to the user.
      For example, if “user1” has the role “A” assigned and the role “A”
      has the role “B” assigned, this function returns “A” and “B”.


.. rubric:: Example



.. code-block:: sql

   SELECT GETSESSION('user') || '@' || GETSESSION('database') as user_name
   FROM Dual();
   
+--------------------------------------------------------------------------+
| user\_name                                                               |
+==========================================================================+
| user1@denodo\_samples\_db                                                |
+--------------------------------------------------------------------------+


HASH
=================================================================================

.. rubric:: Description

The ``HASH`` function calculates the digest of the input value, with the
algorithm MD5 and returns it encoded in base64 (not in base 16).

For the same input, this function always returns the same value.

It returns null when the input parameter is null.

.. rubric:: Syntax

.. code-block:: bnf

   HASH( <value:text> ):text

-  ``value``: Required. The name of a field or a literal.

.. rubric:: Example

.. code-block:: sql

   SELECT hash('text value') as hash_value, hash(null) as null_value
   FROM Dual()


+--------------------------------------+--------------------------------------+
| hash_value                           | null_value                           |
+======================================+======================================+
| eNV9lLZhJ1lex1ztvwnajg==             | NULL                                 |
+--------------------------------------+--------------------------------------+


IS\_PROJECTED\_FIELD
=================================================================================

.. rubric:: Description

The ``IS_PROJECTED_FIELD`` function returns ``true`` if the field passed
as parameter is projected (i.e. the field is in the ``SELECT`` statement of the query). ``False`` otherwise.

.. rubric:: Syntax

.. code-block:: bnf

   IS_PROJECTED_FIELD( <field name:literal> ):boolean

-  ``field name``. Required. Name of the field with the exact case of
   the field. The function returns ``false`` if this parameter is null.

.. rubric:: Examples

Consider the following view ``items``:

+--------------------------------------+--------------------------------------+
| item                                 | price                                |
+======================================+======================================+
| A                                    | 3.45                                 |
+--------------------------------------+--------------------------------------+
| B                                    | 9.99                                 |
+--------------------------------------+--------------------------------------+
| C                                    | 4.99                                 |
+--------------------------------------+--------------------------------------+

.. rubric:: Example 1

.. code-block:: sql

   SELECT item, IS_PROJECTED_FIELD('item')
   FROM items

+--------------------------------------+--------------------------------------+
| item                                 | is\_projected\_field                 |
+======================================+======================================+
| A                                    | true                                 |
+--------------------------------------+--------------------------------------+
| B                                    | true                                 |
+--------------------------------------+--------------------------------------+
| C                                    | true                                 |
+--------------------------------------+--------------------------------------+

.. rubric:: Example 2

.. code-block:: sql

   SELECT sum(price) AS total, IS_PROJECTED_FIELD('price')
   FROM items

.. csv-table:: 
   :header: "total", "is_projected_field"
   
   "18.43", "true"

In this example, ``IS_PROJECTED_FIELD`` returns ``true`` because the field ``price`` is projected, even though the query applies a function over this field.

.. rubric:: Example 3

.. code-block:: sql

   SELECT item, IS_PROJECTED_FIELD('price')
   FROM items

.. csv-table:: 
   :header: "item", "is_projected_field"
   
   "A", "false"
   "B", "false"
   "C", "false"

In this example, ``IS_PROJECTED_FIELD`` returns ``false`` because
the query does not project the field ``price``.

.. rubric:: Example 4

.. code-block:: sql

   SELECT item AS field1, IS_PROJECTED_FIELD('field1')
   FROM items

.. csv-table:: 
   :header: "field1", "is_projected_field"
   
   "A", "false"
   "B", "false"
   "C", "false"

In this example, ``IS_PROJECTED_FIELD`` returns ``false`` because this functions checks if a field of the view is projected, not the alias of the fields.


MAP
=================================================================================

.. rubric:: Description

The ``MAP`` function returns the value associated with a key. The pair
key-value can be obtained from a view or from a map (see section :ref:`Defining a Map`). When the key does not exist, the function returns
``NULL``.

There are two possible signatures:

.. rubric:: Syntax 1

.. code-block:: bnf

   MAP ( <key:text>, <view name:text>, <key field:text>,
       <value field:text> )

It obtains the value associated with a key. MAP searches the value of a
key in the columns of a view.

-  ``key``. Required. The value to search in the view.
-  ``view name``. Required. The name of the view that contains the key
   and its value.
-  ``key field``. Required. The column of the view that contains the
   keys.
-  ``value field``. Required. The column of the view that contains the
   values.

.. rubric:: Syntax 2

.. code-block:: bnf

   MAP ( <key:text>, <map name:text> [, <i18n:text> ] )

It obtains the value associated with a key from a Map.

-  ``key``. Required. The value to search in the map.
-  ``map name``. Required. The name of the map that contains the key and
   its value.
-  ``i18n``. Optional. Internationalization configuration of the
   contents.

.. note:: In both cases, ``key`` is a case-insensitive parameter.

.. rubric:: Examples

.. rubric:: Example 1

Consider the map ``food``:

.. code-block:: vql

   CREATE MAP SIMPLE food (
       'breakfast' = 'milk'
       'dinner' = 'lettuce'
       'lunch' = 'meat'
   )


.. code-block:: sql

   SELECT MAP('breakfast', 'food') AS breakfast
       , MAP('lunch', 'food') AS lunch
       , MAP('dinner', 'food') AS dinner
       , MAP('none', 'food') AS none
   FROM Dual()

+--------------------+--------------------+--------------------+--------------------+
| breakfast          | lunch              | dinner             | none               |
+====================+====================+====================+====================+
| milk               | meat               | lettuce            | NULL               |
+--------------------+--------------------+--------------------+--------------------+

.. rubric:: Example 2

Consider the view ``FOREIGN_SALES`` that contains the revenue of a
company in each country, in the country’s currency.



+--------------------+--------------------+--------------------+--------------------+
| country            | month              | revenue            | currency           |
+====================+====================+====================+====================+
| Mexico             | JAN                | 7536.00            | MXN                |
+--------------------+--------------------+--------------------+--------------------+
| Spain              | JAN                | 20000.00           | EUR                |
+--------------------+--------------------+--------------------+--------------------+
| United Kingdom     | JAN                | 26816.00           | GBP                |
+--------------------+--------------------+--------------------+--------------------+
| Canada             | FEB                | -25616.00          | CAD                |
+--------------------+--------------------+--------------------+--------------------+
| Japan              | FEB                | 100024.00          | JPY                |
+--------------------+--------------------+--------------------+--------------------+

And the ``CURRENCY_RATES_TO_USD`` map that contains the exchange rate of
each currency to dollar.



.. code-block:: vql

   CREATE MAP SIMPLE currency_rates_to_usd (
       'CAD' = '0.957121'
       'EUR' = '1.4971'
       'GBP' = '1.67'
       'JPY' = '0.011166'
       'MXN' = '0.076989'
       'USD' = '1.0'
   );

.. code-block:: sql

   SELECT month
       , country
       , CAST('float', MAP(CURRENCY, 'currency_rates_to_usd')) * revenue AS revenue_usd
   FROM foreign_sales


+-------------------------+-------------------------+-------------------------+
| month                   | country                 | revenue\_usd            |
+=========================+=========================+=========================+
| JAN                     | Mexico                  | 580.19                  |
+-------------------------+-------------------------+-------------------------+
| JAN                     | Spain                   | 29942.00                |
+-------------------------+-------------------------+-------------------------+
| JAN                     | United Kingdom          | 44782.72                |
+-------------------------+-------------------------+-------------------------+
| FEB                     | Canada                  | -24517.61               |
+-------------------------+-------------------------+-------------------------+
| FEB                     | Japan                   | 1116.87                 |
+-------------------------+-------------------------+-------------------------+


NULLIF
=================================================================================

.. rubric:: Description

The ``NULLIF`` function compares two values or expressions and returns
``NULL`` if they are equal. Otherwise it returns the first value.

This function is equivalent to the statement:



.. code-block:: sql

   CASE WHEN <expression> = <expression>
       THEN NULL
       ELSE <expression>
   END

``NULLIF`` performs implicit type conversion. That is, if the two
parameters have different type, it will try to cast one of them in order

to make the comparison.

I.e.: if the first parameter is ``1`` (``text``) and the second is
``1`` (``integer``), it will convert the ``text`` parameter to an
integer and they will be considered equal even if their type is
different.

.. rubric:: Syntax

.. code-block:: bnf

   NULLIF(<expression>, <expression>)

.. rubric:: Examples

Consider the view ``internet_inc``:



+--------------------+--------------------+--------------------+--------------------+
| id                 | summary            | ttime              | taxid              |
+====================+====================+====================+====================+
| 1                  | Error in ADSL      | 2005-06-29         | B78596011          |
|                    | router             | 19:19:41.0         |                    |
+--------------------+--------------------+--------------------+--------------------+
| 2                  | Incident in ADSL   | 2005-06-29         | B78596012          |
|                    | router             | 19:19:41.0         |                    |
+--------------------+--------------------+--------------------+--------------------+
| 3                  | Install additional | 2005-06-29         | B78596013          |
|                    | line               | 19:19:41.0         |                    |
+--------------------+--------------------+--------------------+--------------------+
| 4                  | Bandwidth increase | 2005-06-29         | B78596014          |
|                    |                    | 19:19:41.0         |                    |
+--------------------+--------------------+--------------------+--------------------+

.. rubric:: Example 1



.. code-block:: sql

   SELECT NULLIF(id, 1) AS display 
   FROM internet_inc

+--------------------------------------------------------------------------+
| display                                                                  |
+==========================================================================+
| NULL                                                                     |
+--------------------------------------------------------------------------+
| 2                                                                        |
+--------------------------------------------------------------------------+
| 3                                                                        |
+--------------------------------------------------------------------------+
| 4                                                                        |
+--------------------------------------------------------------------------+

.. rubric:: Example 2



.. code-block:: sql

   SELECT * 
   FROM internet_inc 
   WHERE NULLIF(ID, 1) <> NULL


+--------------------+--------------------+--------------------+--------------------+
| id                 | summary            | ttime              | taxid              |
+====================+====================+====================+====================+
| 2                  | Incident in ADSL   | 2005-06-29         | B78596012          |
|                    | router             | 19:19:41.0         |                    |
+--------------------+--------------------+--------------------+--------------------+
| 3                  | Install additional | 2005-06-29         | B78596013          |
|                    | line               | 19:19:41.0         |                    |
+--------------------+--------------------+--------------------+--------------------+
| 4                  | Bandwidth increase | 2005-06-29         | B78596014          |
|                    |                    | 19:19:41.0         |                    |
+--------------------+--------------------+--------------------+--------------------+

The first row of the view does not match the condition.

.. rubric:: Example 3



.. code-block:: sql

   SELECT COALESCE(NULLIF(ID, '1'), summary) AS display 
   FROM internet_inc

+--------------------------------------------------------------------------+
| display                                                                  |
+==========================================================================+
| Error in ADSL router                                                     |
+--------------------------------------------------------------------------+
| 2                                                                        |
+--------------------------------------------------------------------------+
| 3                                                                        |
+--------------------------------------------------------------------------+
| 4                                                                        |
+--------------------------------------------------------------------------+

.. note:: ``NULLIF`` has automatically converted the second parameter to
   an integer to compare it with the values of the column ID which are also
   integers.


ROWNUM
=================================================================================

.. rubric:: Description

The ``ROWNUM`` function returns a unique number for each row of the
result of a query.

This number is unique for each query even if the data were retrieved
from different sources.

The ``ORDER BY`` clause is processed after ``ROWNUM`` assigns a value to
each row.

For consistent results between several executions of a query, use
``ROWNUM()`` over a view with the ``ORDER BY`` clause.

.. rubric:: Syntax

.. code-block:: bnf

   ROWNUM( [ <offset:long > ] ):long

-  ``offset``. Optional. If present, the first value returned by ``ROWNUM`` is
   (``offset + 1``), instead of ``1``.

.. rubric:: Examples

Consider the view ``internet_inc``:


+--------------------+--------------------+--------------------+--------------------+
| id                 | summary            | ttime              | taxid              |
+====================+====================+====================+====================+
| 1                  | Error in ADSL      | 2005-06-29         | B78596011          |
|                    | router             | 19:19:41.0         |                    |
+--------------------+--------------------+--------------------+--------------------+
| 2                  | Incident in ADSL   | 2005-06-29         | B78596012          |
|                    | router             | 19:19:41.0         |                    |
+--------------------+--------------------+--------------------+--------------------+
| 3                  | Install additional | 2005-06-29         | B78596013          |
|                    | line               | 19:19:41.0         |                    |
+--------------------+--------------------+--------------------+--------------------+
| 4                  | Bandwidth increase | 2005-06-29         | B78596014          |
|                    |                    | 19:19:41.0         |                    |
+--------------------+--------------------+--------------------+--------------------+

.. rubric:: Example 1



.. code-block:: sql

   SELECT rownum(10), summary, taxid 
   FROM internet_inc


+-------------------------+-------------------------+-------------------------+
| rownum                  | summary                 | taxid                   |
+=========================+=========================+=========================+
| 11                      | Error in ADSL router    | B78596011               |
+-------------------------+-------------------------+-------------------------+
| 12                      | Incident in ADSL router | B78596012               |
+-------------------------+-------------------------+-------------------------+
| 13                      | Install additional line | B78596013               |
+-------------------------+-------------------------+-------------------------+
| 14                      | Bandwidth increase      | B78596014               |
+-------------------------+-------------------------+-------------------------+

.. rubric:: Example 2

.. code-block:: sql

   SELECT ROWNUM(), summary, taxid
   FROM internet_inc
   ORDER BY summary

+-------------------------+-------------------------+-------------------------+
| rownum                  | summary                 | taxid                   |
+=========================+=========================+=========================+
| 4                       | Bandwidth increase      | B78596014               |
+-------------------------+-------------------------+-------------------------+
| 1                       | Error in ADSL router    | B78596011               |
+-------------------------+-------------------------+-------------------------+
| 2                       | Incident in ADSL router | B78596012               |
+-------------------------+-------------------------+-------------------------+
| 3                       | Install additional line | B78596013               |
+-------------------------+-------------------------+-------------------------+

Note that the query of "Example 2" sorts the results by the field
``summary``. As the ``ORDER BY`` clause is processed after the
``ROWNUM`` function assigns a value to each row, the first value of
``ROWNUM`` is not ``1``.
