===============================
Datetime Processing Functions
===============================

These functions manipulate values of type ``intervaldaysecond``, ``intervalyearmonth``, ``localdate``, ``time``, ``timestamp``, ``timestamptz`` and ``date`` (deprecated).

.. contents:: Datetime functions supported by Virtual DataPort
   :depth: 1
   :local:
   :backlinks: none
   :class: threecols

ADDDAY
=================================================================================

.. rubric:: Description

The ``ADDDAY`` function returns the datetime passed as parameter with its
field day rolled up (or down, if the increment is negative) by the
amount specified.

.. rubric:: Syntax

.. code-block:: bnf

   ADDDAY( <value:intervaldaysecond>, <increment> ):intervaldaysecond   
   ADDDAY( <value:localdate>, <increment> ):localdate   
   ADDDAY( <value:timestamp>, <increment> ):timestamp   
   ADDDAY( <value:timestamptz>, <increment> ):timestamptz

   ; Deprecated signature because it uses the date (deprecated) type   
   ADDDAY( <value:date>, <increment> ):date

-  ``value``. Required. Datetime expression.
-  ``increment``. Required. Amount to increase the field day. If the
   number is negative, the field is decreased. The type of the expression can be int or long.

.. rubric:: Example

.. code-block:: sql

   SELECT time, ADDDAY(time, 8)
   FROM v


+--------------------------------------+--------------------------------------+
| time                                 | addday                               |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | Jul 7, 2005 19:19:41                 |
+--------------------------------------+--------------------------------------+
| Dec 31, 2010 22:59:56                | Jan 8, 2011 22:59:56                 |
+--------------------------------------+--------------------------------------+


ADDHOUR
=================================================================================

.. rubric:: Description

The ``ADDHOUR`` function returns the datetime passed as parameter with its
field hour rolled up (or down, if the increment is negative) by the
amount specified.

.. rubric:: Syntax

.. code-block:: bnf

   ADDHOUR( <value:intervaldaysecond>, <increment> ):intervaldaysecond   
   ADDHOUR( <value:time>, <increment> ):time   
   ADDHOUR( <value:timestamp>, <increment> ):timestamp   
   ADDHOUR( <value:timestamptz>, <increment> ):timestamptz

   ; Deprecated signature because it uses the date (deprecated) type   
   ADDDAY( <value:date>, <increment> ):date

-  ``value``. Required. Datetime expression.
-  ``increment``. Required. The amount to increase the field hour. If
   the number is negative, the field is decreased. The type of the expression can be int or long.

.. rubric:: Example

.. code-block:: sql

   SELECT time, ADDHOUR(time, -2)
   FROM v

+--------------------------------------+--------------------------------------+
| time                                 | addhour                              |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | Jun 29, 2005 17:19:41                |
+--------------------------------------+--------------------------------------+
| Jun 30, 2005 1:00:00                 | Jun 29, 2005 23:00:00                |
+--------------------------------------+--------------------------------------+


ADDMINUTE
=================================================================================

.. rubric:: Description

The ``ADDMINUTE`` function returns the datetime passed as parameter with its
field minute rolled up (or down, if the increment is negative) by the
amount specified.

.. rubric:: Syntax

.. code-block:: bnf

   ADDMINUTE( <value:intervaldaysecond>, <increment> ):intervaldaysecond   
   ADDMINUTE( <value:time>, <increment> ):time   
   ADDMINUTE( <value:timestamp>, <increment> ):timestamp   
   ADDMINUTE( <value:timestamptz>, <increment> ):timestamptz

   ; Deprecated signature because it uses the date (deprecated) type   
   ADDMINUTE( <value:date>, <increment> ):date
   
-  ``value``. Required. Datetime expression.
-  ``increment``. Required. The amount to increase the field minute. If
   the number is negative, the field is decreased. The type of the expression can be int or long.

.. rubric:: Example



.. code-block:: sql

   SELECT time, ADDMINUTE(time, 10)
   FROM v





+--------------------------------------+--------------------------------------+
| Time                                 | addminute                            |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | Jun 29, 2005 19:29:41                |
+--------------------------------------+--------------------------------------+
| Jun 30, 2005 22:59:00                | Jun 30, 2005 23:09:00                |
+--------------------------------------+--------------------------------------+


ADDMONTH
=================================================================================

.. rubric:: Description

The ``ADDMONTH`` function returns the datetime passed as parameter with its
field month rolled up (or down, if the increment is negative) by the
amount specified.

.. rubric:: Syntax

.. code-block:: bnf

   ADDMONTH( <value:intervalyearmonth>, <increment> ):intervalyearmonth
   ADDMONTH( <value:localdate>, <increment> ):localdate
   ADDMONTH( <value:timestamp>, <increment> ):timestamp
   ADDMONTH( <value:timestamptz>, <increment> ):timestamptz
   
   ; Deprecated signature because it uses the date (deprecated) type   
   ADDMONTH( <value:date, increment:int> ):date
   
-  ``value``. Required. Datetime expression.
-  ``increment``. Required. The amount to increase the field month. If
   the number is negative, the field is decreased. The type of the expression can be int or long.

.. rubric:: Example

.. code-block:: sql

   SELECT time, ADDMONTH(time, -12)
   FROM v


+--------------------------------------+--------------------------------------+
| time                                 | addmonth                             |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | Jun 29, 2004 19:19:41                |
+--------------------------------------+--------------------------------------+
| Jan 8, 2011 22:59:56                 | Jan 8, 2010 22:59:56                 |
+--------------------------------------+--------------------------------------+


ADDSECOND
=================================================================================

.. rubric:: Description

The ``ADDSECOND`` function returns the datetime passed as parameter with its
field second rolled up (or down, if the increment is negative) by the
amount specified.

.. rubric:: Syntax

.. code-block:: bnf

   ADDSECOND( <value:intervaldaysecond>, <increment> ):intervaldaysecond
   ADDSECOND( <value:time>, <increment> ):time
   ADDSECOND( <value:timestamp>, <increment> ):timestamp
   ADDSECOND( <value:timestamptz>, <increment> ):timestamptz

   ; Deprecated signature because it uses the date (deprecated) type   
   ADDSECOND( <value:date>, <increment> ):date


-  ``value``. Required. The datetime expression.
-  ``increment``. Required. The amount to increase the field second. If
   the number is negative, the field is decreased. The type of the expression can be int or long.

.. rubric:: Example

.. code-block:: sql

   SELECT time, ADDSECOND(time, 5)
   FROM v

+--------------------------------------+--------------------------------------+
| time                                 | addsecond                            |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | Jun 29, 2005 19:19:46                |
+--------------------------------------+--------------------------------------+
| Jun 30, 2005 22:59:56                | Jun 30, 2005 23:00:01                |
+--------------------------------------+--------------------------------------+


ADDWEEK
=================================================================================

.. rubric:: Description

The ``ADDWEEK`` function returns the datetime passed as parameter with its
field week rolled up (or down, if the increment is negative) by the
amount specified. That is, rolled up or down in multiples of 7 days.

.. rubric:: Syntax

.. code-block:: bnf

   ADDWEEK( <value:intervaldaysecond>, <increment> ):intervaldaysecond
   ADDWEEK( <value:localdate>, <increment> ):localdate   
   ADDWEEK( <value:timestamp>, <increment> ):timestamp   
   ADDWEEK( <value:timestamptz>, <increment> ):timestamptz

   ; Deprecated signature because it uses the date (deprecated) type   
   ADDWEEK( <value:date>, <increment> ):date

-  ``value``. Required. The datetime field.
-  ``increment``. Required. Number of times to increase the field day, 7
   days. If the number is negative, the field is decreased. If ``0``, it
   returns ``value``, unmodified. The type of the expression can be int or long.

.. rubric:: Example



.. code-block:: sql

   SELECT time, ADDWEEK(time, -2)
   FROM v


+--------------------------------------+--------------------------------------+
| time                                 | addweek                              |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | Jun 15, 2005 19:19:41                |
+--------------------------------------+--------------------------------------+
| Jan 8, 2011 22:59:56                 | Dec 25, 2010 22:59:56                |
+--------------------------------------+--------------------------------------+

We can see that the date is rolled down 2 weeks. It rolls down, instead
of rolling up, because the parameter ``increment`` is a negative number.


