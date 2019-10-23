==============================================
Data Types for Dates, Timestamps and Intervals
==============================================

Virtual DataPort has data types to hold datetime and interval values. See the table below:

.. csv-table:: 
   :header: "Name of the Type in Denodo", "Description", "Name of the Type in the SQL Standard"
   
   "LOCALDATE", "Date without a time zone (year, month and day)", "DATE"
   "TIME", "Time without a time zone (hour, minute, second and fraction of second with nanosecond precision)", "TIME WITHOUT TIME ZONE"
   "TIMESTAMP", "Timestamp without a time zone (year, month, day, hour, minute, second and fraction of second with nanosecond precision)", "TIMESTAMP WITHOUT TIME ZONE"
   "TIMESTAMPTZ", "Timestamp with a time zone displacement", "TIMESTAMP WITH TIME ZONE"
   "INTERVALDAYSECOND", "Duration of a period of time with a precision that can include any set of contiguous fields other than YEAR or MONTH", "day-time intervals"
   "INTERVALYEARMONTH", "Duration of a period of time with a precision that includes a YEAR field or a MONTH field, or both", "year-month intervals"
   "DATE (deprecated)", "Timestamp with a time zone displacement. Although similar to ``TIMESTAMPTZ``, it only has millisecond precision and its behavior is not 100% ANSI SQL-compliant. It should not be used anymore. Maintained for compatibility reasons.", "N/A"
    
Virtual DataPort maintains the DATE type from previous versions to keep backward compatibility. 
However, it will be removed in future major versions so you should plan on stop using it on your new developments. This type is similar to 
the type TIMESTAMP WITH LOCAL TIMEZONE of Oracle.
This data type has been deprecated because it introduces great complexities when dealing with this time, particularly when the client applications or the 
data sources run on a different time zone than the Denodo server.

Rules to Build Queries that Deal with Datetime Types
=====================================================
The following subsections list the rules you should follow when executing queries that involve datetime fields:

.. contents::
   :local:
   :depth: 1
   :backlinks: none

Most Appropriate Method to Define a Datetime Value
--------------------------------------------------
To build a datetime literal you have two options: 

1. Use the syntax of the standard (name of the type followed by the value on a specific format).
#. Or with a function.

Both methods are equivalent.

+----------------+-----------------+----------------------------------------------------------+
| Type in Denodo | Option to Build | Examples                                                 |
|                | a Value of this |                                                          |
|                | Type            |                                                          |
+================+=================+==========================================================+
| localdate      | Literal         | .. code-block:: sql                                      |
|                |                 |                                                          |
|                |                 |    DATE '2018-01-15'                                     |
|                +-----------------+----------------------------------------------------------+
|                | Function        | .. code-block:: sql                                      |
|                |                 |                                                          |
|                |                 |    to_localdate('yyyy-MM-dd', '2018-01-15')              |
+----------------+-----------------+----------------------------------------------------------+
| time           | Literal         | .. code-block:: sql                                      |
|                |                 |                                                          |
|                |                 |    TIME '21:45:59'                                       |
|                |                 |                                                          |
|                |                 |    TIME '21:45:59.999'                                   |
|                +-----------------+----------------------------------------------------------+
|                | Function        | .. code-block:: sql                                      |
|                |                 |                                                          |
|                |                 |    to_time('HH:mm:ss.SSS','21:45:59.999')                |
+----------------+-----------------+----------------------------------------------------------+
| timestamp      |Literal          | .. code-block:: sql                                      |
|                |                 |                                                          |
|                |                 |    TIMESTAMP '2018-01-15 21:45:01'                       |
|                |                 |                                                          |
|                |                 |    TIMESTAMP '2018-01-15 21:45:01.256'                   |
|                +-----------------+----------------------------------------------------------+
|                | Function        | .. code-block:: sql                                      |
|                |                 |                                                          |
|                |                 |    to_timestamp(                                         |
|                |                 |        'yyyy-MM-dd HH:mm:ss.SSS'                         |
|                |                 |      , '2018-01-15 21:45:01.256')                        |
+----------------+-----------------+----------------------------------------------------------+
| date           | The same as with timestamptz (see below). From the perspective of clients, |
| (deprecated)   | both types behave in the same way                                          | 
+----------------+-----------------+----------------------------------------------------------+
| timestamptz    | Literal         | .. code-block:: sql                                      |
|                |                 |                                                          |
|                |                 |    TIMESTAMP WITH TIME ZONE '2018-01-15 21:45:01 +02:00' |
|                |                 |                                                          |
|                |                 |    TIMESTAMP WITH TIME ZONE '2018-01-15 21:45:01.256     |
|                |                 |    +02:00'                                               |
|                +-----------------+----------------------------------------------------------+
|                | Function        | .. code-block:: sql                                      |
|                |                 |                                                          |
|                |                 |    to_timestamptz(                                       |
|                |                 |        'yyyy-MM-dd HH:mm:ss.SSS XXX'                     |
|                |                 |      , '2018-01-15 21:45:01.256 +02:00')                 |
+----------------+-----------------+----------------------------------------------------------+
| interval_year\ | Literal         | .. code-block:: sql                                      |
| \_month        |                 |                                                          |
|                |                 |    INTERVAL '145' YEAR                                   |
|                |                 |                                                          |
|                |                 |    INTERVAL '145-11' YEAR TO MONTH                       |
|                +-----------------+----------------------------------------------------------+  
|                | Function        | There are no functions to build intervals                |
+----------------+-----------------+----------------------------------------------------------+  
| time           | Literal         | .. code-block:: sql                                      |
|                |                 |                                                          |
|                |                 |    INTERVAL '4 5:12:10.222' DAY TO SECOND                |
|                |                 |                                                          |
|                |                 |    INTERVAL '4 5:12' DAY TO MINUTE                       |
|                +-----------------+----------------------------------------------------------+  
|                | Function        | There are no functions to build intervals                |
+----------------+-----------------+----------------------------------------------------------+    


