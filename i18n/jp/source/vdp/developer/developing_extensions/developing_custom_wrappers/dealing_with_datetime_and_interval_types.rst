========================================
Dealing with Datetime and Interval Types
========================================

This section explains how to develop custom wrappers that return datetime values.

When you develop a custom wrapper, the main class of the wrapper has to override the method ``getSchemaParameters(Map<String, String> inputValues)``, which defines the output schema of the custom wrapper. This method has to return an array of `CustomWrapperSchemaParameter <https://community.denodo.com/docs/html/browse/7.0/vdp/javadoc/index.html?com/denodo/vdb/engine/customwrapper/CustomWrapperSchemaParameter.html>`_ objects. The second parameter of the constructors of the class ``CustomWrapperSchemaParameter`` is the parameter ``type``. The value of this parameter is a constant of the class `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_. 

The table below displays the mapping between:

-  A Denodo data type and the constant of the class ``java.sql.Types`` that has to be passed to the constructor of ``CustomWrapperSchemaParameter``.
-  A Denodo data type and the class of the Java object that the custom wrapper has to return.

.. csv-table:: 
   :header: "Denodo Data Type", "Constant of the Class java.sql.Types", "Java Class"

   "localdate", "Types.DATE", "java.time.LocalDate"
   "time", "Types.TIME", "java.time.LocalTime"
   "timestamp", "Types.TIMESTAMP", "java.time.LocalDateTime"
   "timestamptz", "Types.TIMESTAMP_WITH_TIMEZONE", "java.time.OffsetDateTime"
   "intervaldaysecond", "JDBCTypeUtil.INTERVAL_DAY_SECOND", "java.time.Duration"
   "intervalyearmonth", "JDBCTypeUtil.INTERVAL_YEAR_MONTH", "java.time.Period"

JDBCTypeUtil = com.denodo.vdb.vdbinterface.common.clientResult.vo.descriptions.type.util.JDBCTypeUtil

.. code-block:: java
   :caption: Definition of the parameters of a custom wrapper with datetime fields
   
   public CustomWrapperSchemaParameter[] getSchemaParameters(
           Map<String, String> inputValues) { 

       return new CustomWrapperSchemaParameter[] {
           
             new CustomWrapperSchemaParameter("date_field", Types.DATE)
           , new CustomWrapperSchemaParameter("timestamp_field", Types.TIMESTAMP)
           , new CustomWrapperSchemaParameter("timestamptz_field", Types.TIMESTAMP_WITH_TIMEZONE)
           , new CustomWrapperSchemaParameter("time_field", Types.TIME)
           , new CustomWrapperSchemaParameter("intervalyear_month", 
                 JDBCTypeUtil.INTERVAL_YEAR_MONTH)
           , new CustomWrapperSchemaParameter("intervalday_second", 
                 JDBCTypeUtil.INTERVAL_DAY_SECOND) };
   }


In the code below, note that the Java objects returned by the custom wrapper are the appropriate ones according to the table above. For example, in the method ``getSchemaParameters()``, the field "date_field" is created with the constant ``Types.DATE``. In the table above, this type of parameters corresponds with the type "localdate" and the objects of the result have to be ``java.time.LocalDate``.

.. code-block:: java
   :caption: run() method of a custom wrapper that returns datetime values.
   
   void run(CustomWrapperConditionHolder condition,
         List<CustomWrapperFieldExpression> projectedFields,
         CustomWrapperResult result,
         Map<String,String> inputValues)
         throws CustomWrapperException {
       
       // ...
       // ...

       result.addRow(new Object[]{      
               LocalDate.parse("2017-10-11"),
               LocalDateTime.parse("2015-03-08T01:59:59"),
               OffsetDateTime.parse("2015-03-08T01:59:59+01:00"),
               LocalTime.parse("21:15:45"),
               Duration.ofHours(65).plusMinutes(23),
               Period.ofMonths(25)},
               projectedFields);
   
       result.addRow(new Object[]{
               sdf.parse("2017-10-11"),
               sdf.parse("2015-03-08 01:59:59"),
               sdf.parse("2015-03-08 01:59:59 +01:00"),
               sdf.parse("21:15:45"),
               Duration.ofHours(65).plusMinutes(23),
               Period.ofMonths(25)},
               projectedFields);
   }
   
To keep backward compatibility, custom wrappers can still return java.util.Date objects for the types "localdate", "timestamp", "timestamptz" and "time". However, returning this type of objects may not be supported in future major versions of Denodo.
