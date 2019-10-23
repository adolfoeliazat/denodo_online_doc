================================================
Managing Internationalization Configurations
================================================

Virtual DataPort can work with data from a group of different
countries/locations. An internationalization configuration, represented
by a map, exists for each of the countries/locations from which data
managed by Virtual DataPort may come. Various configurable parameters exist for
each of the locations contemplated. Some examples of configurable
parameters are: symbol used as separators into decimal numbers and into
thousands, date format, etc.

Although Virtual DataPort includes internationalization configurations
already created for the most common situations, creating new
configurations is a very simple process. This section describes this
process in detail.

The internationalization parameters of a location can be divided into
various groups. The different groups are mentioned below, and each of
the parameters comprising same are described in detail:

.. note:: The internationalization parameters are case-insensitive. For
   instance, “timeZone” and “timezone” correspond to the same key.


-  Generic parameters

   -  **language** - Indicates the language used in this location. It is a
      valid ISO language code. These codes contain two letters in lower
      case as defined in `ISO-639 <http://www.loc.gov/standards/iso639-2/php/code_list.php>`_. Examples: ``es``
      (Spanish), ``en`` (English), ``fr`` (French).
   -  **country** - Specifies the country associated with this location. It
      is a valid ISO country code. These codes contain two letters in upper
      case, as defined by `ISO-3166 <https://www.iso.org/iso-3166-country-codes.html>`_. Examples: ``ES``
      (Spain), ``ES_EURO`` (Spain with ``EURO`` currency), ``GB``
      (England), ``FR`` (France), ``FR_EURO`` (France with ``EURO``
      currency), ``US`` (United States).
   -  **timeZone** - Indicates the time zone of the location (e.g.
      ``Europe/Madrid`` for Spain = GMT+01:00 = MET = CET).

-  **Configuration of dates**: Configuration of type ``date``.

   -  **datePattern** - Indicates the format for dates. This parameter is a
      pattern with the same syntax as the date related methods of the Java
      classes ``java.text.DateFormat`` and
      ``java.text.SimpleDateFormat``.
      :ref:`Java Date and time patterns used in Virtual DataPort` lists the
      meaning of each of the reserved characters used in a date pattern.
      Example of a date pattern: ``d-MMM-yyyy H'h' m'm'``.
   -  **datesubtypepattern** - Indicates the format for dates whose “type”
      in the “source type properties” is ``DATE``.
   -  **timesubtypepattern** - Indicates the format for dates whose “type”
      in the “source type properties” is ``TIME``.

-  **Configuration of real numbers**: Facilitates the configuration of the
   data types ``float``, ``double`` and ``decimal``.

   -  **doubleDecimalPosition** - Indicates the number of decimal positions
      to be used to represent a decimal value.
   -  **doubleDecimalSeparator** - Represents the decimal separator used to
      print a decimal value.
   -  **doubleGroupSeparator** - Specifies the group separator for decimal
      values.

The following figure contains the statement required to create the
internationalization configuration ``us_pst``, (U.S Pacific Standard
Time):



.. code-block:: vql
   :caption: Internationalization configuration us_pst
   :name: Internationalization configuration us_pst

   CREATE MAP I18N i18n_us_pst (
       'country' = 'US'
       'datepattern' = 'MMM d, yyyy h:mm:ss a'
       'doubledecimalposition' = '2'
       'doubledecimalseparator' = ''
       'doublegroupseparator' = ''
       'language' = 'en'
       'timepattern' = 'DAY'
       'timezone' = 'PST'
   );

Replacing an I18n Map
=====================

To replace an i18n map, execute the statement
``CREATE OR REPLACE MAP I18N <map name>``

To execute this operation, one of the following conditions must be met:

1. The user that executes the statement is an administrator

#. Or, the user that executes the statement is a normal user and
   administrator of the following databases:

   -  The databases in the Server that are not configured to use VCS and
      that depend on the i18n map being replaced.
   -  At least one database that is configured to use VCS, for each remote
      database that depends on this i18n map.


Example of the second condition:

-  There is a custom i18n map called ``mymap``.
-  User1 is a normal user and an administrator of the databases
   ``MyLocalDb``, ``Db1_user1`` and ``Db2_user1``. The i18n map of these
   databases is ``mymap``.
   ``Db1_user1`` and ``Db2_user1`` are configured to use VCS and they
   are linked to the remote databases ``Db1`` and ``Db2`` respectively.
   ``MyLocalDb`` is not configured to use VCS.
-  ``User2`` is a normal user and an administrator of the databases
   ``Db1_user2`` and ``Db2_user2``. The i18n map of these databases is
   ``mymap``.
   ``Db1_user2`` and ``Db2_user2`` are configured to use VCS and they
   are linked to the remote databases ``Db1`` and ``Db2`` respectively.

In this scenario, User1 can execute
``CREATE OR REPLACE MAP I18N mymap``.

User2 cannot execute ``CREATE OR REPLACE MAP I18N mymap`` because it is
not an administrator of the databases that depend on ``mymap`` and are
not configured to use VCS. That is, ``MyLocalDB``.

In databases with VCS configured, when the user performs a check-out of
a database, the Server executes a
``CREATE OR REPLACE MAP <map of the database>``.

Deleting an I18n Map
====================

To delete an i18n map, execute the statement
``DROP MAP I18N <map name>``

This statement will fail if the map is referenced from another element
such as views and databases (all the databases and views have an i18n).

If you want to force the deletion of the map, execute
``DROP MAP I18N <map name> CASCADE``

This will delete the map and also, the views whose i18n map is the one
you are deleting.

If there is a database whose i18n is the one you are deleting, this
command will fail even if you add the ``CASCADE`` modifier.
