==================
Record Constructor
==================

.. rubric:: Description

This component allows the user to create a record using other values
generated in the wrapper (simple values, pages, records or lists) as
well as creating new fields derived from the inputs of the component.

.. rubric:: Input Parameters

A Record Constructor accepts zero or more values, zero or more records,
zero or more pages and zero or more lists of records as input, and uses
them as variables to build the output record. An input value can be used
as a field of the output record, or it can be also used to create new
derived fields by the use of functions; the available functions are the
same than in the Condition component, defined in Appendix A.



.. note:: If any “Hidden” field on the inputs is used as a field of the
   output record, the output field will inherit the “Hidden” property. If
   any “Hidden” field on the inputs is used to create a new derived field
   by the use of an expression, the wizard will ask whether the derived
   field should be hidden or not.

.. rubric:: Output Values

The component returns one record.

.. rubric:: Details of the Component

See section :ref:`Processing the individual records: use of Record
Constructor` for an in-depth explanation of the component.



