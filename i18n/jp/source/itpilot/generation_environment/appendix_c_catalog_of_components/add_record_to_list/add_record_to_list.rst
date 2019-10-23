==================
Add Record To List
==================

.. rubric:: Description

Adds a record to a list. This component must be used, when there is a
previous list (e.g. created using the CreateList component, section :ref:`Create List`) to which new records are to be added.

.. rubric:: Input Parameters

-  Record: record to be added to the list. If the list is not empty, the
   number and type of record fields must be consistent with those
   existing in the list, otherwise all the addrecordtolist components
   and the createlist component related to said list will mark their
   output as incorrect.
-  Target list: name of the list to which the new record is added.

.. rubric:: Output Values

This component returns the same list as received as input, modified to
include the additional record.
