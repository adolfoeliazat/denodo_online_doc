============================
Configuring a Custom Wrapper
============================

A CUSTOM wrapper can be configured with the ``getConfiguration`` method.
This method must return an instance of the
``CustomWrapperConfiguration`` class, which encapsulates the following
configuration parameters:

-  ``delegateProjections`` (true by default). Defines whether a CUSTOM
   wrapper can deal with projected fields when being queried.
-  ``delegateCompoundFieldProjections`` (``true`` by default). Defines
   whether a CUSTOM wrapper can deal with compound projected fields when
   being queried.
-  ``delegateOrConditions`` (``false`` by default). Defines whether a
   CUSTOM wrapper can deal with ``OR`` conditions, as in
   ``WHERE F1 = 1 OR F1 = 2`` in SQL.
-  ``delegateNotConditions`` (``false`` by default). Defines whether a
   CUSTOM wrapper can deal with ``NOT`` conditions, as in
   ``WHERE NOT (F1 = 1)`` in SQL.
-  ``delegateArrayLiterals`` (``false`` by default). Defines whether a
   CUSTOM wrapper can deal with conditions containing arrays (as in
   ``MY_INT_ARRAY = { ROW( 1 ), ROW( 2 ) }``).
-  ``delegateRegisterLiterals`` (``false`` by default). Defines whether
   a CUSTOM wrapper can deal with conditions containing registers (as in
   ``MY_REGISTER = ROW( 1, 'A' )``).
-  ``delegateLeftLiterals`` (false by default). Defines whether a CUSTOM
   wrapper can deal with conditions with literals in their left side (as
   in ``100 = FIELD``).
-  ``delegateRightFields`` (false by default). Defines whether a CUSTOM
   wrapper can deal with conditions with fields in their right side (as
   in ``FIELD1 = FIELD2``).
-  ``delegateRightLiterals`` (true by default). Defines whether a CUSTOM
   wrapper can deal with conditions with literals in their right side
   (as in ``FIELD1 = 100``).
-  ``delegateOrderBy`` (false by default). Defines whether a CUSTOM
   wrapper can deal with ORDER BY expressions.
-  ``allowedOperators`` (by default, an array containing the operator
   ``=``). Defines which operators are supported in the conditions
   passed to the CUSTOM wrapper. 
   
   The Javadoc of the method `setAllowedOperators(...) <../../../javadoc/com/denodo/vdb/engine/customwrapper/CustomWrapperConfiguration.html#setAllowedOperators-java.lang.String:A->`_ of the class "CustomWrapperConfiguration" lists all the possible operators. 

The values for all of these properties can be obtained and defined by
means of the appropriate getters and setters.