=================================
Creating Custom ITPilot Functions
=================================


.. toctree::
   :hidden:

   naming_conventions_and_annotations/naming_conventions_and_annotations.rst
   compound_types/compound_types.rst
   page_type/page_type.rst
   custom_function_return_type/custom_function_return_type.rst
   example/example.rst

Custom functions let users extend the set of functions available in
ITPilot.

Custom functions are Java classes included in a Jar file that are added
to ITPilot so they can be used in the same way as other functions such
as MAX, MIN, SUM, etc.



Denodo4E, an Eclipse plug-in which provides tools for creating,
debugging and deploying Denodo extensions, including custom ITPilot
functions, is included in the Denodo Platform. Please read the README in
:file:`{<DENODO_HOME>}/tools/denodo4e` for more information.



Each function must be in a different Java class, but it is possible to
group them in a single Jar.

We recommend developing custom functions using Java annotations,
although it is also possible to do it using name conventions.

Although custom functions can be created without dependencies on Denodo
libraries, the use of Java annotations is recommended. The annotations
and compound types and values required to create custom functions are
located in :file:`{<DENODO_HOME>}/lib/contrib/denodo-custom.jar`.



These are the rules that every custom function must follow to work
properly:

-  Functions with the same name are not allowed. If a jar contains one
   or more function with name conflicts, nothing in that jar will be
   loaded in the server.
-  All custom functions stored in the same jar are added or removed
   together by uploading/removing the jar in the server.
-  Each function can have many signatures. Each signature is defined by
   an execution method in the Java class defining the custom function.
-  Functions can have arity *n* but only the last parameter of the
   signature can be repeated *n* times.



A custom function is defined in a Java class containing all its
implementation; the name of the function will be extracted from that
Java class. A function can contain several signatures: different
combinations of arguments (different number, types or both). For each
signature of the function, this class must define a Java method
implementing the functionality of the function with those arguments, and
one additional method in case the signature returns a different type
depending on the parameters or the return type is compound (array or
register).



When defining custom functions simple types are mapped directly from
Java objects to ITPilot objects. The following table shows how the
mapping works and which Java types can be used:



.. table:: Equivalency between Java and ITPilot data types
   :name: Equivalency between Java and ITPilot data types

   +--------------------------------------+--------------------------------------+
   | Java                                 | ITPilot                              |
   +======================================+======================================+
   | java.lang.Integer                    | int                                  |
   +--------------------------------------+--------------------------------------+
   | java.lang.Long                       | long                                 |
   +--------------------------------------+--------------------------------------+
   | java.lang.Float                      | float                                |
   +--------------------------------------+--------------------------------------+
   | java.lang.Double                     | double                               |
   +--------------------------------------+--------------------------------------+
   | java.lang.Boolean                    | boolean                              |
   +--------------------------------------+--------------------------------------+
   | java.lang.String                     | text                                 |
   +--------------------------------------+--------------------------------------+
   | java.util.Date                       | date                                 |
   +--------------------------------------+--------------------------------------+
   | java.util.Calendar                   | date                                 |
   +--------------------------------------+--------------------------------------+
   | byte[]                               | binary                               |
   +--------------------------------------+--------------------------------------+

.. note:: The parameters of a custom functions cannot be basic types:
   int, long, double, etc.
