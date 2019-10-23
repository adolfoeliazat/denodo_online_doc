===============================
Changes in Virtual DataPort 7.0
===============================

This section lists the changed introduced in Virtual DataPort 7.0 compared to Virtual DataPort 6.0.

.. contents::
   :local:
   :backlinks: none

Privileges
==========


CONNECT Privilege
-----------------

.. #33924 Change default value of property added in feature #32318

In Denodo 7.0 you have to grant the CONNECT privilege to a user over a database in order to allow this user to access any of the elements of that database. The other privileges over the database or its elements are disregarded if the user/role does not have the privilege CONNECT.

In previous versions, the CONNECT privilege was only checked when establishing the connection.

Because of this change, grant the users/roles the CONNECT privilege over the databases in which they already have other privileges.

Privileges of Users with the Role "serveradmin"
-----------------------------------------------

Denodo 6.0 has an option to grant LDAP/Kerberos users that have the role "serveradmin" the permission to grant/revoke privileges to users and roles, including themselves. This was located in tab *Privileges* of the *Server configuration*.
    
In this version this option is not available. Therefore, you need to add these users to the "assignprivileges" group of Active Directory.

Changes in VQL Syntax
============================================================================

The statement ``ALTER DATABASE`` no longer admits the parameter ``I18N``.

|

.. #36563 - SELECT statements including PASSWORD in the context clause log the clear-text password

In the parameter ``PASSWORD`` of the ``CONTEXT`` it is now mandatory to indicate the password encrypted and add the token ``ENCRYPTED``. For example:

.. code-block:: sql

   SELECT * 
   FROM view1
   CONTEXT(USERNAME = 'admin', PASSWORD = 'd4GvpKA5BiwoGUFrnH92DNq5TTNKWw58I86PVH2tQIs/q1RH9CkCoJj57NnQUlmvgvvVnBvlaH8NFSDM0x5fWCJiAvyia70oxiUWbToKkHl3ztgH1hZLcQiqkpXT/oYd' ENCRYPTED)

This affects the views that:

-  The data is obtained from a data source of type JDBC, web service, BAPI or multidimensional data sources.

-  And the data source was created with the option "with pass-through session credentials".

-  And you want to connect to the source with other credentials than the ones you used to connect to the Server.

Changes in Date types
============================================================================

Denodo 7.0 has new types to represent datetime values.

-  ``localdate``: Represents a date as a year, a month and a day, without taking into account the time zone.
-  ``time``: Represents a time as hour, minute, second and fraction of a second.
-  ``timestamp``: Represents date and time, without taking into account the time zone.
-  ``timestamptz``: Represents date and time with time zone.
-  ``intervaldaysecond``: A duration in terms of days, hours, minutes and seconds.
-  ``intervalyearmonth``: A duration in terms of years and months.

The type ``date`` is now deprecated.

The section :doc:`data types for dates and timestamps<../../../../vdp/vql/language_for_defining_and_processing_data_vql/data_types/data_types_for_dates_timestamps_and_intervals>`
explains in detail these new date types.

We recommend modifying the definition of the views to use the new types because their behavior is more consistent and easier to use. The main reason being that, by default, "date" is a timestamp with time zone.

In Denodo 7.0, the way the administration tool displays the datetime values changes:

-  In previous versions, it depends on the setting *Internationalize query results* (wizard of the menu *Tools* > *Admin tool preferences* > *Locale*). If this check box is cleared, the tool displays these values using the format set by Java, for the country of the computer where the administration tool runs. If this check box is selected, the tool displays these values using the i18n of the query (as defined in the CONTEXT clause of the query or if not present, by the i18n of the Server).

-  In Denodo 7.0, datetime values are always displayed with the same format, regardless of the setting *Internationalize query results* or the language settings of the computer where the administration tool runs:

   -  "yyyy-MM-dd" (<year>-<month>-<day>) for values with type ``localdate`` and values with type ``date (deprecated)`` with subtype ``DATE``. For example: "2018-12-31".
   -  "HH:mm:ss.S" (<hour>:<minute>:<second>.<millisecond>) for values with type ``time`` and values with type ``date (deprecated)`` with subtype ``TIME``. For example: "23:15:05.15673".
   -  "yyyy-MM-dd HH:mm:ss.S" (<date> <time>) for values with type ``timestamp``. For example, "2018-12-31 23:58:05.15673".
   -  "yyyy-MM-dd HH:mm:ss.S XXX" (<date> <time> <time zone>) for values with type ``timestamptz`` and values with type ``date (deprecated)`` with subtype ``TIMESTAMP``. For example: "2018-12-31 23:59:05.15673 -08:00".

   The tool also uses these formats when storing the results of a query to a file. 
   
   If you want a query to display a datetime value with a different format, use the function :ref:`FORMATDATE`.     