Use the Appropriate Function to Obtain the Current Datetime
------------------------------------------------------------
To compare a datetime value with the current instant of time, use the appropriate function depending on the type you are comparing with.

.. csv-table:: 
   :header: "Type of the Expression you are Comparing With", "Function to Use Obtain the Current Instant"

   "localdate", "CURRENT_DATE()"
   "time", "CAST(CURRENT_TIMESTAMP AS TIME WITHOUT TIME ZONE)"
   "timestamp", "CAST(CURRENT_TIMESTAMP AS TIMESTAMP WITHOUT TIME ZONE)"
   "date (deprecated)", "NOW()"
   "timestamptz", "NOW() or CURRENT_TIMESTAMP"
   

Converting a Datetime Value to another Datetime Value
------------------------------------------------------

A datetime value of one type can be converted to a datetime value of another type. The table below shows the possible  conversions between different datetime data types.

.. table:: Datetime data type conversions

   +--------------------------+------------------------+------------------------+----------------------------------------------------------+----------------------------------------------------------+
   |                          |to LOCALDATE            | to TIME                | to TIMESTAMP                                             | to TIMESTAMPTZ                                           | 
   +==========================+========================+========================+==========================================================+==========================================================+
   | **from LOCALDATE**       | trivial                | not supported          |Copy year, month, day. Set hour, minute, second to (zero) |SV→TS w/o TZ→TS w/ TZ                                     |
   +--------------------------+------------------------+------------------------+----------------------------------------------------------+----------------------------------------------------------+
   | **from TIME**            | not supported          | trivial                |Copy date fields from CURRENT_DATE and time fields from SV|SV→TS w/o TZ→TS w/ TZ                                     | 
   +--------------------------+------------------------+------------------------+----------------------------------------------------------+----------------------------------------------------------+
   | **from TIMESTAMP**       |Copy date fields from SV|Copy time fields from SV| trivial                                                  | TV.UTC = SV-STZD; TV.TZ=STZD                             |
   +--------------------------+------------------------+------------------------+----------------------------------------------------------+----------------------------------------------------------+
   | **to TIMESTAMPTZ**       |SV → TS w/o TZ→LOCALDATE|SV→TS w/o TZ→TIME       | SV.UTC + SV.TZ                                           | trivial                                                  |
   +--------------------------+------------------------+------------------------+----------------------------------------------------------+----------------------------------------------------------+

-  SV: source value.
-  TV: target value.
-  UTC: UTC component of SV or TV (if and only if the source or target has time zone).
-  TZ: time zone displacement of SV or TV (if and only if the source or target has time zone), STZD is the 
   SQL-session default time zone displacement.
-  "→": the conversion is conceptually done in several steps. For example, from "timestamptz" to "localdate", the conversion is "SV → TS w/o TZ→LOCALDATE". This means that the "timestamptz" value is converted to "timestamp", and the "timestamp" to "date".

.. rubric:: Rules:

-  All these conversions are done implicitly if necessary, except the conversions from and to timestamptz, which need to be done explicitly (using the function CAST).

-  When converting from timestamp to timestamptz, the time zone added to the timestamptz value is the time zone of the i18n of the Denodo server.


.. rubric:: Examples:
  
-  2017-10-15 21:45:00 → 2017-10-15 21:45:00 +02:00
  
   From timestamp to timestamptz. The time zone of the new value is the i18n of the Server.
  
-  2017-01-15 21:15:00 → 2017-01-15

   From timestamp to date: remove the time components.

-  2017-10-15 21:45:01 → 21:45:01

   From timestamp to time: remove the date components.

