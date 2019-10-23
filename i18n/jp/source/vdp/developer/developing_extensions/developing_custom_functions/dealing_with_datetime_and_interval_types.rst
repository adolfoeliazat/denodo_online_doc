========================================
Dealing with Datetime and Interval Types
========================================

This section explains how to develop custom functions with input and/or output parameters of one of the datetime types.

The table below is the mapping between data types of Denodo and Java classes.

+------------------+-------------------------------------------+
| Type in Denodo   | Java Class of the Package java.time       |
+==================+===========================================+
| localdate        | java.time.LocalDate                       |
+------------------+-------------------------------------------+
| time             | java.time.LocalTime                       |
+------------------+-------------------------------------------+
| timestamp        | java.time.LocalDateTime                   |
+------------------+-------------------------------------------+
| timestamptz      | java.time.ZonedDateTime and               |
|                  | java.util.Calendar                        |
+------------------+-------------------------------------------+
|intervalyearmonth | java.time.Period                          |
+------------------+-------------------------------------------+
|intervaldaysecond | java.time.Duration                        |
+------------------+-------------------------------------------+

For example, to build a custom function with an input parameter of type intervalyearmonth, the class of the input parameter has to be a java.time.Period. 

To return a timestamptz, the function has to return a ZonedDateTime or a Calendar.

Below you can find the implementation of a function with two signatures:

.. code-block:: java
   
   @CustomElement(type = CustomElementType.VDPFUNCTION, name = "MY_CUSTOM_FUNCTION") 
   public class MyCustomFunction {
       
       @CustomExecutor
       /* 
       * Signature #1 has two input parameters: a ZonedDateTime and an Integer.
       * The execution engine considers that the types of the input parameters are 
       * timestamptz and int respectively
       *     
       * It returns a ZonedDateTime. The execution engines converts this into a timestamptz.
       */
       public ZonedDateTime execute(@CustomParam(name = "input") ZonedDateTime input,
               @CustomParam(name = "increment") Integer increment) {

           if (input == null || increment == null) {

               return null;

           } else {

               return input.plusHours(increment);
           }
       }
        
        @CustomExecutor
        /* 
        * Signature #2 has two input parameters: a Duration and a Long.
        * The execution engine considers that the types of the input parameters are 
        * intervaldaysecond and long respectively
        *     
        * It returns a Duration object. The execution engines converts this into a intervaldaysecond.
        */
       public Duration execute(@CustomParam(name = "input") Duration input, 
                @CustomParam(name = "increment") Long increment) {

            if (input == null || increment == null) {
                return null;

            } else {

                return input.plusHours(increment);
            }
       }
   }

To receive the locale of the query, add a parameter of the type java.util.Locale and annotate it with `@CustomLocale <https://community.denodo.com/docs/html/browse/7.0/vdp/javadoc/index.html?com/denodo/common/custom/annotations/CustomLocale.html>`_. The execution will inject the Local  receive the locale of the query.

For example,

.. code-block:: java

   @CustomExecutor
   public ZonedDateTime execute(@CustomParam(name = "input") ZonedDateTime input,
        @CustomParam(name = "increment") Integer increment, @CustomLocale Locale locale) {...

|

In previous versions, the class Calendar is mapped to the type date (deprecated). Starting with Denodo 7.0, the class Calendar is mapped to timestamptz. If you still want to map Calendar to date, execute the following command from the VQL Shell and restart the server.

.. code-block:: vql

   SET 'com.denodo.vdb.compatibility.datetime.custom.mapCalendarToDateType' = 'true';

.. csantos@2018/03/22: I comment everything below here because I do not understand why users would want to need to use OffsetDateTime
    It is important to note that using ZonedDateTime or OffsetDateTime when defining the parameters of a Custom Function is not equivalent. Although both types can represent a instant of time in a specific time zone, ZonedDateTime contains rules to calculate time zone transitions and this may affect the result. Let's see with an example: 

    The following piece of code shows two possible implementations of a "customfirstdayofmonth" funtions, that returns the received datetime but rolled down to the first day of the month. The first uses ZonedDateTime and the second OffsetDateTime.


    .. code-block:: java
       :name: Implementation example
       
        //Depending on the data type, result will be different, as there is a DST involved in the ZonedDateTime
        @CustomExecutor
        public ZonedDateTime execute(@CustomParam(name = "input") ZonedDateTime input) {
           if (input == null) {
               return null;
           } else {
               return input.withDayOfMonth(1);
           }
        }
       
        @CustomExecutor
        public OffsetDateTime execute(@CustomParam(name = "input") OffsetDateTime input) {
          if (input == null) {
               return null;
           } else {
               return input.withDayOfMonth(1);
           }
        }

    When executing a query like this: 

    .. code-block:: sql

      SELECT customfirstdayofmonth(TIMESTAMP '2017-03-30 00:00:00 +00:00' CONTEXT('i18n'='es_euro')
        
    The first version will receive a ZonedDateTime with the value 2017-03-30 02:00:00 +02:00 [Europe/Madrid]. After moving to the first day of month, the rules of the zone determine a new offset +01:00 so the result will be 2017-03-01 02:00:00 +01:00 [Europe/Madrid].
        
    But the second version will receive an OffsetDateTime with the same instant of above but without info about time zone transitions, 2017-03-30 02:00:00 +02:00. After moving to the first day, the offset does not change, returning 2017-03-01 02:00:00 +02:00, which is not the same instant that returns the previous version of the function.

        
    VDP internal custom functions and custom policies use always ZonedDateTime
  