Search Fields that Use the Type Date
------------------------------------

During the process of :doc:`exporting the VQL from a Denodo 6.0 installation <../../export_the_metadata_of_the_current_installation>`, we recommend setting the following properties:

.. code-block:: vql

    SET 'com.denodo.vdb.catalog.exportMigrationCompatibility' = 'true';
    SET 'com.denodo.vdb.catalog.exportMigrationCompatibility.migrateDateTypes' = 'true';

As explained in that section, with these properties, the Denodo server 6.0 analyzes the ``date`` fields to see if they can be exported to one of the new datetime types. However, even with these properties, there will be fields for which the Denodo server does not change the type because it cannot know which of the new types is the appropriate one.

This section explains how to search what views still have fields of type ``date`` and how to change them.

.. rubric:: Step #1

Execute these VQL statements on the new Denodo 7.0 installation:

.. code-block:: vql

   -- Materialized table that returns information about base views, interface views 
   -- and materialized tables, but not derived views.
   CREATE MATERIALIZED TABLE view_summary AS
   SELECT database_name
       ,name
       ,view_type
   FROM GET_VIEWS()
   WHERE view_type IN (0, 2, 3)
   CONTEXT('queryTimeout' = '120000');

   -- Materialized table that returns information about the columns of 
   -- type "date".
   CREATE MATERIALIZED TABLE view_date_column_summary AS
   SELECT database_name
       ,view_name
       ,column_name
       ,column_vdp_type
       ,column_sql_type
   FROM GET_VIEW_COLUMNS()
   WHERE column_vdp_type = 'date' CONTEXT('queryTimeout' = '120000');

.. rubric:: Step #2

Execute the following query. It returns the fields of type "date" of the base views, interface views and materialized views. We do not include derived views yet because in many of them, the type is inherited from an underlying view.

.. code-block:: sql

   SELECT v.database_name
       ,v.name
       ,c.column_name
       ,c.column_vdp_type
       ,c.column_sql_type
   FROM view_summary v
   INNER JOIN view_date_column_summary c ON v.database_name = c.database_name
       AND v.name = c.view_name CONTEXT('queryTimeout' = '120000');

.. rubric:: Step #3

For each field of the result, we recommend changing its type to one of the new data types:

-  For base views, select the type that matches the type of the column in the table of the underlying database.
-  For interface views, modify the definition of the field.
-  For materialized views, recreate them with one of the new types instead of "date".

.. rubric:: Step #4

Execute the following query. It returns the fields of type "date", including derived views.

.. code-block:: sql

   SELECT database_name
       ,view_name
       ,column_name
       ,column_vdp_type
       ,column_sql_type
   FROM GET_VIEW_COLUMNS()
   WHERE column_vdp_type = 'date' CONTEXT('queryTimeout' = '120000');

.. rubric:: Step #5

The type of a field of a derived view is still ``date`` if:

a. The field is defined as such by one of its underlying views.
b. Or if the field is the result of an expression that returns a ``date``. For example, if a field is the result of the expression ``TO_DATE``, the type of the field is ``date``. In this case, you should use one of the new functions instead (:ref:`TO_LOCALDATE`, :ref:`TO_TIME`, :ref:`TO_TIMESTAMP` or :ref:`TO_TIMESTAMPTZ` depending on the type of the input field).  

Backward Compatibility Properties
-----------------------------------

This section explains how to restore the behavior of previous versions of Denodo, regarding the values of type "date". Although they are available, we recommend not using them and instead, switch to the new datetime types.

-  The execution engine may implicitly convert a text value into a datetime value. In previous versions, the result of this conversion is a value of type "date". In Denodo 7.0, literals may be converted to one of the new datetime types, not to "date".

   To keep the behavior of previous versions, execute the following from the VQL Shell:

   .. code-block:: vql

      SET 'com.denodo.vdb.parser.datetime.literals.transformToOldToDateFunction' = 'true';

