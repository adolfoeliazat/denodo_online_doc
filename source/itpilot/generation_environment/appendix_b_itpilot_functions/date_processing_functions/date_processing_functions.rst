.. _itp_gen_environment_guide_date_processing_functions:

=========================
Date Processing Functions
=========================

Date functions allow manipulating date values:

-  ``FORMATDATE``: The ``FORMATDATE`` function is used to transform date
   values into ``string``-type values by using a specific format. It
   receives the following parameters:

   -  A ``string``-type value specifying the desired format to use to
      transform the date, for example “MM-dd-yyyy HH:mm”. This pattern follows the syntax
      specified by the class `SimpleDateFormat <https://docs.oracle.com/javase/8/docs/api/index.html?java/text/SimpleDateFormat.html>`_ of the Java API.
   -  The ``date``-type value to be formatted.
   -  An optional ``string``-type value specifying the name of the locale
      that has to be taken into account when performing the conversion.

   The return value is a ``string``-type value representing the date
   parameter with the selected format.

-  ``GETDAY``: Receives an optional date-type argument and returns a
   long-type object that represents the day of the received date. If
   arguments are not received, a ``long``-type object is created that
   represents the current day.

-  ``GETHOUR``: Receives an optional ``date``-type argument and returns a
   ``long``-type object that represents the time of the received date. If
   no arguments are received, a ``long``-type object is created that
   represents the current time.

-  ``GETMINUTE``: Receives an optional ``date``-type argument and returns a
   ``long``-type object that represents the minutes of the received date.
   If no arguments are received, a ``long``-type object is created that
   represents the current minutes.

-  ``GETMILLISECOND``: Receives an optional ``date``-type argument and
   returns a ``long``-type object that represents the milliseconds of the
   received date. If no arguments are received, a ``long``-type object is
   created that represents the current milliseconds.

-  ``GETMONTH``: Receives an optional ``date``-type argument and returns a
   long-type object that represents the month of the received date. If no
   arguments are received, a ``long``-type object is created that
   represents the current month.

-  ``GETSECOND``: Receives an optional ``date``-type argument and returns a
   ``long``-type object that represents the seconds of the received date.
   If no arguments are received, a ``long``-type object is created that
   represents the current seconds.

-  ``GETYEAR``: Receives an optional ``date``-type argument and returns a
   ``long``-type object that represents the year of the received date. If
   no arguments are received, a ``long``-type object is created that
   represents the current year.

-  ``NOW``: This function does not accept any input argument, and creates a
   new ``date``-type object containing the current date.

-  ``TODATE``: This allows for text strings representing dates to be
   converted into date-type elements. Three ``string``-type arguments are
   given. The first represents a pattern to express dates. 
   This pattern follows the syntax
   specified by the class `SimpleDateFormat <https://docs.oracle.com/javase/8/docs/api/index.html?java/text/SimpleDateFormat.html>`_ of the Java API.
   The second will be a date expressed according to that pattern.
   The third one is a ``string``-type parameter which indicates the
   internationalization configuration that represents the “locale” of the
   date to process. As a result, a ``date``-type element equivalent to the
   specified date is returned.
