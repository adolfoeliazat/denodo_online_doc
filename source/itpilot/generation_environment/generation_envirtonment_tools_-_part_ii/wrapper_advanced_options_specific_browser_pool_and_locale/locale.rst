======
Locale
======

As it was commented in section :ref:`Generation Tool Global Preferences`,
this area is used to configure the locale information of the wrapper and
the default locale assigned to new Extractor components. It incorporates
support for the integration of information from different countries or
geographic areas, expressing the output data in the formats expected by
the country in question.

-  Wrapper locale: the locale used to type the wrapper input parameters
   (the parameters received by the Init component), and the locale of
   the base view when deploying the wrapper to a Virtual DataPort server
   (or generating its VQL file).
-  Default locale for Extractor components: the default locale that will
   be selected when creating new Extractor components in the current
   wrapper.

This configuration determines aspects such as decimal and thousands
separator symbols, date format, etc. ITPilot includes
internationalization configurations for the most common zones. The zone
names correspond with the codes defined in the standard `ISO-639 <https://www.w3.org/WAI/ER/IG/ert/iso639.htm>`_.
Examples: ``ES_EURO`` (Spain), ``GB`` (Great Britain), …



In the :file:`{<DENODO_HOME>}/setup/vdp/metadata/properties/i18n` path there
is a file with the configured parameters for every zone used by the
Generation tool. The internationalization parameters of a location can
be divided into various groups. The different groups are mentioned
below, and each of the parameters comprising it that are applicable to
ITPilot is described in detail:



.. note:: The internationalization parameters are case-insensitive. For
   instance, “timeZone” and “timezone” correspond to the same key.


-  **Generic parameters**

   -  **language** - Indicates the language used in this location. It
      should be a valid ISO language code. These codes contain two letters
      in lower case as defined in the standard `ISO-639 <https://www.w3.org/WAI/ER/IG/ert/iso639.htm>`_. Examples: ``es``
      (Spanish), ``en`` (English), ``fr`` (French).
   -  **country** - Specifies the country associated with this location. It
      should be a valid ISO country code. These codes contain two letters
      in upper case, as defined by the standard `ISO-3166 <http://www.chemie.fu-berlin.de/diverse/doc/ISO_3166.html>`_.
      Examples: ``ES`` (Spain), ``ES_EURO`` (Spain with ``EURO`` currency),
      ``GB`` (England), ``FR`` (France), ``FR_EURO`` (France with ``EURO``
      currency), ``US`` (United States).
   -  **timeZone** - Indicates the time zone of the location
      (e.g. ``Europe/Madrid`` for Spain = GMT+01:00 = MET = CET).

-  *Configuration of dates*: Configuration of *date* data type.

   -  **datePattern** - Indicates the format for dates. To specify the
      format for dates, ASCII characters are used to indicate the different
      units of time. The table `Reserved Characters for Date Format`_ shows the
      meaning of each of the reserved characters used in a date format,
      their presentation and an example of use. Example of a date format:
      ``d-MMM-yyyy H'h' m'm'``.      
      This pattern follows the syntax
      specified by the class `SimpleDateFormat <https://docs.oracle.com/javase/8/docs/api/index.html?java/text/SimpleDateFormat.html>`_.
      of the Java API

.. table:: Reserved Characters for Date Format
   :name: Reserved Characters for Date Format
    
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

.. 

   In the table above, different values are used to indicate the presentation of reserved characters. The specific output format depends on the number of times the different elements are repeated:

   -  ``Text``: use 4 or more characters to use complete form; less than 4
      characters to use the abbreviated form. For instance, if a date
      pattern specifies EEEE in the day of the week position, it indicates
      that day of the week should be shown using the complete form (e.g.
      ‘Monday’) instead of the abbreviated form (e.g. ‘Mon’).
   -  ``Number``: uses the minimum number of digits possible. The zeros are
      added to the left of the shortest numbers. The year is a special
      case: if the number of ‘y’ is 2, the year is shortened to 2 digits.
   -  ``Text & Number``: 3 or more characters to represent it as text;
      otherwise a number is used. For instance, if a date pattern specifies
      MMM in the month position, it indicates that months should be shown
      using the text name (e.g. ‘Jul’). If the pattern specifies MM, the
      month will be shown as a number.


   In a date format the characters that are not found in the ranges ['a'..'z'] or ['A'..'Z'] are considered text in inverted commas, i.e. characters such as ':', '.', ' ', '#' and '@' appear in the resulting date, although they are not in inverted commas in the format pattern.

-  **Configuration of real numbers**: Facilitates the configuration of the
   data types ``float`` and ``double``.

   -  **doubleDecimalPosition** - Indicates the number of decimal positions
      to be used to represent a ``double``-type or ``float``-type value
      (real numbers).
   -  **doubleDecimalSeparator** - Represents the decimal separator used in
      a real number.
   -  **doubleGroupSeparator** - Specifies the group separator for real
      numbers.


.. note:: In the example used in this document, the recommended ‘locale’
   configuration of the web process is “US\_PST: English (United States)”,
   because of the date format used by the web source.