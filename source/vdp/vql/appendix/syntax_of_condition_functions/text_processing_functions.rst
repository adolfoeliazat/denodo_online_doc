=========================
Text Processing Functions
=========================

Text processing functions manipulate and transform values of type
``text``.

Usually input values of types that are not ``text``, are automatically
converted to ``text`` values before applying the function.

.. contents:: Text functions supported by Virtual DataPort
   :depth: 1
   :local:
   :backlinks: none
   :class: threecols

ASCII
=================================================================================

.. rubric:: Description

The ``ASCII`` function returns the Unicode code point of the first
character of the input value.

.. rubric:: Syntax

.. code-block:: bnf

   ASCII( <value:text> ):int

-  ``value``. Required.

.. rubric:: Example



.. code-block:: sql

   SELECT ASCII('Denodo') AS value1, ASCII('興味深い例') AS value2

+--------------------------------------+--------------------------------------+
| value1                               | value2                               |
+======================================+======================================+
| 68                                   | 33288                                |
+--------------------------------------+--------------------------------------+


CHAR
=================================================================================

.. rubric:: Description

The ``CHAR`` function returns the character associated to a Unicode code
point.

When the input code point is higher than 255 and the function is pushed
down to Impala or Hive, the function returns ``NULL``. That is because
they do not support code points higher than 255.

.. rubric:: Syntax

.. code-block:: bnf

   CHAR( <code:int> ):text

-  ``code``. Required. Unicode code point of the character you want to
   obtain.

.. rubric:: Example

.. code-block:: sql

   SELECT CHAR(65) AS char_1, CHAR(945) AS char_alpha;

+--------------------------------------+--------------------------------------+
| char\_1                              | char\_alpha                          |
+======================================+======================================+
| A                                    | α                                    |
+--------------------------------------+--------------------------------------+


CONCAT
=================================================================================

.. rubric:: Description

The ``CONCAT`` function concatenates parameters into one string.

.. rubric:: Syntax

.. code-block:: bnf

   CONCAT( <value 1:text>, <value 2:text> [, <value N:text> ]* ):text

-  ``value 1``. Required. The first text item to be concatenated.
-  ``value 2``. Required. The second text item to be concatenated.
-  ``value N``. Optional. One or more arguments to be concatenated.

.. rubric:: Example



