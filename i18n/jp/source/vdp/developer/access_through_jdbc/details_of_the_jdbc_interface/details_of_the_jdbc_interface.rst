==========================================
Details of the JDBC Interface
==========================================

This section describes information specific to the JDBC driver of Denodo

.. contents::
   :local:
   :backlinks: none
   :depth: 2

Description of Views and Their Fields
=====================================

The driver publishes the description of the views and their fields in the column "REMARKS" of the metadata of tables/views and their fields.

Retrieving the Content Type of Blob Values
==========================================

The driver makes available the :ref:`content type <Working with Blob Fields of Base Views>` of blob fields. The example below shows how to do it.

.. code-block:: java
   :caption: Retrieving the content type of a blob value

   ResultSet rs = stmt.executeQuery(...);
   ...
   com.denodo.vdb.jdbcdriver.VDBJDBCBlob blob = 
       (com.denodo.vdb.jdbcdriver.VDBJDBCBlob) rs.getBlob(index);
   String contentType = blob.getContentType();


Working with Datetime Values with the Denodo JDBC Driver
========================================================

The following subsections provide information about how to use the JDBC driver to work with the different datetime values.

.. contents::
   :local:
   :backlinks: none
   :depth: 1

Setting Datetime Values on Parameters of Prepared Statements
------------------------------------------------------------

The table below lists the method of the class PreparedStatement you have to invoke to set the value of a parameter (“?”), depending on the type of the value.

