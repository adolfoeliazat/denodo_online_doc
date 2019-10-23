================
Wrapper Metadata
================

In the case of most wrappers it is possible to specify metadata of the
output schema (``OUTPUTSCHEMA``) they provide, i.e. the fields that will
represent the data extracted from the source. These fields can be of
three types:

-  ``SIMPLE``: fields belonging to basic data types such as text
   strings, integers, etc. Optionally, you can indicate if they can
   appear in query conditions of the wrapper. Query fields can be
   mandatory (every query must include a condition for such field) or
   optional. When specifying a simple field, its Java data type is also
   specified. For that, the conversion tables specified in the section :ref:`Native-type Conversions of a Wrapper to Java Types` must be taken
   into account.
-  ``REGISTER``: formed by one or various fields, both simple and
   compound.
-  ``ARRAY``: lists composed by register-type fields.

Furthermore, a series of restrictions can be indicated for each output
schema field:

-  Whether the field can include null values (``NULL``) or cannot
   (``NOT NULL``). The default value is ``NULL``.
-  Whether the results can be ordered by the (``SORTABLE``) field or not
   (``NOT SORTABLE``). It is also possible to specify that the results
   can be sorted by the field but only in ascending (``SORTABLE ASC``)
   or descending (``SORTABLE DESC``) order. The ``SORTABLE`` value is
   assumed by default.
-  Whether the field can be updated in an ``UPDATE`` statement
   (``UPDATEABLE``) or cannot (``NOT UPDATEABLE``). The
   ``UPDATEABLE`` value is assumed by default.