-  In Denodo 7.0, custom functions that in its Java source code have an input parameter or return an object of the Java class ``java.util.Calendar``, now return a "timestamptz" value. In previous versions, they return a "date".

   To keep the behavior of previous versions, execute the following from the VQL Shell:

   .. code-block:: vql

      SET 'com.denodo.vdb.compatibility.datetime.custom.mapCalendarToDateType' = 'true'

-  Temporary tables use the new datetime types. In previous versions they use "date". To keep the behavior of previous versions, execute the following from the VQL Shell:

   .. code-block:: vql

      SET 'com.denodo.vdb.parser.datetime.createsqltable.mapDateTimeToOldDateType' = 'true'

Changes in Stored Procedures
============================================================================

Procedure VIEW_DEPENDENCIES
-----------------------------

Now when you call this stored procedure and the target has a dependency of the type ``storedprocedure``, it returns the value
``Predefined Storedprocedure`` when this procedure is a predefined one.

Procedure GET_CATALOG_METADATA_WS
----------------------------------

Now you can call this stored procedure to get information related to Web Services.

Procedures GET_ELEMENTS and GET_VIEWS
--------------------------------------

These procedures now differentiate materialized tables from base views. In previous versions, these procedures considered materialized views as base views.

In the result of these procedures, the value of the field ``subtype`` is now ``materialized`` and the value of ``view_type`` is now ``3``.

Changes in Cost Optimization
============================================================================

As a result of some internal changes in the calculation of the cost optimizations, some query plans involving data
movements and other operations can change from the previous versions.

Subtype of DECIMAL Fields
=============================

When a new base view with a ``DECIMAL`` field is created, we are going to try to use the best subtype that fits the field
size, ``INTEGER``, ``BIGINT`` OR ``DECIMAL``.

To restore the behavior of previous versions, execute this from the VQL Shell:

.. code-block:: vql

   SET 'com.denodo.vdb.admin.model.wrapper.convertExactDecimaltoIntegerSubtype' = 'false'

JDBC Data Sources
=====================

Oracle
------

Starting with Denodo 7.0 GA, right after a JDBC data source opens a connection to Oracle, it executes the following command on that connection:

.. code-block:: sql

   ALTER SESSION SET NLS_DATE_FORMAT= 'YYYY-MM-DD';
   
By doing this, the pattern of the datetime values does not depend on the configuration of Oracle. Note that if in a previous version of Denodo you created a base view from a SQL query, the conditions of this query over datetime fields may not have the right format and the query will fail. In that case, modify the query so the datetime value follows the pattern "YYYY-MM-DD" (<year>-<month>-<day>). E.g. ``hire_date >= DATE '2018-02-03'``.

Apache Hive
-------------------

The Apache Hive driver is no longer distributed. 

The adapters for Apache Hive 0.7, 0.10, 0.11 and 0.12 have been removed. If you were using any of these, use the adapter for Apache Hive 0.13 instead.

.. note::  If you are connecting to Cloudera's or Hortonworks' Hive, use the adapter specific to them. Do not use the generic Apache Hive adapters.

Impala JDBC Drivers
-------------------

.. #29579 Impala JDBC drivers could not be included in Denodo Platform distribution due to license restrictions
.. 33607	Remove the JDBC drivers of Impala 2.1

Due to license restrictions, the JDBC driver of Impala is no longer distributed.

JDBC Drivers Location
-----------------------

Since version 7.0 the drivers that are not distributed should be copied to the next folder:

.. code-block:: vql

   /lib-external/jdbc-drivers/<databasename>

Denodo Connect Component: Salesforce Wrapper
============================================

Starting with Denodo 7.0, Denodo has a :ref:`native data source to Salesforce.com<Salesforce Sources>`. Users that retrieve data from Salesforce are advised to stop using the Denodo Connect Component and use the new data source because the *Denodo Connect* component will not be actively maintained.

Denodo JDBC Driver Compiled with Java 8
============================================================================

Starting with Denodo 7.0, the Denodo JDBC driver is compiled with Java
8. In Denodo 6.0, it is compiled with Java 7 and in Denodo 5.5, with Java 6.

The applications that use the JDBC driver and that run with earlier versions
of Java will have to update their version of Java to version 8 or higher. Otherwise, they will not be able to load this driver.

You cannot use the JDBC driver of earlier versions of Denodo to
connect to Denodo 7.0.