.. table:: Methods to set datetime parameter on a PreparedStatement

   +---------------------+------------------------------------------------------------------+
   | Type                | Method(s) to Set a Parameter of that Type on a PreparedStatement |
   +=====================+==================================================================+
   | localdate           | Any of these:                                                    |
   |                     |                                                                  |
   |                     | -  PreparedStatement.setObject(java.time.LocalDate)              | 
   |                     | -  PreparedStatement.setDate(java.sql.Date)                      |
   |                     |                                                                  |
   |                     | For example:                                                     | 
   |                     |                                                                  |
   |                     | .. code-block:: java                                             |
   |                     |                                                                  |
   |                     |    // Creating a LocalDate object                                |
   |                     |    setObject(1, java.time.LocalDate.of(2018,01,15));             |
   |                     |                                                                  |
   |                     |    // Creating a Date object                                     |
   |                     |    setDate(1, java.sql.Date.valueOf("2018-01-15"));              |
   |                     |                                                                  |
   |                     | According to the documentation of the class                      |
   |                     | `java.sql.Date`_, when using "setDate", the "Date" object must   |
   |                     | be "normalized" by setting the hours, minutes, seconds,          |
   |                     | and milliseconds to zero in the particular time zone with        |
   |                     | which the instance is associated. Because of this, using the     | 
   |                     | LocalDate object is simpler.                                     |
   +---------------------+------------------------------------------------------------------+
   | time                | Any of these:                                                    |
   |                     |                                                                  |
   |                     | - PreparedStatement.setObject(java.time.LocalTime)               | 
   |                     | - PreparedStatement.setTime(java.sql.Time)                       |
   |                     |                                                                  |
   |                     | For example:                                                     | 
   |                     |                                                                  |
   |                     | .. code-block:: java                                             |
   |                     |                                                                  |
   |                     |    // Creating a LocalTime object                                |
   |                     |    setObject(1, LocalTime.of(11, 58, 59, 123000000));            |
   |                     |                                                                  |
   |                     |    // Creating a java.sql.Time object                            |
   |                     |    setTime(2, java.sql.Time.valueOf("11:58:59"));                |
   |                     |                                                                  |
   |                     | According to the documentation of the class                      |
   |                     | `java.sql.Time`_, when using "setTime", the date components of   |
   |                     | the Time object should be set to the "zero epoch" value of       |
   |                     | January 1, 1970. Because of this, using the LocalTime object is  |
   |                     | simpler.                                                         |
   +---------------------+------------------------------------------------------------------+
   | timestamp           | PreparedStatement.setObject(java.time.LocalDateTime)             |
   |                     |                                                                  |
   |                     | For example:                                                     | 
   |                     |                                                                  |
   |                     | .. code-block:: java                                             |
   |                     |                                                                  |
   |                     |    setObject(1, java.time.LocalDateTime.of(                      |
   |                     |        2018, 01, 15, 23, 58, 59, 256000000))                     |
   |                     |                                                                  |
   |                     | The last parameter represents 256 milliseconds because the value |
   |                     | is in nanoseconds. In Denodo the maximum precision for           |
   |                     | timestamp,timestamptz and time is milliseconds, not nanoseconds. |
   |                     |                                                                  |
   |                     | PreparedStatement.setTimeStamp() must only be used with          |
   |                     | timestamptz values. Otherwise, the                               |
   |                     | query will fail unless the query has a cast from timestamptz to  | 
   |                     | timestamp; and even if you have it, in order for this to work,   |
   |                     | the parameter i18n of the connection URI has to match the i18n   |
   |                     | setting of the Denodo server.                                    |
   +---------------------+------------------------------------------------------------------+
   | date (deprecated)   | The same as for timestamptz (see below)                          |
   +---------------------+------------------------------------------------------------------+
   | timestamptz         | Any of these:                                                    |
   |                     |                                                                  |
   |                     | - PreparedStatement.setObject(java.time.OffsetDateTime)          |
   |                     | - PreparedStatement.setTimestamp(java.sql.Timestamp)             | 
   |                     |                                                                  |
   |                     | For example:                                                     | 
   |                     |                                                                  |
   |                     | .. code-block:: java                                             |
   |                     |                                                                  |
   |                     |    // Creating an OffsetDateTime object                          |
   |                     |    setObject(1, OffsetDateTime.parse(                            |
   |                     |        "2018-01-01T21:15:00+01:00"))                             |
   |                     |                                                                  |
   |                     |    // Creating a Timestamp object                                |
   |                     |    SimpleDateFormat sdf =                                        |
   |                     |        new SimpleDateFormat("yyyy-MM-dd HH:mm:ss XXX");          |
   |                     |    sdf.setTimeZone("GMT");                                       |
   |                     |    setTimestamp(                                                 |
   |                     |          1                                                       |
   |                     |        , sdf.parse("1982-12-13 01:59:59 +0000"));                |
   +---------------------+------------------------------------------------------------------+
   | interval_year_month | setObject(java.time.Period)                                      |
   |                     |                                                                  |
   |                     | For example:                                                     | 
   |                     |                                                                  |
   |                     | .. code-block:: java                                             |
   |                     |                                                                  |
   |                     |    // Equivalent to INTERVAL '145-11' YEAR TO MONTH              |
   |                     |    setObject(1, Period.ofYears(145).plusMonths(11));             |
   |                     |                                                                  |
   |                     |    // Equivalent to INTERVAL '145' YEAR                          |
   |                     |    setObject(Period.ofYears (145));                              |
   +---------------------+------------------------------------------------------------------+
   | interval_day_second | setObject(java.time.Duration)                                    |
   |                     |                                                                  |
   |                     | For example:                                                     | 
   |                     |                                                                  |
   |                     | .. code-block:: java                                             |
   |                     |                                                                  |
   |                     |    // Equivalent to INTERVAL '4 5:12' DAY TO MINUTE              |
   |                     |    setObject(Duration.ofDays(4).plusHours(5).plusMinutes(12));   |
   |                     |                                                                  |
   |                     |    // Equivalent to INTERVAL '4 5:12:10.222' DAY TO SECOND       |
   |                     |    setObject(Duration.parse("P4DT5H12M10.222S"));                |  
   +---------------------+------------------------------------------------------------------+

How the Driver Reports the Datetime and Interval Types
------------------------------------------------------

The tables below list how the JDBC driver reports each datetime type.

.. csv-table:: 
   :header: "Type Name in Denodo", "Type Name Returned by the Method ResultSetMetaData.getColumnTypeName()", "Value Returned by the Method ResultSetMetaData.getColumnType(int)"

   "localdate", "DATE", "91"
   "time", "TIME", "92"
   "timestamp", "TIMESTAMP", "93"
   "date (deprecated)", "TIMESTAMP_WITH_TIMEZONE", "2014"
   "timestamptz", "TIMESTAMP_WITH_TIMEZONE", "2014"
   "interval_year_month", "INTERVAL_YEAR_MONTH", "2020"
   "interval_day_second", "INTERVAL_DAY_SECOND", "2021"

