==============
Compound Types
==============

In custom functions, compound types and compound values are represented
with the following Java classes:


-  Registers:

   -  ``com.denodo.common.custom.elements.CustomRecordType`` represents the
      type of a register field (not a value of the register).

      Its method ``getProperties()`` returns a collection of name-type
      pairs. Each element of the collection holds the type of one of the
      fields of the register. The class of the ``Object`` returned by the
      method ``getType()`` of the interface ``CustomRecordType.Property``
      depends on the type of the field:

      i. If the type of the field is basic, the method returns a
         ``java.lang.Class``: ``Long.class``, ``Integer.class``,
         ``String.class``, etc.
      #. If the type of the field is a register, the method returns a
         ``CustomRecordType`` object.
      #. If the type of the field is an array, the method returns a
         ``CustomArrayType`` object.

   -  ``com.denodo.common.custom.elements.CustomRecordValue`` represents the
      value of register field.

      Its method ``getProperties()`` returns a collection of name-value
      pairs of the register. Each element of the collection holds the value
      of one of the fields of the register. The class of the ``Object``
      returned by the method ``getValue()`` of the interface
      ``CustomRecordValue.Property`` depends on the type of the field:

      i. If the type of the value is basic, the method returns a basic Java
         object: ``java.lang.String``, ``java.lang.Integer``,
         ``java.lang.String``, etc.
      #. If the type of the field is a register, the method returns a
         ``CustomRecordValue`` object.
      #. If the type of the field is an array, the method returns a
         ``CustomArrayValue`` object.

-  Arrays:

   -  ``com.denodo.common.custom.elements.CustomArrayType`` represents the
      data type of an array field (not a value of the array).
      It holds the name of the type and an instance of ``CustomRecordType``
      with the type of the elements of the array (an array is always an
      array of registers)
      
   -  ``com.denodo.common.custom.elements.CustomArrayValue`` represents the
      values of an array. It holds a list of ``CustomRecordValue`` objects.

   -  ``com.denodo.common.custom.elements.CustomGroupValue`` represents the
      list of values coming from a non-aggregation field in an aggregation
      function.

The class ``CustomElementsUtil`` provides methods to create array and
register types and values.
