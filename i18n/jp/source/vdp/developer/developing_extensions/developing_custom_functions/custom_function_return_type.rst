===========================
Custom Function Return Type
===========================

The custom functions whose return type is an ``array`` or a
``register``, or whose return type depends on the input values, have to
implement an additional method that returns the type of the function
based on the parameters of the function.

If the function has several signatures that meet one of these
conditions, there must be an additional method for each signature.

In the custom functions developed with Java annotations, this additional
method has to be annotated with ``CustomExecutorReturnType``. If it is
developed with name conventions, the name of the method has to be
``executeReturnType``.

This additional method has to meet these rules:

#. It must have the same number of parameters as the “execute” method.

#. Each parameter of the additional method must have the same type or an
   equivalent one, as its respective parameter in the ``execute`` method:

   a. If ``execute`` returns a basic Java type, the additional method has
      to return the same basic Java class. For example, if ``execute``
      returns a ``String`` object, the additional method has to return
      ``java.lang.String.class``.
   #. If ``execute`` returns a ``CustomRecordValue`` object, the additional
      method has to return a ``CustomRecordType`` object.
   #. If ``execute`` returns a ``CustomArrayValue`` object, the additional
      method has to return a ``CustomArrayType`` object.
      See the table :ref:`Equivalency between Java and Virtual DataPort Data Types` to
      know the type that these return parameters must have in Virtual
      DataPort.
   #. If a parameter of ``execute`` is a ``CustomRecordValue``, the type of
      the parameter in the additional method has to be
      ``CustomRecordType``.
   #. If a parameter of ``execute`` is a ``CustomArrayValue``, the type of
      the parameter in the additional method has to be ``CustomArrayType``.

#. If the returned type is a compound data type, the type will be created
in Virtual DataPort, unless it already exists. If the returned type does
not have name, the type will be created with a random name.

At runtime, every time this function is invoked, Virtual DataPort will
execute this additional method to know the return type of the function.

The following two sections contain an example of how to implement this
additional method in a function that uses annotations and in a function
that uses name conventions.

Function Without Annotations with Return Type Depending on the Input
====================================================================

Implementation of a function ``SPLIT`` which splits strings around
matches of a given regular expression and returns the array of those
substrings:

 
.. code-block:: java
  :caption: Example of function without annotations with return type depending on the input

   public class SplitVdpFunction {
       private static final String STRING_FIELD = "string";
       public CustomArrayValue execute(String regex, String value) {
           if (value == null || regex == null) {
               return null;
           }
           String[] result = value.split(regex);
           LinkedHashMap<String, Object> results = 
                   new LinkedHashMap<String, Object>(1);
           List<CustomRecordValue> arrayValues = 
                   new ArrayList<CustomRecordValue>(result.length);
           for (String string : result) {
               results.put(STRING_FIELD, string);
               CustomRecordValue recordValue =
                       CustomElementsUtil.createCustomRecordValue(results);
               arrayValues.add(recordValue);
           }
           return CustomElementsUtil.createCustomArrayValue(arrayValues);
       }
   
       public CustomArrayType executeReturnType(String regex, String value){
           LinkedHashMap<String, Object> props = 
                   new LinkedHashMap<String, Object>();
           props.put(STRING_FIELD, String.class);
           CustomRecordType record = 
                   CustomElementsUtil.createCustomRecordType(props);
           CustomArrayType array = 
                   CustomElementsUtil.createCustomArrayType(record);
           return array;
       }
   }
   
Aggregation Function Using Annotations
=============================================

The following function returns the first value of a non group-by field
for each group:

.. code-block:: java
  :caption: Example of aggregation function using annotations

   @CustomElement(
         type=CustomElementType.VDPAGGREGATEFUNCTION, name="FIRST_RECORD")
   public class FirstRecordFunction {
   
       @CustomExecutor
       public CustomRecordValue execute(   
               @CustomGroup(groupType=CustomRecordValue.class, name="records")       
                   CustomGroupValue<CustomRecordValue> records) {
   
           if(records == null) {
               return null;
           }
           if(records.size() == 0) {
               return null;
           }
   
           return records.getValue(0);
       }
   
       @CustomExecutorReturnType
       public CustomRecordType executeReturnType(
               CustomRecordType recordType) {
   
           return recordType;
       }
   }


.. code-block:: java
  :caption: Example of aggregation function using annotations

   @CustomElement(type=CustomElementType.VDPAGGREGATEFUNCTION, name="FUNCTION_F1")
   public class FirstRecordFunction {
   
       @CustomExecutor
       public Long execute(   
               @CustomGroup(groupType=Long.class, name="values")       
                   CustomGroupValue<Long> records) {
   
           ...
           ...
           return ...
       }
   
       @CustomExecutorReturnType
       public Class executeReturnType(Long values) {
   
           return Long.class;
       }
   }