The types ``date`` and ``timestamptz`` are reported with the same type (TIMESTAMP WITH TIMEZONE) so a client application cannot distinguish them. This is intentional, to facilitate the migration from versions prior to Denodo 7.0. Client applications do not need to distinguish between these types and treat both as ``timestamptz``.

The codes for the types ``interval_year_month`` and ``interval_day_second`` are defined by Denodo because they are not part of the JDBC API.

|

.. table::

   +---------------------+-------------------------------------------+------------------------------------+
   | Type Name in Denodo | Result of                                 | Java Class of the Objects Returned |
   |                     | ResultSetMetaData.getColumnClassName(int) | by the class ResultSet.getObject() |
   +=====================+===========================================+====================================+
   |localdate            | java.sql.Date                             | java.sql.Date                      |
   +---------------------+-------------------------------------------+------------------------------------+
   | time                | java.sql.Time                             | java.sql.Time                      |
   +---------------------+-------------------------------------------+------------------------------------+
   | timestamp           | java.sql.Timestamp                        | java.sql.Timestamp                 |
   +---------------------+-------------------------------------------+------------------------------------+
   | date (deprecated)   | java.sql.Timestamp                        | java.sql.Timestamp                 |
   +---------------------+-------------------------------------------+------------------------------------+
   | timestamptz         | java.sql.Timestamp                        | java.sql.Timestamp                 |
   +---------------------+-------------------------------------------+------------------------------------+
   | interval_year_month | java.lang.Long                            | java.lang.Long                     |
   |                     |                                           |                                    |
   |                     |                                           | Invoke ``java.time.Period.ofMonths |
   |                     |                                           | (value)`` to convert this value    |
   |                     |                                           | into a ``java.time.Period`` object.|
   |                     |                                           |                                    |
   |                     |                                           | To obtain a ``Duration`` object    |
   |                     |                                           | from the driver, invoke            |
   |                     |                                           | ``ResultSet.getObject(col,         |
   |                     |                                           | java.time.Period.class)``.         |
   +---------------------+-------------------------------------------+------------------------------------+
   | interval_day_second | java.lang.Long                            | java.lang.Long                     |
   |                     |                                           |                                    |
   |                     |                                           | Invoke ``java.time.Duration.       |
   |                     |                                           | ofMillis(value)`` to convert this  |
   |                     |                                           | value into a ``java.time.Duration``|
   |                     |                                           | object.                            |
   |                     |                                           |                                    |
   |                     |                                           | To obtain a ``Duration`` object    |
   |                     |                                           | from the driver, invoke            |
   |                     |                                           | ``ResultSet.getObject(col,         |
   |                     |                                           | java.time.Duration.class)``.       |
   +---------------------+-------------------------------------------+------------------------------------+