ADDYEAR
=================================================================================

.. rubric:: Description

The ``ADDYEAR`` function returns the datetime passed as parameter with its
field year rolled up (or down, if the increment is negative) by the
amount specified.

.. rubric:: Syntax

.. code-block:: bnf

   ADDWEEK( <value:intervalyearmonth>, <increment> ):intervalyearmonth
   ADDWEEK( <value:localdate>, <increment> ):localdate
   ADDWEEK( <value:timestamp>, <increment> ):timestamp
   ADDWEEK( <value:timestamptz>, <increment> ):timestamptz

   ; Deprecated signature because it uses the date (deprecated) type   
   ADDWEEK( <value:date>, <increment> ):date


-  ``value``. Required. The datetime field.
-  ``increment``. Required. The amount to increase the field year. If
   the number is negative, the field is decreased. The type of the expression can be int or long.

.. rubric:: Example



.. code-block:: sql

   SELECT time, ADDYEAR(time, 7) 
   FROM v

+--------------------------------------+--------------------------------------+
| time                                 | addyear                              |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | Jun 29, 2012 19:19:41                |
+--------------------------------------+--------------------------------------+
| Jan 8, 2011 22:59:56                 | Jan 8, 2018 22:59:56                 |
+--------------------------------------+--------------------------------------+


CURRENT_DATE
=================================================================================

.. rubric:: Description

The ``CURRENT_DATE`` function returns a ``localdate`` value that represents
the current date.

.. rubric:: Syntax

.. code-block:: bnf

   CURRENT_DATE() : localdate
   CURRENT_DATE : localdate

You can invoke this function with or without brackets. See the following
example.

.. rubric:: Example



.. code-block:: sql

   SELECT CURRENT_DATE() AS current_date_1, CURRENT_DATE AS
   current_date_2





+--------------------------------------+--------------------------------------+
| current\_date\_1                     | current\_date\_2                     |
+======================================+======================================+
| Oct 28, 2013                         | Oct 28, 2013                         |
+--------------------------------------+--------------------------------------+


EXTRACT
=================================================================================

.. rubric:: Description

The ``EXTRACT`` function extracts the year, month, day, hour, minute or
second from a ``datetime`` value.

.. rubric:: Syntax

.. code-block:: bnf

   EXTRACT ( <part of field> FROM <value> )
   
-  ``part of field``. Required. It can be one of the following values:

   -  ``YEAR``: returns the year of the date
   -  ``MONTH``: returns the month of the date
   -  ``DAY``: returns the day of the date
   -  ``HOUR``: returns the hour of the date
   -  ``MINUTE``: returns the minute of the date
   -  ``SECOND``: returns the second of the date
   -  ``MILLISECOND``: returns the millisecond of the date
   -  ``QUARTER``: returns the quarter of the date. The first quarter is 1, the last one is 4.
   -  ``WEEK``: returns the week number in the year. The first week of the year is 1.
   -  ``DOW``: returns the day of the week, between Sunday (0) and Saturday (6)
   -  ``DOY``: returns the day of the year. The first day of the year is 1.

-  ``value`` Required. A datetime expression.

**Examples**

**Example 1**



.. code-block:: sql

   SELECT time, EXTRACT(YEAR FROM time) AS year
   FROM view


+--------------------------------------+--------------------------------------+
| time                                 | year                                 |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | 2005                                 |
+--------------------------------------+--------------------------------------+
| Jan 1, 2012 22:59:56                 | 2012                                 |
+--------------------------------------+--------------------------------------+

This query extracts the year from the column of a field of the result.

**Example 2**



.. code-block:: sql

   SELECT time, EXTRACT(ADDDAY(time, 1)) AS next_day 
   FROM view


+--------------------------------------+--------------------------------------+
| time                                 | next\_day                            |
+======================================+======================================+
| Jun 30, 2005 19:19:41                | 1                                    |
+--------------------------------------+--------------------------------------+
| Jan 1, 2012 22:59:56                 | 2                                    |
+--------------------------------------+--------------------------------------+

This query extracts the hour of a ``datetime`` value returned by an
expression.


FIRSTDAYOFMONTH
=================================================================================

.. rubric:: Description

The ``FIRSTDAYOFMONTH`` function returns the datetime passed as parameter,
with the field day rolled down to the first day of the month. If the
datetime passed as parameter already is the first day of the month, it
returns the parameter unchanged.

.. rubric:: Syntax

.. code-block:: bnf

   FIRSTDAYOFMONTH( <value:localdate> ):localdate
   FIRSTDAYOFMONTH( <value:timestamp> ):timestamp
   FIRSTDAYOFMONTH( <value:timestamptz> ):timestamptz
   
   ; Deprecated signature because it uses the date (deprecated) type   
   FIRSTDAYOFMONTH( <value:date> ):date

-  ``value``. Required.

.. rubric:: Example


.. code-block:: sql

   SELECT time, FIRSTDAYOFMONTH(time) FROM v

+--------------------------------------+--------------------------------------+
| time                                 | firstdayofmonth                      |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | Jun 1, 2005 19:19:41                 |
+--------------------------------------+--------------------------------------+
| Jan 8, 2011 22:59:56                 | Jan 1, 2011 22:59:56                 |
+--------------------------------------+--------------------------------------+
| Jan 1, 2011 22:59:56                 | Jan 1, 2011 22:59:56                 |
+--------------------------------------+--------------------------------------+


FIRSTDAYOFWEEK
=================================================================================

.. rubric:: Description

The ``FIRSTDAYOFWEEK`` function returns the datetime passed as parameter,
with the field day rolled down to the first day of the week.

If the datetime passed as parameter already is the first day of the week, it
returns the parameter unchanged.

The first day of the week depends on the locale of the view and the
query.

For example, in the locale ``us_pst`` (U.S. Pacific Standard Time zone),
the first day of the week is Sunday, but in ``es_euro`` (Spain’s time
zone), the first day of the week is Monday.

If the function is delegated to a database, the result may depend on the
underlying database.

You can see the i18n of a view in the Advanced dialog of the view. See
more about this in the section :doc:`/vdp/administration/creating_derived_views/advanced_configuration_of_views/internationalization_configuration` of
the Administration Guide.

.. rubric:: Syntax

.. code-block:: bnf

   FIRSTDAYOFWEEK( <value:localdate> ):localdate
   FIRSTDAYOFWEEK( <value:timestamp> ):timestamp
   FIRSTDAYOFWEEK( <value:timestamptz> ):timestamptz

   ; Deprecated signature because it uses the date (deprecated) type   
   FIRSTDAYOFWEEK( <value:date> ):date

-  ``value``. Required.

.. rubric:: Example



.. code-block:: sql

   SELECT time, FIRSTDAYOFWEEK(time)
   FROM v





+--------------------------------------+--------------------------------------+
| time                                 | firstdayofweek                       |
+======================================+======================================+
| Wednesday, Jun 29, 2005 19:19:41     | Monday, Jun 27, 2005 19:19:41        |
+--------------------------------------+--------------------------------------+
| Monday, Jan 10, 2011 22:59:56        | Monday, Jan 10, 2011 22:59:56        |
+--------------------------------------+--------------------------------------+

We can see that in the second row the day already is the first day of
the week, so the output of the function is the same as the input.


FORMATDATE
=================================================================================

.. rubric:: Description

The ``FORMATDATE`` function returns a string containing a datetime-type
formatted using the given pattern.

This function relies on the date and time formatting system of Java. The :ref:`Java Date and
time patterns used in Virtual DataPort` lists the date and time
patterns of Java.

In order to delegate this function to a database, Virtual DataPort translates the pattern to the 
equivalent one in the underlying database. If the database does not support the pattern, the execution 
engine of Virtual DataPort will execute the function instead of delegating it to the database.

.. rubric:: Syntax

.. code-block:: bnf

   FORMATDATE( <datetime pattern:text>, <datetime>, [ <i18n:text> ] ):text

-  ``datetime pattern``. Required. Pattern used to format the datetime passed in
   the second parameter (see section :ref:`Date and Time Pattern Strings`
   for more information about date patterns format).
-  ``datetime``. Required. The datetime value to be formatted. The type of the expression can be 
   localdate or time or timestamp or timestamptz or date.
