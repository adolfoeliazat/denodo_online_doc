==============
Compound Types
==============

Compound types and values in the custom functions are defined by the
following Java classes:

-  ``com.denodo.common.custom.elements.CustomRecordType``. Class
   representing a register data type. It stores the type name and a set
   of name-type pairs where the name is a string and the type is either
   a *java.lang.Class* of some of the Java classes used for simple
   types, or a Denodo compound type (*CustomRecordType* or
   *CustomArrayType*).
-  ``com.denodo.common.custom.elements.CustomRecordValue``. Class
   representing a register data value. It stores a set of name-value
   pairs where the name is a string and the value is either an instance
   of a simple type (java.lang.String, java.lang.Integer, etc.), or
   another compound value (*CustomRecordValue* or *CustomArrayValue*).
-  ``com.denodo.common.custom.elements.CustomArrayType``. Class
   representing an array data type. It stores the type name and an
   instance of *CustomRecordType*, that defines the type of the elements
   of the array.
-  ``com.denodo.common.custom.elements.CustomArrayValue``. Class
   representing an array value. It stores a list of *CustomRecordValue*
   instances.
-  ``com.denodo.common.custom.elements.CustomElementsUtil``. Helper
   class with methods to instantiate compound types and values, if
   needed.