.. csantos@2018/03/20 commented because using getDate and getTime seems to provide more problems than anything else.

    Getting and Setting Datetime and Interval Values Using the JDBC driver
    ----------------------------------------------------------------------

    JDBC API provides three classes that wrap java.util.Date to allow identify the value as SQL DATE, TIME or TIMESTAMP

    ResultSet, from JDBC API, provides methods to get datetime values:

    - getDate(int [,Calendar]): returns java.sql.Date.
      Uses Calendar to calculate the appropriated milliseconds value to represent the date value from the database. That is: printing the returned value using the TimeZone of the provided calendar will print the exact date that is returning in the database.
    - getTime(int [,Calendar]): returns java.sql.Time.
      Uses Calendar to calculate appropriated milliseconds value to represent the time value from the database at epoch date (1970-01-01)
    - getTimestamp(int [,Calendar]): returns java.sql.Timestamp.
      Uses Calendar to calculate appropriated milliseconds value to represent the timestamp in the underlying database value does not store time zone information.
    - getObject(int [,Class]: returns the value. 
      If class is specified try to convert the value to return an instance of that class.

  
    Setting values with Denodo JDBC Driver datetime examples
    ------------------------------------------------------------------------------------------------
    This section explains with examples how to use the new datetime types with the Denodo JDBC Driver.

    Setting localdate values with Denodo JDBC Driver example

    .. code-block:: java
       :name: Setting localdate values with Denodo JDBC Driver example
       
        //OPTION 1: use setDate, or setObject assuming JVM timezone
        pst.setDate(1, Date.valueOf("2017-01-18"));
        pst.setObject(1, Date.valueOf("2017-01-18"));
        
        //OPTION 2: use setDate and a custom TimeZone
        Calendar calendar =    Calendar.getInstance(TimeZone.getTimeZone("America/Los_Angeles"));
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        sdf.setTimeZone(calendar.getTimeZone());
        pst.setDate(1, sdf.parse("2017-01-18"), calendar);
        
        //OPTION 3: use setObject with java.time.LocalDate
        pst.setObject(1, LocalDate.parse("2017-01-18"));    


    Setting timestamptz values with Denodo JDBC Driver example

    .. code-block:: java
       :name: Setting timestamptz values with Denodo JDBC Driver example
       
        //OPTION 1: use setTimestamp, or setObject assuming JVM timezone GMT+1
        pst.setTimestamp(1, Timestamp.valueOf("2017-01-01 21:15:00"));
        pst.setObject(1, Timestamp.valueOf("2017-01-01 21:15:00"));
        //----> Sets 2017-01-01 21:15:00 +01:00
        
        //OPTION 2: use setTimestamp and a custom TimeZone
        Calendar calendar = Calendar.getInstance(TimeZone.getTimeZone("America/Los_Angeles"));
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        sdf.setTimeZone(calendar.getTimeZone());
        pst.setTimestamp(1, sdf.parse("2017-01-01 21:15:00"), calendar);
        //----> Sets 2017-01-01 21:15:00 -08:00
        
        //OPTION 3: use setObject with java.time.OffsetDateTime
        pst.setObject(1, OffsetDateTime.parse("2017-01-01T21:15:00+01:00"));
        

    There is a problem regarding timestamp values. With the setTimestamp method it is not possible to determine if we have to set a timestamptz or a timestamp, and the driver assumes timestamptz

    If using setTimestamp: provide a value that when casted from timestamptz to timestamp results in the desired timestamp. Take into account query time zone. It is required to explicitly cast to in the query.


    Setting timestamp values with Denodo JDBC Driver example

    .. code-block:: java
       :name: Setting timestamp values with Denodo JDBC Driver example
       
        //OPTION 1: use setTimestamp, or setObject assuming JVM timezone is equal to query time zone (GMT+1)
        pst.setTimestamp(1, Timestamp.valueOf("2017-01-01 21:15:00"));
        pst.setObject(1, Timestamp.valueOf("2017-01-01 21:15:00"));
        //----> Sets 2017-01-01 21:15:00 +01:00 that when casted to timestamp will result in 2017-01-01 21:15:00
        
        //OPTION 2: use setTimestamp, JVM time zone different from query time zone (America/Los_Angeles)
        Calendar calendar = Calendar.getInstance(TimeZone.getTimeZone("America/Los_Angeles"));
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        sdf.setTimeZone(calendar.getTimeZone());
        pst.setTimestamp(1, sdf.parse("2017-01-01 21:15:00"), calendar);
        //----> Sets 2017-01-01 21:15:00 -08:00 that when casted to timestamp will result in 2017-01-01 21:15:00
        
        //OPTION 3: use setObject with java.time.LocalDateTime
        pst.setObject(1, LocalDateTime.parse("2017-01-01T21:15:00"));
        
        
    Setting intervaldaysecond values with Denodo JDBC Driver example

    .. code-block:: java
       :name: Setting intervaldaysecond values with Denodo JDBC Driver example
       
        //Use setObject with java.time.Duration
        pst.setObject(1, Duration.ofDays(1).plusHours(15);
        
    Setting intervalyearmonth values with Denodo JDBC Driver example

    .. code-block:: java
       :name: Setting intervalyearmonth values with Denodo JDBC Driver example
       
        //Use setObject with java.time.Period
        pst.setObject(1, Period.ofMonths(16);



Obtaining the Names of Elements Inside a Struct (Register)
==========================================================

The JDBC driver of Denodo transforms compound values to classes of the JDBC API:

-  Converts values of type ``register`` to `java.sql.Struct`_
   objects.
-  Converts values of type ``array`` to `java.sql.Array`_ objects.
-  ``java.sql.Array`` objects are arrays of ``Struct`` objects.

The standard JDBC API provides methods to obtain the values inside
``java.sql.Struct`` objects (i.e. inside ``register`` fields). However,
it does not offer any way of obtaining the name of the subfields of a
``Struct`` or obtaining these values by the name of the subfield.

This section explains how to, using the Denodo JDBC driver:

#. Obtain the name of the subfields of a ``Struct`` object.
#. Obtain a value of a subfield by its name, instead of its position
   inside the ``register``.

For example, let us say that you have an application that executes a
query that returns a ``register`` field whose subfields are
``last_name`` and ``first_name``. For each row, the result set returns a
``Struct`` object. To obtain the values of each ``Struct`` object, the
application has to invoke the method ``Struct.getAttributes()``, which
returns an array of two values: the last name and the first name. If
later, you modify this register to add a subfield (e.g. ``telephone``),
the array returned by the method ``Struct.getAttributes()`` will have
three elements instead of two. In addition, if the first element of the
array is now the telephone and not the last name, the application will
obtain invalid data.

To avoid this sort of maintainability issues you may want to use the
classes of the Denodo JDBC API to obtain the values of a ``Struct`` by
name and not by its position in the register. This will make your
application more robust to changes.

The example below shows how to do this.

.. code-block:: java
  :caption: Obtaining the name of a value inside a Struct object

   import com.denodo.vdb.jdbcdriver.printer.Field;
   import com.denodo.vdb.jdbcdriver.VDBJDBCResultSetMetaData;
   import com.denodo.vdb.vdbinterface.common.clientResult.vo.descriptions.type.RegisterVO;
   import com.denodo.vdb.vdbinterface.common.clientResult.vo.descriptions.type.RegisterValueVO;
   
   ...
   
   public static void main(String[] args)
           throws Exception {
   
       /*
        * The method getConnection() returns a Connection to Virtual 
        * DataPort
        */
       Connection connection = getConnection();
       Statement st = connection.createStatement();
       String query = "SELECT * FROM view_with_compound_fields";
       ResultSet rs = st.executeQuery(query);
   
       /* 
        * The classes 'VDBJDBCResultSetMetaData' and 'Field' are part 
        * of the Denodo JDBC API. They do not belong to the standard 
        * JDBC API.
        */
       VDBJDBCResultSetMetaData metaData = 
               (VDBJDBCResultSetMetaData) rs.getMetaData();
       Field[] fields = metaData.getFields();
       while (rs.next()) {
   
           int columnCount = metaData.getColumnCount();
           for (int i = 1; i <= columnCount; i++) {
   
               Object value = rs.getObject(i);
               if (value != null) {
   
                   if (metaData.getColumnType(i) == Types.STRUCT) {
                       /*
                        * The JDBC API represents the values of type 
                        * 'register' as 'Struct' objects.
                        */
   
                       /* 
                        * The classes 'RegisterVO' and                                        
                        * 'RegisterValueVO' are part of the Denodo JDBC
                        * API. They do not belong to the standard Java
                        * API.
                        */
                       RegisterVO vdpType = 
                          ((RegisterVO) fields[i - 1].getVdpType());
                       List<RegisterValueVO> registerSubTypes = 
                           vdpType.getElements();
                       Struct struct = (Struct) value;
                       Object[] structValues = struct.getAttributes();
                       String firstName = null, lastName = null;
                       for (int j=0; j < registerSubTypes.size(); j++) {
                           /*
                            * The variable 'registerSubTypes' 
                            * contains the names of the names of the 
                            * subfields.
                            */
   
                           String subFieldName = 
                               registerSubTypes.get(j).getName();
                           switch (subFieldName) {
                           case "first_name":
                               firstName = (String) structValues[j];
                               break;
   
                           case "last_name":
                               lastName = (String) structValues[j];
                               break;
                           }
                           /*
                            * ...
                            */
                       }
                   } else if (metaData.getColumnType(i)==Types.ARRAY) {
                       /* 
                        * The JDBC API represents the values of type 
                        * 'array' as 'Array' objects.
                        */
                       Object[] register = 
                           (Object[]) rs.getArray(i).getArray();
                       for (Object o : register) {
                           /*
                            * In the Denodo JDBC API, the content of an 
                            * 'Array' is an array of 'Struct' objects.
                            */
   
                           Struct s = (Struct) o;
                           /*
                            * ...
                            */
                       }
                   } // else ...
               }
           }
       }
   
       /*
        * Close ResultSet, Statement and Connection.
        */
   }

.. _java.sql.Date: https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Date.html
.. _java.sql.Time: https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Time.html
.. _java.sql.Struct: https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Struct.html
.. _java.sql.Array: https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Array.html