-  ``i18n``. Optional. Internationalization configuration. When
   ``date_pattern`` contains the pattern of the day in the week (``EEE``
   or ``EEEE``) or the name of the month (``MMM`` or ``MMMM``), this
   parameter indicates the language used to return these two elements.
   
   The value of this parameter has to be one of the i18n maps of the
   Server. E.g. ``us_pst``, ``us_est``, ``gb``, ``de``, etc.

**Examples**

**Example 1**



.. code-block:: sql

   SELECT date, FORMATDATE('yyyy.MM.dd G ''at'' HH:mm:ss', date) AS
   format_date
   FROM v


+--------------------------------------+--------------------------------------+
| date                                 | format\_date                         |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | 2005.06.29 AD at 19:19:41            |
+--------------------------------------+--------------------------------------+
| Jan 8, 2011 22:59:56                 | 2011.01.08 AD at 22:59:56            |
+--------------------------------------+--------------------------------------+

Text between single quotes is not interpreted (see ``'at'``) and is
copied to the output as it is.

.. note:: If ``date_pattern`` contains single quotes (``'``) and is also
   surrounded by single quotes, you have to escape these quotes with
   another single quote like this:

.. code-block:: sql

   SELECT formatdate('yyyy.MM.dd G ''at'' HH:mm:ss', now())

**Example 2**

.. code-block:: sql

   SELECT date, formatdate('h:mm a', date) AS format_date
   FROM v



+--------------------------------------+--------------------------------------+
| date                                 | format\_date                         |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | 7:19 PM                              |
+--------------------------------------+--------------------------------------+
| Jan 8, 2011 22:59:56                 | 22:59 PM                             |
+--------------------------------------+--------------------------------------+

**Example 3**



.. code-block:: sql

   SELECT date, formatdate('yyMMddHHmmss', date) AS format_date
   FROM v


+--------------------------------------+--------------------------------------+
| date                                 | format\_date                         |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | 050629191941                         |
+--------------------------------------+--------------------------------------+
| Jan 8, 2011 22:59:56                 | 110108225956                         |
+--------------------------------------+--------------------------------------+

**Example 4**



.. code-block:: sql

   SELECT date, FORMATDATE('MMMM, EEEE dd, yyyy', date, 'us_pst') AS
       format_date
   FROM v


+--------------------------------------+--------------------------------------+
| date                                 | format\_date                         |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | June, Wednesday 29, 2005             |
+--------------------------------------+--------------------------------------+
| Jan 8, 2011 22:59:56                 | January, Saturday 8, 2011            |
+--------------------------------------+--------------------------------------+

**Example 5**



.. code-block:: sql

   SELECT date, FORMATDATE('MMMM, EEEE dd, yyyy', date, 'de') AS
       format_date
   FROM v





+--------------------------------------+--------------------------------------+
| date                                 | format\_date                         |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | Juni, Mittwoch 29, 2005              |
+--------------------------------------+--------------------------------------+
| Jan 8, 2011 22:59:56                 | Januar, Samstag 08, 2011             |
+--------------------------------------+--------------------------------------+

The only difference between examples 4 and 5 is the parameter ``i18n``.
In example 4, the parameter is ``us_pst``, so the function returns the
names of the days in the week and months in English. In Example 5, as
the i18n is ``de``, the function returns these values in German.


GETDAY
=================================================================================

.. rubric:: Description

The ``GETDAY`` function returns the "day" field of a given datetime. The
function returns a long data-type ranging from 1 to 31.

.. rubric:: Syntax

.. code-block:: bnf

   GETDAY( <value:intervaldaysecond> ):long
   GETDAY( <value:localdate> ):long
   GETDAY( <value:timestamp> ):long
   GETDAY( <value:timestamptz> ):long

   ; Deprecated signature because it uses the date (deprecated) type   
   GETDAY( <value:date> ):long
  
-  ``value``. Required. Datetime to retrieve the day from.

.. rubric:: Example



.. code-block:: sql

   SELECT date, getday(date) as day
   FROM v;





+--------------------------------------+--------------------------------------+
| date                                 | day                                  |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | 29                                   |
+--------------------------------------+--------------------------------------+
| Jan 8, 2011 22:59:56                 | 8                                    |
+--------------------------------------+--------------------------------------+


GETDAYOFWEEK
=================================================================================

.. rubric:: Description

The ``GETDAYOFWEEK`` function returns the number of the day of the week
of this datetime.

The first day of the week is ``1`` and the last day is ``7``.

The first day of the week depends on the locale of the view and the
query. For example, in the locale ``us_pst`` (U.S. Pacific Standard Time
zone), the first day of the week is Sunday, but in ``es_euro`` (Spain’s
time zone), the first day of the week is Monday.

If the function is delegated to a database, the result may depend on the
underlying database. E.g. Oracle 11g always considers that the first day
of the week is Monday.

You can see the i18n of a view in the Advanced dialog of the view. See
more about this in the section :doc:`/vdp/administration/creating_derived_views/advanced_configuration_of_views/internationalization_configuration` of
the Administration Guide.

.. rubric:: Syntax

.. code-block:: bnf

   GETDAYOFWEEK( <value:localdate> ):long
   GETDAYOFWEEK( <value:timestamp> ):long
   GETDAYOFWEEK( <value:timestamptz> ):long

   ; Deprecated signature because it uses the date (deprecated) type   
   GETDAYOFWEEK( <value:date> ):long

-  ``value``. Required.

**Examples**

**Example 1**



.. code-block:: sql

   SELECT NOW(), GETDAYOFWEEK(NOW())
   CONTEXT('i18n' = 'US_PST')





+--------------------------------------+--------------------------------------+
| Now                                  | getdayofweek                         |
+======================================+======================================+
| Jan 6, 2013 00:00:00                 | 1                                    |
+--------------------------------------+--------------------------------------+

**Example 2**



.. code-block:: sql

   SELECT NOW(), GETDAYOFWEEK(NOW())
   CONTEXT('i18n' = 'ES_EURO')





+--------------------------------------+--------------------------------------+
| now                                  | getdayofweek                         |
+======================================+======================================+
| Jan 6, 2013 00:00:00                 | 7                                    |
+--------------------------------------+--------------------------------------+

The difference between Example 1 and Example 2 is the i18n of the query,
set in the ``CONTEXT`` clause. When you do not add the ``i18n``
parameter to the ``CONTEXT``, the query uses the i18n of the view.


GETDAYOFYEAR
=================================================================================

.. rubric:: Description

The ``GETDAYOFYEAR`` function returns the number of the day in the year
of the datetime.

The first day of the year is ``1``.

.. rubric:: Syntax

.. code-block:: bnf

   GETDAYOFYEAR( <value:localdate> ):long
   GETDAYOFYEAR( <value:timestamp> ):long
   GETDAYOFYEAR( <value:timestamptz> ):long

   ; Deprecated signature because it uses the date (deprecated) type   
   GETDAYOFYEAR( <value:date> ):long

-  ``value``. Required.

.. rubric:: Example



.. code-block:: sql

   SELECT TO_DATE('dd-MM-yyyy', '01-01-2013'),
   GETDAYOFYEAR( TO_DATE('dd-MM-yyyy', '01-01-2013') )





+--------------------------------------+--------------------------------------+
| date                                 | getdayofyear                         |
+======================================+======================================+
| Jan 1, 2013 00:00:00                 | 1                                    |
+--------------------------------------+--------------------------------------+


GETDAYSBETWEEN
=================================================================================

.. rubric:: Description

The ``GETDAYSBETWEEN`` function returns the number of days between two
dates.

It returns ``0`` if both dates represent the same day.

It returns a positive number, if the first parameter is first.

It returns a negative number if the second parameter is first.

.. rubric:: Syntax

.. code-block:: bnf

   GETDAYSBETWEEN( <value 1:localdate>, <value 2:localdate> ):long
   GETDAYSBETWEEN( <value 1:timestamp>, <value 2:timestamp> ):long
   GETDAYSBETWEEN( <value 1:timestamp>, <value 2:timestamptz> ):long

   ; Deprecated signature because it uses the date (deprecated) type   
   GETDAYSBETWEEN( <value:date> ):long

-  ``value 1``. Required.
-  ``value 2``. Required.

.. rubric:: Example



.. code-block:: sql

   SELECT date1, date2, GETDAYSBETWEEN(date1, date2)
   FROM view


+-------------------------+-------------------------+-------------------------+
| date1                   | date2                   | getdaysbetween          |
+=========================+=========================+=========================+
| Jan 1, 2013 0:00:00 AM  | Jan 2, 2013 0:00:00 AM  | 1                       |
+-------------------------+-------------------------+-------------------------+
| Jan 1, 2013 0:00:00 AM  | Dec 31, 2013 0:00:00 AM | 364                     |
+-------------------------+-------------------------+-------------------------+


GETHOUR
=================================================================================

.. rubric:: Description

The ``GETHOUR`` function returns the "hour" field of a given datetime. The
function returns a long data-type, ranging from 0 (12:00 A.M.) to 23
(11:00 P.M.).

.. rubric:: Syntax

.. code-block:: bnf

   GETHOUR( <value:intervaldaysecond> ):intervaldaysecond
   GETHOUR( <value:time> ):time
   GETHOUR( <value:timestamp> ):timestamp
   GETHOUR( <value:timestamptz> ):timestamptz

   ; Deprecated signature because it uses the date (deprecated) type   
   GETHOUR( <value:date> ):date


   
-  ``value``. Required. Datetime to retrieve the hour from.

.. rubric:: Example



.. code-block:: sql

   SELECT date, gethour(date) as hour
   FROM v;


+--------------------------------------+--------------------------------------+
| date                                 | hour                                 |
+======================================+======================================+
| Jun 29, 2005 19:20:41                | 19                                   |
+--------------------------------------+--------------------------------------+

GETMILLISECOND
=================================================================================

The ``GETMILLISECOND`` function returns the "milliseconds" field of a
given datetime.

.. rubric:: Syntax

.. code-block:: bnf

   GETMILLISECOND( <value:intervaldaysecond> ):long
   GETMILLISECOND( <value:time> ):long
   GETMILLISECOND( <value:timestamp> ):long
   GETMILLISECOND( <value:timestamptz> ):long

   ; Deprecated signature because it uses the date (deprecated) type   
   GETMILLISECOND( <value:date> ):long

-  ``value``. Required.


GETMINUTE
=================================================================================

.. rubric:: Description

The ``GETMINUTE`` function returns the "minute" field of a given datetime.
The function returns a value of type long, ranging from 0 to 59.

.. rubric:: Syntax

.. code-block:: bnf

   GETMINUTE( <value:intervaldaysecond> ):long
   GETMINUTE( <value:time> ):long
   GETMINUTE( <value:timestamp> ):long
   GETMINUTE( <value:timestamptz> ):long

   ; Deprecated signature because it uses the date (deprecated) type   
   GETMINUTE( <value:date> ):long

-  ``value``. Required. Datetime to retrieve the minute from.

.. rubric:: Example



.. code-block:: sql

   SELECT date, getMinute(date) as minute
   FROM v;





+--------------------------------------+--------------------------------------+
| date                                 | minute                               |
+======================================+======================================+
| Jun 29, 2005 19:20:41                | 20                                   |
+--------------------------------------+--------------------------------------+


GETMONTH
=================================================================================

.. rubric:: Description

The ``GETMONTH`` function returns the number of month in a year of a
given datetime. The function returns a long data-type, ranging from 1
(January) to 12 (December).

.. rubric:: Syntax

.. code-block:: bnf

   GETMONTH( <value:intervalyearmonth> ):long
   GETMONTH( <value:localdate> ):long
   GETMONTH( <value:timestamp> ):long
   GETMONTH( <value:timestamptz> ):long

   ; Deprecated signature because it uses the date (deprecated) type   
   GETMONTH( <value:date> ):long

-  ``datetime``. Required. Datetime to retrieve the number of month from.

.. rubric:: Example



.. code-block:: sql

   SELECT date, getMonth(date) as month
   FROM v


+--------------------------------------+--------------------------------------+
| date                                 | month                                |
+======================================+======================================+
| Jun 29, 2005 19:20:41                | 6                                    |
+--------------------------------------+--------------------------------------+


GETMONTHSBETWEEN
=================================================================================

.. rubric:: Description

The ``GETMONTHSBETWEEN`` function returns the number of months between
two datetimes.

It returns 0 if both datetimes represent the same month.

It returns a positive number, if the first parameter is first.

It returns a negative number if the second parameter is first.

.. rubric:: Syntax

.. code-block:: bnf

   GETMONTHSBETWEEN( <value 1:localdate>, <value 2:localdate> ):long
   GETMONTHSBETWEEN( <value 1:timestamp>, <value 2:timestamp> ):long
   GETMONTHSBETWEEN( <value 1:timestamptz>, <value 2:timestamptz> ):long

   ; Deprecated signature because it uses the date (deprecated) type   
   GETMONTHSBETWEEN( <value 1:date>, <value 2:date> ):long

-  ``value 1``. Required.
-  ``value 1``. Required.

.. rubric:: Example



.. code-block:: sql

   SELECT date1, date2, GETMONTHSBETWEEN(date1, date2)





+-------------------------+-------------------------+-------------------------+
| date1                   | date2                   | getmonthsbetween        |
+=========================+=========================+=========================+
| Jan 1, 2013 0:00:00 AM  | Feb 1, 2013 0:00:00 AM  | 1                       |
+-------------------------+-------------------------+-------------------------+
| Jan 1, 2013 0:00:00 AM  | Dec 31, 2013 0:00:00 AM | 11                      |
+-------------------------+-------------------------+-------------------------+
| Jan 1, 2013 0:00:00 AM  | Jan 15, 2013 0:00:00 AM | 0                       |
+-------------------------+-------------------------+-------------------------+


GETQUARTER
=================================================================================

.. rubric:: Description

The ``GETQUARTER`` function returns the quarter of the year of a given
datetime.

The result ranges from 1 to 4. 1 is the first quarter of the year
(January to March), 2 is the second (April to June), etc.

.. rubric:: Syntax

.. code-block:: bnf

   GETQUARTER( <value:localdate> ):long
   GETQUARTER( <value:timestamp> ):long
   GETQUARTER( <value:timestamptz> ):long

   ; Deprecated signature because it uses the date (deprecated) type   
   GETQUARTER( <value:date> ):long

-  ``datetime``. Required. Datetime from which you want to retrieve its quarter.

.. rubric:: Example



.. code-block:: sql

   SELECT date, GETQUARTER(date) as quarter
   FROM v


+--------------------------------------+--------------------------------------+
| date                                 | quarter                              |
+======================================+======================================+
| Jun 29, 2015 19:20:41                | 2                                    |
+--------------------------------------+--------------------------------------+
| Mar 1, 2015 00:00:00                 | 1                                    |
+--------------------------------------+--------------------------------------+


GETSECOND
=================================================================================

.. rubric:: Description

The ``GETSECOND`` function returns the "second" field of a given datetime.
The function returns a value of type long that ranges from 0 to 59.

.. rubric:: Syntax

.. code-block:: bnf

   GETSECOND( <value:intervaldaysecond> ):long
   GETSECOND( <value:time> ):long
   GETSECOND( <value:timestamp> ):long
   GETSECOND( <value:timestamptz> ):long

   ; Deprecated signature because it uses the date (deprecated) type   
   GETSECOND( <value:date> ):long

-  ``value``. Required. Datetime to retrieve the second from.

.. rubric:: Example



.. code-block:: sql

   SELECT date, GETSECOND(date) as second
   FROM v


+--------------------------------------+--------------------------------------+
| date                                 | second                               |
+======================================+======================================+
| Jun 29, 2005 19:20:41                | 41                                   |
+--------------------------------------+--------------------------------------+


GETTIMEINMILLIS
=================================================================================

.. rubric:: Description

The ``GETTIMEINMILLIS`` function returns the number of milliseconds from
January 1, 1970, 00:00:00 GMT to the datetime passed as parameter.

It returns a negative number if the datetime is prior to 1970.

.. rubric:: Syntax

.. code-block:: bnf

   GETTIMEINMILLIS( <value:localdate> ):long
   GETTIMEINMILLIS( <value:timestamp> ):long
   GETTIMEINMILLIS( <value:timestamptz> ):long

   ; Deprecated signature because it uses the date (deprecated) type   
   GETTIMEINMILLIS( <value:date> ):long

-  ``value``. Required.

.. rubric:: Example



.. code-block:: sql

   SELECT date, getTimeInMillis(date) as milliseconds
   FROM v


+--------------------------------------+--------------------------------------+
| date                                 | milliseconds                         |
+======================================+======================================+
| Jun 29, 2005 19:20:41                | 1120098041000                        |
+--------------------------------------+--------------------------------------+


GETWEEK
=================================================================================

.. rubric:: Description

The ``GETWEEK`` function returns the week of the year of a given datetime.

The first week of the year is 1. As defined in the standard ISO8601, the
first week of the year is that in which at least 4 days are in the year.
As a result of this definition, depending on the year the day 1 of the
year may be considered to belong to the previous year.

.. rubric:: Syntax

.. code-block:: bnf

   GETWEEK( <value:localdate> ):long
   GETWEEK( <value:timestamp> ):long
   GETWEEK( <value:timestamptz> ):long

   ; Deprecated signature because it uses the date (deprecated) type   
   GETWEEK( <value:date> ):long


-  ``value``. Required. Datetime from which you want to retrieve the week of
   the year.

.. rubric:: Example


.. code-block:: sql

   SELECT date, GETWEEK(date) as week
   FROM v


+--------------------------------------+--------------------------------------+
| Date                                 | week                                 |
+======================================+======================================+
| Jan 01, 2016 00:00:00                | 53                                   |
+--------------------------------------+--------------------------------------+
| Jan 10, 2016 00:00:00                | 1                                    |
+--------------------------------------+--------------------------------------+
| Jan 11, 2016 00:00:00                | 2                                    |
+--------------------------------------+--------------------------------------+


GETYEAR
=================================================================================

.. rubric:: Description

The ``GETYEAR`` function returns the "year" field of a given datetime.

.. rubric:: Syntax

.. code-block:: bnf

   GETYEAR( <value:intervalyearmonth> ):long
   GETYEAR( <value:localdate> ):long
   GETYEAR( <value:timestamp> ):long
   GETYEAR( <value:timestamptz> ):long

   ; Deprecated signature because it uses the date (deprecated) type   
   GETYEAR( <value:date> ):long

-  ``value``. Required. Datetime to retrieve the year from.

.. rubric:: Example



.. code-block:: sql

   SELECT date, GETYEAR(date) as year
   FROM Dual();


+--------------------------------------+--------------------------------------+
| date                                 | year                                 |
+======================================+======================================+
| Jun 29, 2005 19:20:41                | 2005                                 |
+--------------------------------------+--------------------------------------+


LASTDAYOFMONTH
=================================================================================

.. rubric:: Description

The ``LASTDAYOFMONTH`` function returns the datetime passed as parameter
with the field day rolled up to the last day of the month. If the date
passed as parameter already is the last day of the month, it returns the
parameter unchanged.

.. rubric:: Syntax

.. code-block:: bnf

   LASTDAYOFMONTH( <value:localdate> ):localdate
   LASTDAYOFMONTH( <value:timestamp> ):timestamp
   LASTDAYOFMONTH( <value:timestamptz> ):timestamptz

   ; Deprecated signature because it uses the date (deprecated) type   
   LASTDAYOFMONTH( <value:date> ):date


-  ``value``. Required.

.. rubric:: Example


.. code-block:: sql

   SELECT time, LASTDAYOFMONTH(time)
   FROM v

+--------------------------------------+--------------------------------------+
| time                                 | lastdayofmonth                       |
+======================================+======================================+
| Jun 30, 2005 19:19:41                | Jun 30, 2005 19:19:41                |
+--------------------------------------+--------------------------------------+
| Feb 12, 2011 22:59:56                | Feb 28, 2011 22:59:56                |
+--------------------------------------+--------------------------------------+

We can see that in the first row the day is already the last day of the
month, so the output of the function is the same as the input.


LASTDAYOFWEEK
=================================================================================

.. rubric:: Description

The ``LASTDAYOFWEEK`` function returns the datetime passed as parameter with
the field day rolled up to the last day of the week.

If the datetime passed as parameter already is the last day of the week, it
returns the parameter unchanged.

The last day of the week depends on the locale of the view and the
query.

For example, in the locale ``us_pst`` (U.S. Pacific Standard Time zone),
the last day of the week is Saturday, but in ``es_euro`` (Spain’s time
zone), the last day of the week is Sunday.

If the function is delegated to a database, the result may depend on the
underlying database.

You can see the i18n of a view in the Advanced dialog of the view. See
more about this in the section :doc:`/vdp/administration/creating_derived_views/advanced_configuration_of_views/internationalization_configuration` of
the Administration Guide.

.. rubric:: Syntax

.. code-block:: bnf

   LASTDAYOFWEEK( <value:localdate> ):localdate
   LASTDAYOFWEEK( <value:timestamp> ):timestamp
   LASTDAYOFWEEK( <value:timestamptz> ):timestamptz

   ; Deprecated signature because it uses the date (deprecated) type   
   LASTDAYOFWEEK( <value:date> ):date

-  ``value``. Required.

.. rubric:: Example


.. code-block:: sql

   SELECT time, LASTDAYOFWEEK(time)
   FROM v


+--------------------------------------+--------------------------------------+
| time                                 | lastdayofweek                        |
+======================================+======================================+
| Thursday, Jun 30, 2005 19:19:41      | Sunday, Jul 03, 2005 19:19:41        |
+--------------------------------------+--------------------------------------+
| Saturday, Dec 31, 2011 22:59:56      | Sunday, Jan 1, 2012 22:59:56         |
+--------------------------------------+--------------------------------------+
| Sunday, Jul 03, 2005 19:19:41        | Sunday, Jul 03, 2005 19:19:41        |
+--------------------------------------+--------------------------------------+


MAX
=================================================================================

See appendix :ref:`arithmetic_function_max`.


MIN
=================================================================================

See appendix :ref:`arithmetic_function_min`.


NEXTWEEKDAY
=================================================================================

.. rubric:: Description

The ``NEXTWEEKDAY`` function returns this datetime with its field day rolled
up to the day of the week indicated by the parameter ``weekDay``.

If the parameter ``datetime`` already represents the day ``weekDay``, the
function rolls up the date to the same day of next week.

The days of the week are: Sunday = 0, Monday = 1, Tuesday = 2 …

.. rubric:: Syntax

.. code-block:: bnf

   NEXTWEEKDAY( <value:localdate>, <week day:int> ):localdate
   NEXTWEEKDAY( <value:timestamp>, <week day:int> ):timestamp
   NEXTWEEKDAY( <value:timestamptz>, <week day:int> ):timestamptz

   ; Deprecated signature because it uses the date (deprecated) type   
   NEXTWEEKDAY( <value:date>, <week day:int> ):date

-  ``value``. Required. Datetime expression.
-  ``week day``. Required. The day of the week that the datetime will be
   rolled up to.

.. rubric:: Example



.. code-block:: sql

   SELECT time, NEXTWEEKDAY(time, 3)
   FROM v





+--------------------------------------+--------------------------------------+
| time                                 | nextweekday                          |
+======================================+======================================+
| Thursday, Jun 30, 2005 19:19:41      | Wednesday, Jul 6, 2005 19:19:41      |
+--------------------------------------+--------------------------------------+
| Monday, Feb 7, 2011 22:59:56         | Feb, Wed 9, 2011 22:59:56            |
+--------------------------------------+--------------------------------------+
| Wednesday, Feb 9, 2011 9:37:02       | Wednesday, Feb 16, 2011 9:37:02      |
+--------------------------------------+--------------------------------------+


NOW
=================================================================================

.. rubric:: Description

The ``NOW`` function returns the current date and time.

If you want to obtain a ``date`` value that represents the current date,
but not the time, use the function ``CURRENT_DATE()`` (see section :ref:`CURRENT_DATE`)

.. rubric:: Syntax

.. code-block:: bnf

   NOW():timestamptz

.. rubric:: Example



.. code-block:: sql

   SELECT now() as date_and_time_now
   FROM Dual();





+--------------------------------------------------------------------------+
| date\_and\_time\_now                                                     |
+==========================================================================+
| Feb 9, 2011 9:37:02+01:00                                                |
+--------------------------------------------------------------------------+


PREVIOUSWEEKDAY
=================================================================================

.. rubric:: Description

The ``PREVIOUSWEEKDAY`` function returns this datetime with its field day
rolled down to the day of the week indicated by the parameter
``weekDay``.

If the parameter ``datetime`` already represents the day ``weekDay``, the
function rolls down the datetime to the same day of previous week.

The days of the week are: Sunday = 0, Monday = 1, Tuesday = 2 …

.. rubric:: Syntax

.. code-block:: bnf

   PREVIOUSWEEKDAY( <value:localdate>, <week day:int> ):localdate
   PREVIOUSWEEKDAY( <value:timestamp>, <week day:int> ):timestamp
   PREVIOUSWEEKDAY( <value:timestamptz>, <week day:int> ):timestamptz

   ; Deprecated signature because it uses the date (deprecated) type   
   PREVIOUSWEEKDAY( <value:date>, <week day:int> ):date

-  ``value``. Required. Datetime expression.
-  ``week day``. Required. The day of the week that the datetime will be
   rolled down to.

.. rubric:: Example



.. code-block:: sql

   SELECT time, previousweekday(time, 2)
   FROM v





+--------------------------------------+--------------------------------------+
| time                                 | previousweekday                      |
+======================================+======================================+
| Thursday, Jun 30, 2005 19:19:41      | Tuesday, Jun 28, 2005 19:19:41       |
+--------------------------------------+--------------------------------------+
| Monday, Feb 7, 2011 22:59:56         | Tuesday, Feb 1, 2011 22:59:56        |
+--------------------------------------+--------------------------------------+
| Tuesday, Jan 4, 2011 9:37:02         | Tuesday, Dec 28, 2010 9:37:02        |
+--------------------------------------+--------------------------------------+


.. _date_processing_functions_subtract:

SUBTRACT
=================================================================================

See :ref:`SUBTRACT <vql-guide-arithmetic-processing-functions-subtract>`.


.. _date_processing_functions_to_date:

TO_DATE
=================================================================================

.. rubric:: Description

The ``TO_DATE`` function converts a ``text`` value containing a datetime in
a specific format, into a value of type ``date`` that represents this
date.

.. important:: Avoid using this function on new projects because it is deprecated and it may be removed in the 
   next major version of Denodo. It is deprecated 
   because its first input parameter is of type date, which is also deprecated. 
   Instead, use :ref:`TO_LOCALDATE`, :ref:`TO_TIME`, :ref:`TO_TIMESTAMP` or :ref:`TO_TIMESTAMPTZ`. 
   See more about datetime values and functions in the section :ref:`Data Types for Dates, Timestamps and Intervals` 
   of the VQL Guide.

   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

.. rubric:: Syntax

.. code-block:: bnf

   TO_DATE( <date pattern:text>, <value:text> [, <i18n:text> ] [, <timestamp:boolean> ] ):date

-  ``date pattern``. Required. Pattern describing the date and time
   format of ``value``, following the syntax defined by Java in the
   class ``java.text.SimpleDataFormat`` (see section :ref:`Date and Time
   Pattern Strings` for more information).

   In order to delegate this function to a database, Virtual DataPort translates the pattern 
   to the equivalent one in the underlying database. If the database does not support the pattern, the execution engine of Virtual DataPort will execute the function instead of delegating it to the database.

-  ``value``. Required. String that contains a date following the
   pattern of the parameter ``datePattern``.
-  ``i18n``. Optional. Internationalization configuration. When
   ``date pattern`` contains the pattern of the day in the week (``EEE``
   or ``EEEE``) or the name of the month (``MMM`` or ``MMMM``), this
   parameter indicates the language that the function expects these
   values to be in. For example, if ``value`` will contain the names of
   the months in German, the value of this parameter has to be ``de``,
   unless the i18n of the database is ``de`` as well.

   The value of this parameter has to be one of the i18n maps of the
   Server. E.g. ``us_pst``, ``us_est``, ``gb``, ``de``, etc.
   
-  ``timestamp``. Optional. If ``false``, the function sets to ``0`` the
   fields that represent the time: hour, minute, second and millisecond.
   Passing ``true`` to this parameter is the same as not passing it.

**Examples**

**Example 1**



.. code-block:: sql

   SELECT TO_DATE('M dd yyyy HH:mm:ss', '3 05 2010 21:17:05')
   FROM Dual();





+--------------------------------------------------------------------------+
| to\_date                                                                 |
+==========================================================================+
| Fri Mar 05 21:17:05 2010                                                 |
+--------------------------------------------------------------------------+

**Example 2**



.. code-block:: sql

   SELECT TO_DATE ('yyyyMMddHHmmss', '20100701102030')
   FROM Dual();





+--------------------------------------------------------------------------+
| to\_date                                                                 |
+==========================================================================+
| Thu Jul 01 10:20:30 2010                                                 |
+--------------------------------------------------------------------------+

**Example 3**



.. code-block:: sql

   SELECT TO_DATE('yyyy-MM-dd''T''HH:mm:ss.SSS', '2001-07-04T12:08:56.235')
   FROM Dual();





+--------------------------------------------------------------------------+
| to\_date                                                                 |
+==========================================================================+
| Wed Jul 04 12:08:56 2001                                                 |
+--------------------------------------------------------------------------+

**Example 4**



.. code-block:: sql

   SELECT TO_DATE('yyyy-MM-dd''T''HH:mm:ss.SSS', '2001-07-04T12:08:56.235', false)
   FROM Dual();

+--------------------------------------------------------------------------+
| to\_date                                                                 |
+==========================================================================+
| Wed Jul 04 00:00:00 2001                                                 |
+--------------------------------------------------------------------------+

Note about examples 3 and 4: as defined by the Java class
`java.text.SimpleDataFormat <https://docs.oracle.com/javase/8/docs/api/index.html?java/text/SimpleDateFormat.html>`_, the parts of the
date pattern that are literals have to be surrounded with single quotes.
In these two examples, ``T``. But the single quote is a special
character in a Virtual DataPort literal and it has to be escaped with
another single quote. That is why the date pattern of the examples 3 and
4 contains ``''T''``.

**Example 5**



.. code-block:: sql

   SELECT date_string, TO_DATE('MMMM, EEEE dd, yyyy', date_string, 'de')
   FROM v





+--------------------------------------+--------------------------------------+
| date\_string                         | to\_date                             |
+======================================+======================================+
| Juni, Mittwoch 29, 2005              | Wed Jun 29 00:00:00 2005             |
+--------------------------------------+--------------------------------------+
| Januar, Samstag 08, 2011             | Sat Jan 08 00:00:00 2011             |
+--------------------------------------+--------------------------------------+

In this example, as the parameter ``i18n`` is ``de``, the function
expects the names of the month (``MMM``) and names of days in the week
(``EEEE``) of ``date_string`` to be in German.

TO_LOCALDATE
=================================================================================

.. rubric:: Description

The ``TO_LOCALDATE`` function converts a ``text`` value containing a datetime in
a specific format, into a value of type ``localdate`` that represents this
datetime.

.. rubric:: Syntax

.. code-block:: bnf

   TO_LOCALDATE( <localdate pattern:text>, <value:text> [, <language:text> ] ):localdate

-  ``localdate pattern``. Required. Pattern describing the date and time
   format of ``value``, following the syntax defined by Java in the
   class ``java.text.SimpleDataFormat`` (see section :ref:`Date and Time
   Pattern Strings` for more information).
   
   In order to delegate this function to a database, Virtual DataPort translates the pattern to the equivalent one in the underlying database. If the database does not support the pattern, the execution engine of Virtual DataPort will execute the function instead of delegating it to the database.

-  ``value``. Required. String that contains a localdate following the
   pattern of the parameter ``localdate Pattern``.
-  ``language``. Optional. Internationalization configuration. When
   ``localdate pattern`` contains the pattern of the day in the week (``EEE``
   or ``EEEE``) or the name of the month (``MMM`` or ``MMMM``), this
   parameter indicates the language that the function expects these
   values to be in. For example, if ``value`` will contain the names of
   the months in German, the value of this parameter has to be ``de``.

   The value of this parameter has to be one of the Java language names
   

**Examples**


**Example 1**



.. code-block:: sql

   SELECT TO_LOCALDATE ('yyyyMMdd', '20100701')
   FROM Dual();


+--------------------------------------------------------------------------+
| to\_localdate                                                            |
+==========================================================================+
| Thu Jul 01 2010                                                          |
+--------------------------------------------------------------------------+

**Example 2**



.. code-block:: sql

   SELECT TO_LOCALDATE('M dd yyyy HH:mm:ss', '3 05 2010 21:17:05')
   FROM Dual();





+--------------------------------------------------------------------------+
| to\_localdate                                                            |
+==========================================================================+
| Fri Mar 05 2010                                                          |
+--------------------------------------------------------------------------+


**Example 3**

.. code-block:: sql

   SELECT TO_LOCALDATE('yyyy-MM-dd''T''HH:mm:ss.SSS', '2001-07-04T12:08:56.235')
   FROM Dual();

+--------------------------------------------------------------------------+
| to\_localdate                                                            |
+==========================================================================+
| Wed Jul 04  2001                                                         |
+--------------------------------------------------------------------------+


.. note:: As defined by the Java class
   `java.text.SimpleDataFormat <https://docs.oracle.com/javase/8/docs/api/index.html?java/text/SimpleDateFormat.html>`_, the parts of the
   date pattern that are literals have to be surrounded with single quotes.
   
   In this example, ``T``. But the single quote is a special
   character in a Virtual DataPort literal and it has to be escaped with
   another single quote. That is why the date pattern contains ``''T''``.

**Example 4**



.. code-block:: sql

   SELECT date_string, TO_LOCALDATE('MMMM, EEEE dd, yyyy', date_string, 'de')
   FROM v





+--------------------------------------+--------------------------------------+
| date\_string                         | to\_localdate                        |
+======================================+======================================+
| Juni, Mittwoch 29, 2005              | Wed Jun 29 2005                      |
+--------------------------------------+--------------------------------------+
| Januar, Samstag 08, 2011             | Sat Jan 08 2011                      |
+--------------------------------------+--------------------------------------+

In this example, as the parameter ``language`` is ``de``, the function
expects the names of the month (``MMM``) and names of days in the week
(``EEEE``) of ``date_string`` to be in German.

   


TO_TIME
=================================================================================

.. rubric:: Description

The ``TO_TIME`` function converts a ``text`` value containing a datetime in
a specific format, into a value of type ``time`` that represents the time part of the 
datetime.

.. rubric:: Syntax

.. code-block:: bnf

   TO_TIME( <time pattern:text>, <value:text> [, <language:text> ] ):time

-  ``time pattern``. Required. Pattern describing the date and time
   format of ``value``, following the syntax defined by Java in the
   class ``java.text.SimpleDataFormat`` (see section :ref:`Date and Time
   Pattern Strings` for more information).
   
   In order to delegate this function to a database, Virtual DataPort translates the pattern to the equivalent one in the underlying database. If the database does not support the pattern, the execution engine of Virtual DataPort will execute the function instead of delegating it to the database.

-  ``value``. Required. String that contains a datetime following the
   pattern of the parameter ``time Pattern``.
-  ``language``. Optional. Internationalization configuration. When
   ``time pattern`` contains the pattern of the day in the week (``EEE``
   or ``EEEE``) or the name of the month (``MMM`` or ``MMMM``), this
   parameter indicates the language that the function expects these
   values to be in. For example, if ``value`` will contain the names of
   the months in German, the value of this parameter has to be ``de``.

   The value of this parameter has to be one of the Java language names.
   

**Examples**

**Example 1**



.. code-block:: sql

   SELECT TO_TIME ('HHmmss', '102030')
   FROM Dual();



+--------------------------------------------------------------------------+
| to\_time                                                                 |
+==========================================================================+
| 10:20:30                                                                 |
+--------------------------------------------------------------------------+

**Example 2**



.. code-block:: sql

   SELECT TO_TIME('M dd yyyy HH:mm:ss', '3 05 2010 21:17:05')
   FROM Dual();



+--------------------------------------------------------------------------+
| to\_time                                                                 |
+==========================================================================+
| 21:17:05                                                                 |
+--------------------------------------------------------------------------+

**Example 3**

.. code-block:: sql

   SELECT TO_TIME('yyyy-MM-dd''T''HH:mm:ss.SSS', '2001-07-04T12:08:56.235')
   FROM Dual();

+--------------------------------------------------------------------------+
| to\_time                                                                 |
+==========================================================================+
| 12:08:56.235                                                             |
+--------------------------------------------------------------------------+


.. note:: As defined by the Java class
   `java.text.SimpleDataFormat <https://docs.oracle.com/javase/8/docs/api/index.html?java/text/SimpleDateFormat.html>`_, the parts of the
   date pattern that are literals have to be surrounded with single quotes.

   In this example, ``T``. But the single quote is a special
   character in a Virtual DataPort literal and it has to be escaped with
   another single quote. That is why the date pattern contains ``''T''``.


TO_TIMESTAMP
=================================================================================


.. rubric:: Description

The ``TO_TIMESTAMP`` function converts a ``text`` value containing a datetime in
a specific format, into a value of type ``timestamp`` that represents this
datetime.

.. rubric:: Syntax

.. code-block:: bnf

   TO_TIMESTAMP( <timestamp pattern:text>, <value:text> [, <language:text> ] ):timestamp

-  ``timestamp pattern``. Required. Pattern describing the date and time
   format of ``value``, following the syntax defined by Java in the
   class ``java.text.SimpleDataFormat`` (see section :ref:`Date and Time
   Pattern Strings` for more information).

   In order to delegate this function to a database, Virtual DataPort translates the pattern to the equivalent one in the underlying database. If the database does not support the pattern, the execution engine of Virtual DataPort will execute the function instead of delegating it to the database.

-  ``value``. Required. String that contains a datetime following the
   pattern of the parameter ``timestamp Pattern``.
-  ``language``. Optional. Internationalization configuration. When
   ``timestamp pattern`` contains the pattern of the day in the week (``EEE``
   or ``EEEE``) or the name of the month (``MMM`` or ``MMMM``), this
   parameter indicates the language that the function expects these
   values to be in. For example, if ``value`` will contain the names of
   the months in German, the value of this parameter has to be ``de``.

   The value of this parameter has to be one of the Java language names.
   

**Examples**

**Example 1**



.. code-block:: sql

   SELECT TO_TIMESTAMP('M dd yyyy HH:mm:ss', '3 05 2010 21:17:05')
   FROM Dual();





+--------------------------------------------------------------------------+
| to\_timestamp                                                            |
+==========================================================================+
| Fri Mar 05 21:17:05.000 2010                                             |
+--------------------------------------------------------------------------+

**Example 2**



.. code-block:: sql

   SELECT TO_TIMESTAMP ('yyyyMMddHHmmss', '20100701102030')
   FROM Dual();





+--------------------------------------------------------------------------+
| to\_timestamp                                                            |
+==========================================================================+
| Thu Jul 01 10:20:30.000 2010                                             |
+--------------------------------------------------------------------------+

**Example 3**



.. code-block:: sql

   SELECT TO_TIMESTAMP('yyyy-MM-dd''T''HH:mm:ss.SSS', '2001-07-04T12:08:56.235')
   FROM Dual();





+--------------------------------------------------------------------------+
| to\_timestamp                                                            |
+==========================================================================+
| Wed Jul 04 12:08:56.235 2001                                             |
+--------------------------------------------------------------------------+


**Example 4**



.. code-block:: sql

   SELECT date_string, TO_TIMESTAMP('MMMM, EEEE dd, yyyy', date_string, 'de')
   FROM v

+--------------------------------------+--------------------------------------+
| date\_string                         | to\_timestamp                        |
+======================================+======================================+
| Juni, Mittwoch 29 01:24:58, 2005     | Wed Jun 29 01:24:58.000 2005         |
+--------------------------------------+--------------------------------------+
| Januar, Samstag 08 03:15:01, 2011    | Sat Jan 08 03:15:01.000 2011         |
+--------------------------------------+--------------------------------------+

In this example, as the parameter ``language`` is ``de``, the function
expects the names of the month (``MMM``) and names of days in the week
(``EEEE``) of ``date_string`` to be in German.

TO_TIMESTAMPTZ
=================================================================================

.. rubric:: Description

The ``TO_TIMESTAMPTZ`` function converts a ``text`` value containing a datetime in
a specific format, into a value of type ``timestamptz`` that represents this
datetime.

.. rubric:: Syntax

.. code-block:: bnf

   TO_TIMESTAMPTZ( <timestamptz pattern:text>, <value:text> [, <language:text> ] ):timestamptz

-  ``timestamptz pattern``. Required. Pattern describing the date and time
   format of ``value``, following the syntax defined by Java in the
   class ``java.text.SimpleDataFormat`` (see section :ref:`Date and Time
   Pattern Strings` for more information).
   
   In order to delegate this function to a database, Virtual DataPort translates the pattern to the equivalent one in the underlying database. If the database does not support the pattern, the execution engine of Virtual DataPort will execute the function instead of delegating it to the database.

-  ``value``. Required. String that contains a datetime following the
   pattern of the parameter ``timestamptz Pattern``.
-  ``language``. Optional. Internationalization configuration. When
   ``timestamptz pattern`` contains the pattern of the day in the week (``EEE``
   or ``EEEE``) or the name of the month (``MMM`` or ``MMMM``), this
   parameter indicates the language that the function expects these
   values to be in. For example, if ``value`` will contain the names of
   the months in German, the value of this parameter has to be ``de``.

   The value of this parameter has to be one of the Java language names.
   

**Examples**

**Example 1**



.. code-block:: sql

   SELECT TO_TIMESTAMPTZ('M dd yyyy HH:mm:ss', '3 05 2010 21:17:05')
   FROM Dual();





+--------------------------------------------------------------------------+
| to\_timestamptz                                                          |
+==========================================================================+
| Fri Mar 05 21:17:05.000 2010 +01:00                                      |
+--------------------------------------------------------------------------+

**Example 2**



.. code-block:: sql

   SELECT TO_TIMESTAMPTZ ('yyyyMMddHHmmss', '20100701102030')
   FROM Dual();





+--------------------------------------------------------------------------+
| to\_timestamptz                                                          |
+==========================================================================+
| Thu Jul 01 10:20:30.000 2010 +02:00                                      |
+--------------------------------------------------------------------------+

**Example 3**



.. code-block:: sql

   SELECT TO_TIMESTAMPTZ('yyyy-MM-dd''T''HH:mm:ss.SSS', '2001-07-04T12:08:56.235')
   FROM Dual();





+--------------------------------------------------------------------------+
| to\_timestamptz                                                          |
+==========================================================================+
| Wed Jul 04 12:08:56.235 2001 +02:00                                      |
+--------------------------------------------------------------------------+


**Example 4**



.. code-block:: sql

   SELECT date_string, TO_TIMESTAMPTZ('MMMM, EEEE dd, yyyy', date_string, 'de')
   FROM v

+--------------------------------------+--------------------------------------+
| date\_string                         | to\_timestamptz                      |
+======================================+======================================+
| Juni, Mittwoch 29 01:24:58, 2005     | Wed Jun 29 01:24:58.000 2005 +02:00  |
+--------------------------------------+--------------------------------------+
| Januar, Samstag 08 03:15:01, 2011    | Sat Jan 08 03:15:01.000 2011 +01:00  |
+--------------------------------------+--------------------------------------+

In this example, as the parameter ``language`` is ``de``, the function
expects the names of the month (``MMM``) and names of days in the week
(``EEEE``) of ``date_string`` to be in German.

TRUNC
=================================================================================

.. rubric:: Description

The ``TRUNC`` function returns the date passed as parameter, truncated
to a specific unit of measure.

This function has the same syntax as the function ``TRUNC(date)`` of the
Oracle database. The parameter ``pattern`` also has the same syntax.

.. rubric:: Syntax

.. code-block:: bnf

   TRUNC( <value:localdate> [, <pattern:text> ] ):localdate
   TRUNC( <value:time> [, <pattern:text> ] ):time
   TRUNC( <value:timestamp> [, <pattern:text> ] ):timestamp
   TRUNC( <value:timestamptz> [, <pattern:text> ] ):timestamptz

   ; Deprecated signature because it uses the date (deprecated) type   
   TRUNC( <value:date> [, <pattern:text> ] ):date

-  ``value``. Required. Datetime to be truncated.
-  ``pattern``. The ``datetime`` is truncated to the unit specified by this
   parameter.
   If ``pattern`` is missing, ``datetime`` is truncated to the nearest day.
   The table below lists the possible values of this parameter.

.. table:: TRUNC function: values of the pattern parameter
   :name: TRUNC function: values of the pattern parameter
   
   +--------------------------------------+--------------------------------------+
   | Pattern                              | Truncating Unit                      |
   +======================================+======================================+
   | CC                                   | Century                              |
   | SCC                                  |                                      |
   +--------------------------------------+--------------------------------------+
   | SYYYY                                | Year                                 | 
   | YYYY                                 |                                      |
   | YEAR                                 |                                      |
   | SYEAR                                |                                      |
   | YYY                                  |                                      |
   | YY                                   |                                      |
   | Y                                    |                                      |
   +--------------------------------------+--------------------------------------+
   | IYYY                                 | ISO Year                             |
   | IYY                                  |                                      |
   | IY                                   |                                      |
   | I                                    |                                      |
   +--------------------------------------+--------------------------------------+
   | Q                                    | Quarter                              |
   +--------------------------------------+--------------------------------------+
   | MONTH                                | Month                                |
   | MON                                  |                                      |
   | MM                                   |                                      |
   | RM                                   |                                      |
   +--------------------------------------+--------------------------------------+
   | WW                                   | Same day of the week as the first    |
   |                                      | day of the year                      |
   +--------------------------------------+--------------------------------------+
   | IW                                   | Same day of the week as the first    |
   |                                      | day of the ISO year                  |
   +--------------------------------------+--------------------------------------+
   | W                                    | Same day of the week as the first    |
   |                                      | day of the month                     |
   +--------------------------------------+--------------------------------------+
   | DDD                                  | Day                                  |
   | DD                                   |                                      |
   | J                                    |                                      |
   +--------------------------------------+--------------------------------------+
   | DAY                                  | Starting day of the week             |
   | DY                                   |                                      |
   | D                                    |                                      |
   +--------------------------------------+--------------------------------------+
   | HH                                   | Hour                                 |
   | HH12                                 |                                      |
   | HH24                                 |                                      |
   +--------------------------------------+--------------------------------------+
   | MI                                   | Minute                               |
   +--------------------------------------+--------------------------------------+


**Examples**

**Example 1**



.. code-block:: sql

   SELECT time, TRUNC(time)
   FROM v

+--------------------------------------+--------------------------------------+
| time                                 | trunc                                |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | Jun 29, 2005 0:0:00                  |
+--------------------------------------+--------------------------------------+
| Jan 8, 2011 22:59:56                 | Jan 8, 2011 0:0:00                   |
+--------------------------------------+--------------------------------------+

As the parameter pattern is not present, the date is truncated to the
day.

**Example 2**



.. code-block:: sql

   SELECT time, TRUNC(time, 'MONTH')
   FROM v





+--------------------------------------+--------------------------------------+
| Time                                 | trunc                                |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | Jun 1, 2005 0:00:00                  |
+--------------------------------------+--------------------------------------+
| Jan 8, 2011 22:59:56                 | Jan 1, 2011 0:00:00                  |
+--------------------------------------+--------------------------------------+

**Example 3**



.. code-block:: sql

   SELECT time, TRUNC(time, 'Q')
   FROM v





+--------------------------------------+--------------------------------------+
| time                                 | Trunc                                |
+======================================+======================================+
| Jun 29, 2005 19:19:41                | Apr 1, 2005 00:00:00                 |
+--------------------------------------+--------------------------------------+
| Jan 8, 2011 22:59:56                 | Jan 1, 2011 00:00:00                 |
+--------------------------------------+--------------------------------------+

The pattern ``Q`` means that the date will be truncated to the
quarter.
