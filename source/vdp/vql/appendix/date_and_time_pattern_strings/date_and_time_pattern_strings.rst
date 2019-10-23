=============================
Date and Time Pattern Strings
=============================

Virtual DataPort uses the 
`date and time Java patterns <https://docs.oracle.com/javase/8/docs/api/index.html?java/text/SimpleDateFormat.html>`_
to specify date and time formats. In these patterns,
the letters of the first column represent parts of a date.

.. table:: Java Date and time patterns used in Virtual DataPort
   :name: Java Date and time patterns used in Virtual DataPort

   +--------------------+---------------------------+--------------------+--------------------+
   | Symbol             | Date or Time Component    | Presentation       | Examples           |
   +====================+===========================+====================+====================+
   | G                  | Era designator            | Text               | AD                 |
   +--------------------+---------------------------+--------------------+--------------------+
   | y                  | Year                      | Year               | 1996; 96           |
   +--------------------+---------------------------+--------------------+--------------------+
   | M                  | Month in year             | Month              | July; Jul; 07      |
   +--------------------+---------------------------+--------------------+--------------------+
   | w                  | Week in year              | Number             | 27                 |
   +--------------------+---------------------------+--------------------+--------------------+
   | W                  | Week in month             | Number             | 2                  |
   +--------------------+---------------------------+--------------------+--------------------+
   | D                  | Day in year               | Number             | 189                |
   +--------------------+---------------------------+--------------------+--------------------+
   | d                  | Day in month              | Number             | 10                 |
   +--------------------+---------------------------+--------------------+--------------------+
   | F                  | Day of week in month      | Number             | 2                  |
   +--------------------+---------------------------+--------------------+--------------------+
   | E                  | Day in week               | Text               | Tuesday; Tue       |
   +--------------------+---------------------------+--------------------+--------------------+
   | a                  | Am/pm marker              | Text               | PM                 |
   +--------------------+---------------------------+--------------------+--------------------+
   | H                  | Hour in day (0-23)        | Number             | 0                  |
   +--------------------+---------------------------+--------------------+--------------------+
   | k                  | Hour in day (1-24)        | Number             | 24                 |
   +--------------------+---------------------------+--------------------+--------------------+
   | K                  | Hour in am/pm (0-11)      | Number             | 0                  |
   +--------------------+---------------------------+--------------------+--------------------+
   | h                  | Hour in am/pm (1-12)      | Number             | 12                 |
   +--------------------+---------------------------+--------------------+--------------------+
   | m                  | Minute in hour            | Number             | 30                 |
   +--------------------+---------------------------+--------------------+--------------------+
   | s                  | Second in minute          | Number             | 55                 |
   +--------------------+---------------------------+--------------------+--------------------+
   | S                  | Millisecond               | Number             | 978                |
   +--------------------+---------------------------+--------------------+--------------------+
   | z                  | Time zone                 | General time zone  | Pacific Standard   |
   |                    |                           |                    | Time; PST;         |
   |                    |                           |                    | GMT-08:00          |
   +--------------------+---------------------------+--------------------+--------------------+
   | Z                  | Time zone                 | RFC 822 time zone  | -0800              |
   +--------------------+---------------------------+--------------------+--------------------+
   | .. raw:: html      | Escape character for text |                    | (not displayed)    |
   |                    |                           |                    |                    |
   |    '               |                           |                    |                    |
   +--------------------+---------------------------+--------------------+--------------------+
   | .. raw:: html      | Single inverted comma     | Literal            | .. raw:: html      |
   |                    |                           |                    |                    |   
   |    ''              |                           |                    |    '               |
   +--------------------+---------------------------+--------------------+--------------------+

In the table above, different values are used to indicate the
arrangement of reserved characters. The specific output format depends
on the number of times the different elements are repeated in each
position:

-  ``Text``: use 4 or more characters to specify complete form; less
   than 4 characters to use the abbreviated form. For instance, if a
   date pattern specifies EEEE in the day of the week position, it
   indicates that day of the week should be shown using the complete
   form (e.g. "Monday") instead of the abbreviated form (e.g. "Mon").
-  ``Number``: it always uses the minimum number of digits possible. 0s
   are added to the left of the shortest numbers if required. The year
   is a special case: if the number of "y" is 2, the year is shortened
   to 2 digits.
-  ``Text & Number``: 3 or more characters to represent it as text;
   otherwise a number is used. For instance, if a date pattern specifies
   MMM in the month position, it indicates that months should be shown
   using the text name (e.g. "Jul"). If the pattern specifies MM, the
   month will be shown as a number.
   In a date format the characters that are not found in the ranges
   ``['a'..'z']`` or ``['A'..'Z']`` are considered constants, i.e.
   characters such as ``':'``, ``'.'``, ``' '``, ``'#'`` and ``'@'``
   appear in the resulting date, although they are not in inverted
   commas in the format pattern.