.. code-block:: sql

   SELECT original_text, CONCAT('I like to fly to ', originalText, ' every
   month') as concat_text
   FROM my_table;


+--------------------------------------+--------------------------------------+
| original\_text                       | concat\_text                         |
+======================================+======================================+
| San Francisco, CA                    | I like to fly to San Francisco, CA   |
|                                      | every month                          |
+--------------------------------------+--------------------------------------+
| San Jose, CA                         | I like to fly to San Jose, CA every  |
|                                      | month                                |
+--------------------------------------+--------------------------------------+
| Birmingham, AL                       | I like to fly to Birmingham, AL      |
|                                      | every month                          |
+--------------------------------------+--------------------------------------+
| NY, NY                               | I like to fly to NY, NY every month  |
+--------------------------------------+--------------------------------------+


INSTR
=================================================================================

.. rubric:: Description

The ``INSTR`` function returns the index of a string within another
string.

.. rubric:: Syntax

.. code-block:: bnf

   INSTR( <str1:text>, <str2:text> ):int

Returns the index of the first character of the first occurrence of
``str2`` within ``str1``.

The index of the first character is ``0``.

If ``str1`` is ``NULL``, the function returns ``NULL``.

If ``str2`` is not present within ``str1``, it returns ``-1``.

This function is case-sensitive when executed by Virtual DataPort.
However, when it is pushed-down to the database, the behavior may be
different. There are databases where this function is case-sensitive and
others where is case-insensitive.

.. rubric:: Example


.. code-block:: sql

   SELECT original_text, INSTR(originalText, 'i') as result
   FROM myTable;


+--------------------------------------+--------------------------------------+
| original\_text                       | result                               |
+======================================+======================================+
| San Francisco, CA                    | 9                                    |
+--------------------------------------+--------------------------------------+
| San Jose, CA                         | -1                                   |
+--------------------------------------+--------------------------------------+
| Birmingham, AL                       | 1                                    |
+--------------------------------------+--------------------------------------+
| NY, NY                               | -1                                   |
+--------------------------------------+--------------------------------------+


LEN
=================================================================================

.. rubric:: Description

The ``LEN`` function returns the number of characters in a text string

.. rubric:: Syntax

.. code-block:: bnf

   LEN( <value:text> ):int

-  ``value``. Required. The text whose length you want to find. Spaces
   count as characters.

.. rubric:: Example



.. code-block:: sql

   SELECT original_text, LEN(originalText) as len_text
   FROM myTable;


+--------------------------------------+--------------------------------------+
| original\_text                       | len\_text                            |
+======================================+======================================+
| San Francisco, CA                    | 18                                   |
+--------------------------------------+--------------------------------------+
| San Jose, CA                         | 13                                   |
+--------------------------------------+--------------------------------------+
| Birmingham, AL                       | 15                                   |
+--------------------------------------+--------------------------------------+
| NY, NY                               | 7                                    |
+--------------------------------------+--------------------------------------+


LOWER
=================================================================================

.. rubric:: Description

The ``LOWER`` function converts text to lowercase.

.. rubric:: Syntax

.. code-block:: bnf

   LOWER( <value:text> ):text

-  ``value``. Required. Text to convert to lower case.

.. rubric:: Example


.. code-block:: sql

   SELECT original_text, LOWER(originalText) as lower_text
   FROM Mytable;


+--------------------------------------+--------------------------------------+
| original\_text                       | lower\_text                          |
+======================================+======================================+
| San Francisco, CA                    | san francisco, ca                    |
+--------------------------------------+--------------------------------------+
| San Jose, CA                         | san jose, ca                         |
+--------------------------------------+--------------------------------------+
| Birmingham, AL                       | birmingham, al                       |
+--------------------------------------+--------------------------------------+
| NY, NY                               | ny, ny                               |
+--------------------------------------+--------------------------------------+


LTRIM
=================================================================================

.. rubric:: Description

The ``LTRIM`` function returns the input value, without its leading
white spaces and carriage returns.

.. rubric:: Syntax

.. code-block:: bnf

   LTRIM( <value:text> ):text

-  ``value``. Required.


MAX
=================================================================================

.. rubric:: Description

The ``MAX`` function returns the lexicographically greatest argument of
the list. The function compares the Unicode value of each character of the input values.

This function is case sensitive.

.. rubric:: Syntax

.. code-block:: bnf

   MAX( <value 1:text>, <value 2:text> [, <value N:text> ]* ):text

-  ``value 1``. Required.
-  ``value 2``. Required.
-  ``value N``. Optional. One or more arguments.

**Examples**

**Example 1**


.. code-block:: sql

   SELECT MAX('DENODO', 'Virtual DataPort')
   FROM Dual();

+--------------------------------------------------------------------------+
| max                                                                      |
+==========================================================================+
| Virtual DataPort                                                         |
+--------------------------------------------------------------------------+

**Example 2**

.. code-block:: sql

   SELECT MAX('denodo', 'Virtual DataPort')
   FROM Dual();


+--------------------------------------------------------------------------+
| max                                                                      |
+==========================================================================+
| denodo                                                                   |
+--------------------------------------------------------------------------+

In this example, the result is "denodo" because the first letter has the highest Unicode value: d = 100 and V = 86.

MIN
=================================================================================

.. rubric:: Description

The ``MIN`` function returns the lexicographically lowest argument of
the list. The function compares the Unicode value of each character of the input values.

This function is case sensitive.

.. rubric:: Syntax

.. code-block:: bnf

   MIN( <value 1:text>, <value 2:text> [, <value N:text> ]*): text

-  ``value 1``. Required.
-  ``value 2``. Required.
-  ``value N``. Optional.

**Examples**

**Example 1**



.. code-block:: sql

   SELECT MIN('ITPILOT', 'virtual DataPort', 'aracne')
   FROM Dual();

+--------------------------------------------------------------------------+
| min                                                                      |
+==========================================================================+
| ITPILOT                                                                  |
+--------------------------------------------------------------------------+

**Example 2**

.. code-block:: sql

   SELECT MIN('it pilot', 'virtual DataPort', 'aracne')
   FROM Dual();


+--------------------------------------------------------------------------+
| min                                                                      |
+==========================================================================+
| aracne                                                                   |
+--------------------------------------------------------------------------+

In this example, the result is "aracne" because the first letter has the lowest Unicode value: a = 97, i = 105 and v = 118.

POSITION
=================================================================================

.. rubric:: Description

The ``POSITION`` function returns the first position, if any, at which
one string (``value1``) occurs within another (``value2``).

.. rubric:: Syntax

.. code-block:: bnf

   POSITION( <value 1:text> IN <value 2:text> ) : int

-  ``value 1``. Text you want to search in ``value2``.
-  ``value 2``.

If ``value 1`` or ``value 2`` are ``NULL``, the function returns ``NULL``.

If the length of ``value 1`` is zero, the function returns one.

If ``value 1`` does not occur in ``value 2``, the function returns zero.

.. rubric:: Example

.. code-block:: sql

   SELECT POSITION('no' IN 'Denodo') AS pos_1, POSITION('z' IN 'Denodo') AS
   pos_2


+--------------------------------------+--------------------------------------+
| pos\_1                               | pos\_2                               |
+======================================+======================================+
| 3                                    | 0                                    |
+--------------------------------------+--------------------------------------+


REGEXP
=================================================================================

.. rubric:: Description

The ``REGEXP`` function replaces each substring of the input string that matches the given regular expression, with the given replacement.

.. rubric:: Syntax

.. code-block:: bnf

   REGEXP( <original text:text>, <regex:text>, <replacement:text> ):text

-  ``original text``. Required. Input string.
-  ``regex``. Required. Regular expression to which ``original text`` is matched.
-  ``replacement``. Required. Each match of ``regular expression`` will be replaced by this. This value is also a regular expression so you can specify capturing groups.

If any of the parameters is null, the function returns null.

``regex`` is a regular expression so you can pass any text value but take into account that some characters will have special meaning. E.g. ``.`` represents any character not just the dot, ``\d`` represents a digit, etc.

This function follows the behavior of the Java regular expressions, which are very similar to the Perl ones. Find more information in the `Java documentation about regular expressions <https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html>`_.

The characters ``^`` and ``$`` only match at the beginning and the end, respectively, of the input value. To detect line terminators, use ``\n``.

.. rubric:: Examples

.. rubric:: Example 1

Replacing the character "#" with "*".

.. code-block:: sql

   SELECT REGEXP('########## DATABASE ##########', '#', '*') AS result
   FROM Dual();

+--------------------------------+
| result                         |
+================================+
| ********** DATABASE ********** | 
+--------------------------------+

.. rubric:: Example 2

Regular expression with character classes "\d" and capturing groups:

.. code-block:: sql

   SELECT REGEXP('Number: 3829022', 'Number: (\d+)', 'Value: $1') AS result
   FROM Dual();

+--------------------------------+
| result                         |
+================================+
| Value: 3829022                 | 
+--------------------------------+

``$1`` represents the first capturing group.

REMOVEACCENTS
=================================================================================

.. rubric:: Description

The ``REMOVEACCENTS`` function replaces all characters with an accent
with the same characters without accent.

.. rubric:: Syntax

.. code-block:: bnf

   REMOVEACCENTS( <value:text> ):text

-  ``value``. Required. Text you want to remove accents from.

.. rubric:: Example



.. code-block:: sql

   SELECT REMOVEACCENTS('bё áéíóú àèìòù') as text_without_accent
   FROM Dual();

+--------------------------------------------------------------------------+
| text\_without\_accent                                                    |
+==========================================================================+
| Bё aeiou aeiou                                                           |
+--------------------------------------------------------------------------+


REPEAT
=================================================================================

.. rubric:: Description

The ``REPEAT`` function repeats a text a given number of times.

.. rubric:: Syntax

.. code-block:: bnf

   REPEAT ( <value:text>, <count:int> ):text

-  ``value``. Required. The text you want to repeat
-  ``count``. Required. Number of times ``value`` will be repeated. If 0
   or less than 0, the function returns an empty string.

.. rubric:: Example



.. code-block:: sql

   SELECT REPEAT('Denodo ', 3), REPEAT('Platform', 0)

+--------------------------------------+--------------------------------------+
| repeat                               | repeat\_                             |
+======================================+======================================+
| Denodo Denodo Denodo                 |                                      |
+--------------------------------------+--------------------------------------+


REPLACE
=================================================================================

.. rubric:: Description

The ``REPLACE`` function substitutes new text for old text in a text
string.

.. rubric:: Syntax

.. code-block:: bnf

   REPLACE( <value:text>, <from:text>, <to:text> ):text

-  ``value``. Required. Text which you want to replace some/all of it.
-  ``from``. Required. All occurrences to be replaced.
-  ``to``. Required. Text which will replace all the occurrences of
   ``from``.

.. rubric:: Example



.. code-block:: sql

   SELECT original_text, REPLACE(originalText, 'CA', 'California') as
   replace_text
   FROM my_table;


+--------------------------------------+--------------------------------------+
| original\_text                       | replace\_text                        |
+======================================+======================================+
| San Francisco, CA                    | San Francisco, California            |
+--------------------------------------+--------------------------------------+
| San Jose, CA                         | San Jose, California                 |
+--------------------------------------+--------------------------------------+
| Birmingham, AL                       | Birmingham, AL                       |
+--------------------------------------+--------------------------------------+
| NY, NY                               | NY, NY                               |
+--------------------------------------+--------------------------------------+


REPLACEMAP
=================================================================================

.. rubric:: Description

The ``REPLACEMAP`` function, whose input parameters are a ``text`` value
and a list of key-value pairs, replaces all the occurrences of each key
in the text, with its value.

The list of key-value pairs can be obtained from a view or a map (see
section :ref:`Defining a Map`). If the list is obtained from a view, the
keys are obtained from one of the fields of the view and the values,
from another.

.. rubric:: Syntax 1

.. code-block:: bnf

   REPLACEMAP( <search text:text>, <map_name:text> ):text

-  ``search_text``. Required. Text which you want to replace some/all of
   it.
-  ``map_name``. Required. Name of the map that contains the key/value
   pairs.

If ``map_name`` does not exist, the function returns ``NULL``.

.. rubric:: Syntax 2

.. code-block:: bnf

   REPLACEMAP( <search_text:text>, <viewName:text>, <keyField:text>, <valueField:text> ):text

-  ``searchText``. Required. Text which you want to replace some/all of
   it.
-  ``viewName``. Required. View that contains the key/value pairs.
-  ``keyField``. Required. The field from ``view_name`` which contains
   the keys.
-  ``valueField``. Required. The field from ``view_name`` which contains
   the values.

If ``viewName`` does not exist, the function returns ``NULL``.

If ``keyField`` or ``valueField`` are not fields of the view, the
function returns ``NULL``.

**Examples**

**Example 1**

Consider the following map:

.. code-block:: sql
   :caption: Now consider the following query:
   :name: Now consider the following query:

   CREATE MAP simple "daysOfTheWeek" (
       'Sun' = 'Sunday'
       'Mon' = 'Monday'
       'Tus' = 'Tuesday'
       'Wed' = 'Wednesday'
       'Thur'= 'Thursday'
       'Fri' = 'Friday'
       'Sat' = 'Saturday'
       'Sun' = 'Sunday'
   );



.. code-block:: sql

   SELECT text_block
       , replacemap (textblock, 'daysOfTheWeek') as text_block_with_full_name
   FROM V;

+--------------------------------------+--------------------------------------+
| text\_block                          | text\_block\_with\_full\_name        |
+======================================+======================================+
| I like to travel on Sun              | I like to travel on Sunday           |
+--------------------------------------+--------------------------------------+
| I am available to travel on Mon      | I am available to travel on Monday   |
+--------------------------------------+--------------------------------------+
| My best day of vacation is Sat       | My best day of vacation is Saturday  |
| because I see my relatives on Wed    | because I see my relatives on        |
|                                      | Wednesday                            |
+--------------------------------------+--------------------------------------+

The third row contains two keys of the map (“Sat” and “Wed”) and they
are both replaced by the value of these keys in the map.

In the ``CREATE MAP`` statement, we have surrounded the name of the map
with the double quotes. Otherwise, the map, or any other element, is
created in lowercase. I.e. ``CREATE MAP daysOfTheWeek...`` creates the
map ``daysoftheweek``.

The second parameter of the function has to be the name of the map with
the same case it was created. I.e., if you have created the map with the
statement ``CREATE MAP daysOfTheWeek...``, the second parameter of
``REPLACEMAP`` has to be ``daysoftheweek`` (in lowercase)

The section :ref:`Unicode Identifiers` gives more details about the
identifiers of elements in Virtual DataPort.

**Example 2**

Consider the view ``days_of_the_week``:

+--------------------------------------+--------------------------------------+
| full\_day\_name                      | abbreviated\_format                  |
+======================================+======================================+
| Sunday                               | Sun                                  |
+--------------------------------------+--------------------------------------+
| Monday                               | Mon                                  |
+--------------------------------------+--------------------------------------+
| Tuestday                             | Tus                                  |
+--------------------------------------+--------------------------------------+
| Wednesday                            | Wed                                  |
+--------------------------------------+--------------------------------------+
| Thursday                             | Thur                                 |
+--------------------------------------+--------------------------------------+
| Friday                               | Fri                                  |
+--------------------------------------+--------------------------------------+
| Saturday                             | Sat                                  |
+--------------------------------------+--------------------------------------+

Now consider the following query:



.. code-block:: sql

   SELECT text_block,
   replacemap (text_block, 'days_of_the_week', 'abbreviated_format',
   'full_day_name') AS text_block_with_full_name
   FROM V;

+--------------------------------------+--------------------------------------+
| text\_block                          | text\_block\_with\_full\_name        |
+======================================+======================================+
| I like to travel on Sun              | I like to travel on Sunday           |
+--------------------------------------+--------------------------------------+
| I am available to travel on Mon      | I am available to travel on Monday   |
+--------------------------------------+--------------------------------------+
| My best day of vacation is Sat       | My best day of vacation is Saturday  |
| because I see my relatives on Wed    | because I see my relatives on        |
|                                      | Wednesday                            |
+--------------------------------------+--------------------------------------+


RTRIM
=================================================================================

.. rubric:: Description

The ``RTRIM`` function returns the input value, without its trailing
white spaces and carriage returns.

.. rubric:: Syntax

.. code-block:: bnf

   RTRIM( <value:text> ):text

-  ``value``. Required.


SIMILARITY
=================================================================================

.. rubric:: Description

The ``SIMILARITY`` function calculates the textual similarity between
two text strings based on a given textual similarity algorithm.

.. rubric:: Syntax

.. code-block:: bnf

   SIMILARITY( <value 1:text>, <value 2:text> [ , <algorithm:text> ]):double

-  ``value 1``. Required. Text to be compared.
-  ``value 2``. Required. Text to be compared with value1.
-  ``algorithm``. Optional. Algorithm to use. Virtual DataPort provides
   the following textual similarity algorithms ("JaroWinklerTFIDF" by default):

.. csv-table:: 
   :header: "Algorithms Based on Distance Between Text Strings", "Algorithms Based on the Appearance of Common Terms in the Texts", "Combinations of Both"
   
   "ScaledLevenshtein", "TFIDF", "JaroWinklerTFIDF"
   "JaroWinkler", "Jaccard", ""
   "Jaro", "UnsmoothedJS", ""
   "Level2 Jaro", "", ""
   "MongeElkan", "", ""
   "Level2MongeElkan", "", ""

.. rubric:: Example

.. code-block:: sql

   SELECT city, SIMILARITY(city , 'San') as similarity
   FROM V
   ORDER BY similarity DESC


+--------------------------------------+--------------------------------------+
| city                                 | similarity                           |
+======================================+======================================+
| San Jose                             | 0.71                                 |
+--------------------------------------+--------------------------------------+
| San Francisco                        | 0.71                                 |
+--------------------------------------+--------------------------------------+
| NY                                   | 0.00                                 |
+--------------------------------------+--------------------------------------+
| Birmingham                           | 0.00                                 |
+--------------------------------------+--------------------------------------+


SPLIT
=================================================================================

.. rubric:: Description

The ``SPLIT`` function splits strings around matches of a given regular
expression and returns an array containing these substrings.

The results do not contain the regular expression.

.. rubric:: Syntax

.. code-block:: bnf

   SPLIT( <regexp:text>, <value:text> ):array

-  ``regexp``. Required. A regular expression. The substrings that match
   this regular expression are not included in the result.
-  ``value``. Required. Field name or text to split.

**Examples**

Consider the following view ``V``:

+--------------------------------------+--------------------------------------+
| a                                    | b                                    |
+======================================+======================================+
| 10.10                                | I am some text                       |
+--------------------------------------+--------------------------------------+
| -80.10                               | Text is $% needed always             |
+--------------------------------------+--------------------------------------+
| 20.50                                | Text for a living                    |
+--------------------------------------+--------------------------------------+
| NULL                                 | NULL                                 |
+--------------------------------------+--------------------------------------+

**Example 1**

.. code-block:: sql

   SELECT SPLIT('0', a), SPLIT('Text\s+\w+', b)
   FROM V

+--------------------------------------+--------------------------------------+
| split                                | split\_1                             |
+======================================+======================================+
| Array { { 1 } { .1 } }               | Array { I am some text }             |
+--------------------------------------+--------------------------------------+
| Array { { -8 } { .1 } }              | Array { , $% needed always}          |
+--------------------------------------+--------------------------------------+
| Array { { 2 } { .5 } }               | Array { , a living }                 |
+--------------------------------------+--------------------------------------+
| NULL                                 | NULL                                 |
+--------------------------------------+--------------------------------------+

The regular expression ``Text\s+\w+`` captures the word “Text” and
the word next to it.

**Example 2**



.. code-block:: sql

   SELECT split(' ', B) 
   FROM V

+--------------------------------------------------------------------------+
| SPLIT                                                                    |
+==========================================================================+
| Array { { I } { am} { some} { text} }                                    |
+--------------------------------------------------------------------------+
| Array { { Text } { is } { $% } { needed } { always } }                   |
+--------------------------------------------------------------------------+
| Array { { Text } { for } { a } { living } }                              |
+--------------------------------------------------------------------------+
| NULL                                                                     |
+--------------------------------------------------------------------------+

**Example 3**



.. code-block:: sql

   SELECT split('\.', A) 
   FROM V


+--------------------------------------------------------------------------+
| SPLIT                                                                    |
+==========================================================================+
| Array { { 10 } { 10 } }                                                  |
+--------------------------------------------------------------------------+
| Array { { -80 } { 10 } }                                                 |
+--------------------------------------------------------------------------+
| Array { { 20 } { 50 } }                                                  |
+--------------------------------------------------------------------------+
| NULL                                                                     |
+--------------------------------------------------------------------------+

The regular expression “\\\\.” captures the character dot. That is
because it has been escaped with the character ``\``. Otherwise,
``.`` matches any character.


SUBSTRING / SUBSTR
=================================================================================

.. rubric:: Description

The ``SUBSTRING`` and ``SUBSTR`` functions return a substring of an
input string.

Note that the results of **Syntax 1** and **Syntax 2** are not equivalent. When 
this function is used with Syntax 2, it behaves as specified in the standard SQL-92.

.. rubric:: Syntax 1

.. code-block:: bnf

   SUBSTRING( <value:text>, <start index:int> [, <end index:int> ]):text

-  ``value``. Required. Text string containing the characters to
   extract.
-  ``start index``. Required. Index of the first character of the new
   substring. The index of the first character of the input string is 0.
-  ``end index``. Optional. Index of the last character.

With this syntax, the function returns a substring that begins at ``start index`` of the input string.

If ``start index`` is a negative value, the function returns a substring
that begins at the index 0.

If ``end index`` is not present, the result goes from ``start index`` to
the end of the input value.

If ``end index`` is present, the result goes from ``start index`` and
extends to the character at index ``endIndex-1``. Thus the length of the
result is ``endIndex-startIndex``.

The function returns an empty string if one of the following conditions
are met:

-  ``start index`` and ``end index`` are equal.
-  ``start index`` is greater than the length of ``value``.

The function returns ``NULL`` if at least one of the following
conditions are met:

-  Any of the parameters are ``NULL``.
-  ``start index`` is greater than ``end index``.

.. rubric:: Syntax 2

.. code-block:: bnf

   SUBSTRING( <value:text> FROM <start index:int> [ FOR <length:int> ] ):text
   
   SUBSTR( <value:text> FROM <start index:int> [ FOR <length:int> ] ):text
   
   SUBSTR( <value:text>, <start index:int> [, <length:int> ] ):text

These three ways of invoking the function behave exactly in the same
way.

.. note: These syntax behave differently than *Syntax 1*.

The behavior of this syntax is the defined for the function ``SUBSTRING`` in the standard SQL-92:

-  The function returns a substring that begins at ``start index`` of the input string 
   (with this syntax, the index of the first character is ``1`` while in *Syntax 1*, is ``0``)

-  If ``length`` is not present, the substring extends to the end of the
   input value.

-  If ``length`` is present, the substring has the length indicated by this
   value or shorter, if the length of input string is lower than
   ``start_index + length``.

-  If at least one of the parameters is ``NULL``, the function returns
   ``NULL``.

-  If ``length`` is present and, ``start index`` or ``length`` are negative
   values, the following formula specifies the result: the function will
   return the L characters of ``value`` starting at the character C being:

   -  L = Minimum (``start index`` + ``length``, length of ``value`` + 1) -
      Maximum (``start_index``, 1)
   -  C = the larger of ``start index`` and 1.

-  If ``length`` is not present and ``start index`` is a negative value,
   the function will return ``value``.


.. rubric:: Example of Syntax 1

.. code-block:: sql

   SELECT city, SUBSTRING(city, 1), SUBSTRING(city, 1, 5)
   FROM locations

+-------------------------+-------------------------+-------------------------+
| city                    | substring               | substring\_1            |
+=========================+=========================+=========================+
| San Jose                | an Jose                 | an J                    |
+-------------------------+-------------------------+-------------------------+
| San Francisco           | an Francisco            | an F                    |
+-------------------------+-------------------------+-------------------------+
| Birmingham              | irmingham               | irmi                    |
+-------------------------+-------------------------+-------------------------+
| NY                      | Y                       | Y                       |
+-------------------------+-------------------------+-------------------------+

.. rubric:: Example of Syntax 2

.. code-block:: sql

   SELECT city, SUBSTRING(city FROM 2), SUBSTRING(city FROM 3 FOR 5)
   FROM locations

+-------------------------+-------------------------+-------------------------+
| city                    | substring               | substring\_1            |
+=========================+=========================+=========================+
| San Jose                | an Jose                 | n Jos                   |
+-------------------------+-------------------------+-------------------------+
| San Francisco           | an Francisco            | n Fra                   |
+-------------------------+-------------------------+-------------------------+
| Birmingham              | irmingham               | rming                   |
+-------------------------+-------------------------+-------------------------+
| NY                      | Y                       | <empty string>          |
+-------------------------+-------------------------+-------------------------+

.. rubric:: Example of Syntax 2

.. code-block:: sql

   SELECT SUBSTRING ('Denodo' from -2 for 4) AS f

+--------------------------------------------------------------------------+
| f                                                                        |
+==========================================================================+
| D                                                                        |
+--------------------------------------------------------------------------+


TEXTCONSTANT
=================================================================================

.. note:: The ``TEXTCONSTANT`` function is deprecated and it may be removed in future
   major versions of the Denodo Platform.
   
   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

.. rubric:: Description

The ``TEXTCONSTANT`` function parses a parameter as a text.

.. rubric:: Syntax

.. code-block:: bnf

   TEXTCONSTANT( <text> ):text

-  ``text``. Required. Text to be displayed as is in the result.

.. rubric:: Example

.. code-block:: sql

   SELECT original_text, TEXTCONSTANT('I like to fly to') as constant_text
   FROM mytable;

+--------------------------------------+--------------------------------------+
| original\_text                       | constant\_text                       |
+======================================+======================================+
| San Francisco, CA                    | I like to fly to                     |
+--------------------------------------+--------------------------------------+
| San Jose, CA                         | I like to fly to                     |
+--------------------------------------+--------------------------------------+
| Birmingham , AL                      | I like to fly to                     |
+--------------------------------------+--------------------------------------+
| NY, NY                               | I like to fly to                     |
+--------------------------------------+--------------------------------------+


TRIM
=================================================================================

.. rubric:: Description

The ``TRIM`` function returns the input string without its leading
and/or trailing pad characters. By default, the pad characters to remove
are the whitespace and the carriage return. But you can indicate a
different character.

``LTRIM`` (see section :ref:`LTRIM`) only removes the leading white spaces
and carriage returns.

``RTRIM`` (see section :ref:`RTRIM`) only removes the trailing white spaces
and carriage returns.

.. rubric:: Syntax 1

With the first one, the characters removed are the spaces and carriage
returns.

With the second one, by default the function removes the spaces but not
the carriage returns.

.. rubric:: Syntax 2

.. code-block:: bnf

   TRIM ( <value:text> )

-  ``value``. Required. Text from which you want to remove the spaces
   and carriage returns.

.. rubric:: Syntax

.. code-block:: bnf

   TRIM ( [ <trim specification> [ <trim character:text> ] FROM ] <value:text> )

   <trim specification> ::= 
       LEADING 
     | TRAILING
     | BOTH

-  ``value``. Required. Text from which you want to remove the pad
   character. By default, the space character

   If this parameter is NULL, the function returns NULL.

-  ``LEADING``/``TRAILING``/``BOTH``. Optional.

   -  ``LEADING``: removes the pad character from the beginning of the
      input value.
   -  ``TRAILING``: removes the pad character from the end of the input
      value.
   -  ``BOTH``: removes the pad character from the beginning and end of the
      input value.

   Not adding this token is equivalent to adding the token ``BOTH``.

-  ``trim character``. Optional. The function will remove this character
   from the leading/trailing value. If this parameter contains more than
   one character, only the first one is taken into account, the rest are
   ignored.

**Examples**

**Example 1**

Query that removes the white spaces and carriage returns from the
beginning and end of the input value.



.. code-block:: sql

   SELECT original_text, TRIM(originaltext) as trim_text
   FROM mytable;





+--------------------------------------+--------------------------------------+
| original\_text                       | trim\_text                           |
+======================================+======================================+
| San Francisco , CA                   | San Francisco , CA                   |
+--------------------------------------+--------------------------------------+
| San Jose , CA                        | San Jose , CA                        |
+--------------------------------------+--------------------------------------+
| Birmingham , AL                      | Birmingham , AL                      |
+--------------------------------------+--------------------------------------+
| NY, NY                               | NY, NY                               |
+--------------------------------------+--------------------------------------+

**Example 2**

Query that removes the white spaces - but not the carriage returns -
from the beginning of the value.



.. code-block:: sql

   SELECT original_text, TRIM( LEADING FROM originaltext) as trim_text
   FROM mytable;


+--------------------------------------+--------------------------------------+
| original\_text                       | trim\_text                           |
+======================================+======================================+
| San Francisco , CA                   | San Francisco , CA                   |
+--------------------------------------+--------------------------------------+
| San Jose , CA                        | San Jose , CA                        |
+--------------------------------------+--------------------------------------+
| Birmingham , AL                      | Birmingham , AL                      |
+--------------------------------------+--------------------------------------+
| NY, NY                               | NY, NY                               |
+--------------------------------------+--------------------------------------+

**Example 3**

Query that removes the character “c” from the end of the value.

.. code-block:: sql

   SELECT original_text, TRIM( TRAILING 'c' FROM original_text ) as trim_text
   FROM mytable;

+--------------------------------------+--------------------------------------+
| original\_text                       | trim\_text                           |
+======================================+======================================+
| aaabbcbccc                           | aaabbcb                              |
+--------------------------------------+--------------------------------------+
| aaabbb                               | aaabbb                               |
+--------------------------------------+--------------------------------------+
| <null>                               | <null>                               |
+--------------------------------------+--------------------------------------+


UPPER
=================================================================================

.. rubric:: Description

The ``UPPER`` function converts text to uppercase.

.. rubric:: Syntax

.. code-block:: bnf

   UPPER( <value:text> ):text

-  ``value``. Required. Text to convert to upper case.

.. rubric:: Example



.. code-block:: sql

   SELECT original_text, UPPER(originalText) as upper_text
   FROM Mytable;


+--------------------------------------+--------------------------------------+
| original\_text                       | upper\_text                          |
+======================================+======================================+
| San Francisco , CA                   | SAN FRANCISCO , CA                   |
+--------------------------------------+--------------------------------------+
| San Jose , CA                        | SAN JOSE , CA                        |
+--------------------------------------+--------------------------------------+
| Birmingham , AL                      | BIRMINGHAM , AL                      |
+--------------------------------------+--------------------------------------+
| NY, NY                               | NY , NY                              |
+--------------------------------------+--------------------------------------+


