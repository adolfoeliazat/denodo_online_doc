===========================
Developing Custom Functions
===========================

.. toctree::
   :hidden:

   creating_custom_functions_with_annotations.rst
   creating_custom_functions_using_name_conventions.rst
   compound_types.rst
   custom_function_return_type.rst
   getting_information_about_the_context_of_the_query.rst
   dealing_with_datetime_and_interval_types.rst

Custom functions allow users to extend the set of functions available in
Virtual DataPort. Custom functions are implemented as Java classes
included in a Jar file that is added to Virtual DataPort (see section
:ref:`Importing Extensions` of the Administration Guide). These custom
functions can be used in the same way as every other function like
``MAX``, ``MIN``, ``SUM``, etc.

Virtual DataPort allows creating condition and aggregation custom
functions. Each function must be in a different Java class, but it is
possible to group them together in a single Jar file.

In the Virtual DataPort installation (in the directory
:file:`{<DENODO_HOME>}/samples/vdp/customFunctions)`, there are examples of
custom functions. The ``README`` file of this directory explains how to
compile and use these examples.

We strongly recommend developing custom functions using Java annotations
(see section :ref:`Creating Custom Functions with Annotations`). Although it
is also possible to develop them following certain name conventions (see
section :ref:`Creating Custom Functions Using Name Conventions`), your
custom function will not have access to all the features provided by the
Denodo Platform.

These are the rules that every custom function must follow to work
properly:

-  Functions with the same name are not allowed. If a Jar contains one
   or more functions with the same name, the Server will not load
   anything from that Jar.
-  All custom functions stored in the same Jar are added or removed
   together by uploading/removing the Jar in the Server.
-  Each function can have many signatures. Each signature represents a
   different method in the Java class that defines the custom function.
-  Functions can have arity n but only the last parameter of the
   signature can be repeated n times.
-  Functions have to be stateless. That is, they should not store any
   data between executions. E.g., do not use global variables.
   If a custom function is implemented as stateful, it may not work
   properly in certain scenarios.

Custom functions signatures that return compound type values (register
or array) need an additional method to compute the structure of the
return type. This way Virtual DataPort knows in advance the output
schema of the query. This method is also needed if the output type
depends on the input values of the custom function.

When defining custom functions simple types are mapped directly from
Java objects to Virtual DataPort data objects. The following table shows
how the mapping works and which Java types can be used:

.. csv-table:: Equivalency between Java and Virtual DataPort data types
   :header: "Virtual DataPort Type", "Java Class"
   :name: Equivalency between Java and Virtual DataPort Data Types
   
   "blob","byte[]"
   "boolean","java.lang.Boolean"
   "date (deprecated)","java.util.Calendar"
   "decimal","java.math.BigDecimal"
   "double","java.lang.Double"
   "float","java.lang.Float"
   "int","java.lang.Integer"
   "intervaldaysecond","java.time.Duration"
   "intervalyearmonth","java.time.Period"
   "localdate","java.time.LocalDate"
   "long","java.lang.Long"
   "text","java.lang.String"
   "time","java.time.LocalTime"
   "timestamp","java.time.LocalDateTime"
   "timestamptz", "java.time.ZonedDateTime and java.util.Calendar"

The parameters of a custom functions cannot be primitives: ``int``,
``long``, ``double``, etc. are not valid. Instead, use ``java.lang.Integer``, ``java.lang.Long``, ``java.long.Double``, etc.

The last parameter of the function can be a “varargs” argument. For
example:

``function1(Integer parameter1, String... parameterN)``

In Virtual DataPort, this function will have a variable number of
arguments.

|

If a custom function relies on third-party libraries, do the following:

-  Copy the contents of the required jar files into the jar that contains
   the custom function. We have to copy the contents of the required
   jars, not the jars themselves.
   
-  Or copy the required jar files to the directory
   :file:`{<DENODO_HOME>}/extensions/thirdparty/lib`. In general, we advise against copying jar files to this directory unless you are sure that these libraries do not interact with other libraries of the Denodo Platform. For example, copying libraries of Apache Commons to this directory will probably cause some malfunction on the Virtual DataPort server.
