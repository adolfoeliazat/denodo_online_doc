=================================================
Using Datetime Values in Denodo Stored Procedures
=================================================

This section explains how to develop stored procedures that have input and/or output parameters of a datetime type.


When you develop a stored procedure, the main class of the procedure has to override the method `getParameters() <https://community.denodo.com/docs/html/browse/7.0/vdp/javadoc/index.html?com/denodo/vdb/engine/storedprocedure/StoredProcedureExecutor.html>`_, which defines the output schema of the procedure. This method has to return an array of `StoredProcedureParameter <https://community.denodo.com/docs/html/browse/7.0/vdp/javadoc/index.html?com/denodo/vdb/engine/storedprocedure/StoredProcedureParameter.html>`_ objects. Each object represents an input parameter or an output one.

The second parameter of the constructors of the class ``StoredProcedureParameter`` is the parameter ``type``. The value of this parameter is a constant of the class `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_. 

The table below displays the mapping between:

-  A Denodo data type and the constant of the class ``java.sql.Types`` that has to be passed to the constructor of ``StoredProcedureParameter``.
-  A Denodo data type and the class of the Java object that the stored procedure has to return.

.. csv-table:: 
   :header: "Denodo Data Type", "Constant of the Class java.sql.Types", "Java Class of the Input Parameters", "Java Class of the Output Values"

   "localdate", "Types.DATE", "java.sql.Date", "java.time.LocalDate or java.sql.Date"
   "time", "Types.TIME", "java.sql.Time", "java.time.LocalTime or java.sql.Time"
   "timestamp", "Types.TIMESTAMP", "java.sql.Timestamp", "java.time.LocalDateTime or java.sql.Timestamp"
   "timestamptz", "Types.TIMESTAMP_WITH_TIMEZONE", "java.sql.Timestamp", "java.time.OffsetDateTime or java.sql.Timestamp"
   "intervalyearmonth", "JDBCTypeUtil.INTERVAL_YEAR_MONTH", "java.time.Period", "java.time.Period"
   "intervaldaysecond", "JDBCTypeUtil.INTERVAL_DAY_SECOND", "java.time.Duration", "java.time.Duration"
   
JDBCTypeUtil = com.denodo.vdb.vdbinterface.common.clientResult.vo.descriptions.type.util.JDBCTypeUtil

For example, if you want to have an input parameter of type ``time``, the value of the parameter ``type`` has to be ``Types.TIME``.

At runtime, when the procedure is executed, the execution engines invokes the method ``doCall(Object[] inputValues)`` of the procedure. The Java class of the objects of the array ``inputValues`` depends on the value of the ``type`` declared in the constructor of ``StoredProcedureParameter``.

.. code-block:: java
   :name: Definition of the parameters of a stored procedure with new datetime fields
   
   @Override
   public StoredProcedureParameter[] getParameters() {

       return new StoredProcedureParameter[] {
           
             // Input parameter of type "localdate"
             new StoredProcedureParameter("date_field", Types.DATE, StoredProcedureParameter.DIRECTION_IN)
             
             // Input parameter of type "timestamp"
           , new StoredProcedureParameter("timestamp_field", Types.TIMESTAMP, StoredProcedureParameter.DIRECTION_IN)
           
             // Input parameter of type "timestamptz"
           , new StoredProcedureParameter("timestamptz_field", Types.TIMESTAMP_WITH_TIMEZONE, StoredProcedureParameter.DIRECTION_IN)
           
             // Input parameter of type "time"
           , new StoredProcedureParameter("time_field", Types.TIME, StoredProcedureParameter.DIRECTION_IN)
           
             // Input/output parameter of type "interval_day_second"
           , new StoredProcedureParameter("intervaldaysecond_field", JDBCTypeUtil.INTERVAL_DAY_SECOND, StoredProcedureParameter.DIRECTION_INOUT)
           
             // Output parameter of type "interval_year_month"
           , new StoredProcedureParameter("intervalyearmonth_field", JDBCTypeUtil.INTERVAL_YEAR_MONTH, StoredProcedureParameter.DIRECTION_OUT)
        };
   }

The listing below shows a sample implementation of ``doCall(...)``. This method is invoked by the execution engine to run the procedure.

.. code-block:: java
   :caption: Example of doCall()
   
   public void doCall(Object[] inputValues) {

       Date sqlDate = (Date) inputValues[0]; //Types.DATE
       LocalDate localDate = sqlDate.toLocalDate();
   
       Timestamp sqlTimestamp = (Timestamp) inputValues[1]; //Types.TIMESTAMP
       LocalDateTime localDateTime = sqlTimestamp.toLocalDateTime();
   
       Timestamp sqlTimestamptz = (Timestamp) inputValues[2];  //Types.TIMESTAMP_WITH_TIMEZONE
       OffsetDateTime offsetDateTime = sqlTimestamptz.toInstant().atOffset(ZoneOffset.UTC);
   
       Time sqlTime = (Time) inputValues[3]; //Types.TIME
       LocalTime localTime = sqlTime.toLocalTime();
   
       Duration d = (Duration) inputValues[4]; // JDBCTypeUtil.INTERVAL_DAY_SECOND
       Period p = (Period) inputValues[5]; ]; // JDBCTypeUtil.INTERVAL_YEAR_MONTH 
   }
   
Regarding datetime values, the procedure can return objects of the package java.sql or java.time.

In the listing below, the procedure returns two rows that are equivalent. The first row is generated with objects of the package java.sql and the second one, with objects of the package java.time.

.. code-block:: java
   
   public void doCall(Object[] inputValues) {

        // Adding a row with java.sql objects
        getProcedureResultSet().addRow(new Object[]{
            Date.valueOf("2017-10-11"), 
            Timestamp.valueOf("2015-03-08 01:59:59"), 
            new Timestamp(sdf.parse("2015-03-08 01:59:59 +01:00").getTime()), 
            Time.valueOf("21:15:45"), 
            Duration.ofHours(65).plusMinutes(23), 
            Period.ofMonths(25)});
   
        // Adding a row with java.time objects   
        getProcedureResultSet().addRow(new Object[]{
            LocalDate.parse("2017-10-11"),
            LocalDateTime.parse("2015-03-08T01:59:59"),
            OffsetDateTime.parse("2015-03-08T01:59:59+01:00"),
            LocalTime.parse("21:15:45"),
            Duration.ofHours(65).plusMinutes(23),
            Period.ofMonths(25)
   }

Compatibility with Stored Procedures of Previous Versions
---------------------------------------------------------

In previous versions, the constants of the class java.sql.Types are mapped to different data types of Denodo.

-  Types.DATE:

   -  Denodo 7.0: mapped to ``localdate``
   -  Previous versions: mapped to ``date`` (deprecated)
   
-  Types.TIMESTAMP:

   -  Denodo 7.0: mapped to ``timestamp``
   -  Previous versions: mapped to ``date`` (deprecated)
   
-  Types.TIME:

   -  Denodo 7.0: mapped to ``time``
   -  Previous versions: mapped to ``long``
 
If you developed a stored procedure that still relies on these objects, declare the procedure with the token ``USE_DENODO_6_0_TYPE_MAPPING``. For example:
   
.. code-block:: vql

   CREATE PROCEDURE testnewtypesprocedure2
       CLASSNAME='com.denodo.vdb.test.TestNewTypesProcedure'
       CLASSPATH=''
       USE_DENODO_6_0_TYPE_MAPPING = true;     

      
      
.. _StoredProcedureParameter: https://community.denodo.com/docs/html/browse/7.0/vdp/javadoc/index.html?com/denodo/vdb/engine/storedprocedure/StoredProcedureParameter.html