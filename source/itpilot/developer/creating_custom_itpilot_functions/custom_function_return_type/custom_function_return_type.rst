===========================
Custom Function Return Type
===========================

As explained before, custom functions which return type depends on input
values or functions returning compound types can implement an additional
method in order to compute the return type without executing the
function. This is entirely *optional*, but it provides better
performance when the execution of the function is slower or more memory
intensive than the return type calculation.



This additional method must follow a few rules:

1. When the ``execute`` method returns a non-constant compound type (a
   record whose fields -number of fields, and their names and/or types-
   depend on the input parameters) or a ``java.lang.Object`` then the
   additional method must be implemented. In other situations it is
   optional (the return type is obtained from the method directly).


#. The execution method must have the same number of parameters as the
   additional method.

#. Each parameter of the additional method must have the same or equivalent
   type, as its respective parameter in the ``execute`` method:

   If the execute method returns a basic Java type, the additional method has to return the same basic Java class.
   
   I.e. If the ``execute`` method returns a ``String`` object, the additional 
   method has to return ``java.lang.String.class``.
   
   If the ``execute`` method returns a ``CustomRecordValue`` object, the additional 
   method has to return a ``CustomRecordType`` object.
   
   If the execute method returns a ``CustomArrayValue`` object, the additional 
   method has to return a ``CustomArrayType`` object. 
   
   See table :ref:`Equivalency between Java and ITPilot data types` at the 
   beginning of section :ref:`Creating Custom ITPilot Functions` to know the type that these return parameters will have in ITPilot.
   