-  2017-10-15 21:45:00 +05:00 → 2017-10-15 18:45:00
  
   From timestamptz to timestamp: convert to local timestamp subtracting the difference between the time zone displacement of the source value and the time zone of the i18n of the Denodo server. In this example, the time zone of the Server is +02:00. 
  
-  2017-10-15 21:45:00 +05:00 → 2017-01-16
  
   From timestamptz to localdate: first convert to timestamp (2017-10-16 00:45:00), and then remove the time part. In this example, the time zone of the Server is +08:00. Note that in this example, the day in the source value is different from the day in the target value. That is because in the first stage of the conversion (from timestamptz to timestamp), the time is modified according to the difference in time zones and this can lead to a change in the day as well.
   
-  2017-10-15 21:45:00 +05:00 → 18:45:00

   From timestamptz to time: first convert to timestamp (2017-10-15 18:45:00), and then remove the date part. In this example, the time zone of the Server is +02:00.
   
-  2017-01-15 → 2017-01-15 00:00:00
 
   From localdate to timestamp: set the time components to zero.
   
-  2017-01-15 → 2017-01-15 00:00:00 +05:00

   From localdate to timestamptz: first convert to timestamp, and then add the time zone of the i18n of the Server. In this example, the time zone of the Server is +05:00. 

-  21:45:01 → 2017-10-15 21:45:01
   
   From time to timestamp: complete the date part with the current date in the Denodo server.

-  21:45:00 → 2017-10-15 21:45:00 +02:00

   From time to timestamptz: first convert to timestamp (2017-10-15 21:45:00), and then add the time zone of the i18n of the Server. In this example, the time zone of the Server is +02:00. 

Data Types for Intervals
------------------------

Intervals represent a period of time. There are two types of intervals:

1. ``intervalyearmonth``: duration of a period of time with a precision that includes a YEAR field or a MONTH field, or both. Examples:

   .. code-block:: sql

      -- 145 years
      INTERVAL '145' YEAR
      
      -- 145 years and 11 months
      INTERVAL '145-11' YEAR TO MONTH
      
      -- 145 months
      INTERVAL '145' MONTH

2. ``intervaldaysecond``: duration of a period of time with a precision that can include any set of contiguous fields other than YEAR or MONTH.

   .. code-block:: sql

      -- 4 days, 5 hours, 12 minutes, 10 seconds and 222 milliseconds
      INTERVAL '4 5:12:10.222' DAY TO SECOND
      
      -- 4 days, 5 hours and 12 minutes
      INTERVAL '4 5:12' DAY TO MINUTE
      
      -- 11 hours and 20 minutes
      INTERVAL '11:20' HOUR TO MINUTE
      
      -- 10 minutes and 22 seconds
      INTERVAL '10:22' MINUTE TO SECOND
      
      -- 30 seconds and 123 milliseconds
      INTERVAL '30.123' SECOND
      
Two interval values are only comparable if they are of the same interval type.

Arithmetic Operators for Datetime and Interval Values
-----------------------------------------------------

You can use arithmetic operators to perform operations over values of type datetime and INTERVAL.

.. table:: Valid operators involving datetimes and intervals

   +------------------+----------------+-----------------+--------------------+
   |     Operand 1    |    Operator    |    Operand 2    |    Result Type     |
   +==================+================+=================+====================+
   | Datetime         | \-             | Datetime        | long :sup:`*-1`    |
   +------------------+----------------+-----------------+--------------------+
   | Datetime         | \+ or -        | Interval        | Datetime           |
   +------------------+----------------+-----------------+--------------------+
   | Interval         | \+             | Datetime        | Datetime           |
   +------------------+----------------+-----------------+--------------------+
   | Interval         | \+ or -        | Interval        | Interval           |
   +------------------+----------------+-----------------+--------------------+
   | Interval         | \* or /        | int or long     | Interval           |
   +------------------+----------------+-----------------+--------------------+
   | int or long      | \*             | Interval        | Interval           |
   +------------------+----------------+-----------------+--------------------+

:sup:`*-1`: number of days between <operand 1> and <operand 2>. The result is a positive number if <operand 1> is a more recent datetime than <operand 2>. Otherwise, the result is a negative number.
      
Nanosecond Precision of Time Values
===================================

Starting with Denodo 7.0 update 20180926, the types TIME, TIMESTAMP, TIMESTAMPTZ and INTERVALDAYSECOND have nanosecond precision, supporting up to nine digits as fraction of second.

In previous updates of Denodo 7.0, these values only had millisecond precision. If a source returned these values with a precision higher than millisecond, Denodo truncated them to milliseconds. With the update 20180926, these values are no longer truncated. If the change in precision affects other applications, you can go back to the previous behavior by running this command from the VQL Shell:
  
.. code-block:: vql 

   SET 'com.denodo.vdb.types.datetime.enableNanoSecondPrecisionSupport' = 'false';

Restart the Virtual DataPort server to apply the change.