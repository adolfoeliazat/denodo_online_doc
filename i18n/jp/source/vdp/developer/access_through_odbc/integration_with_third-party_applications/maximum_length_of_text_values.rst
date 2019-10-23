=============================
Maximum Length of Text Values
=============================

When an application executes a query through the ODBC interface of
Virtual DataPort, this interface provides metadata about every field of
the result set of the query. For fields of type ``text``, it reports
among other things, the maximum length of the values of this field.

When a ``text`` field has its “Type size” defined in its “Source type
properties”, the ODBC interface reports this value. When the type size
is not defined, the ODBC interface reports that the maximum size of the
values of this field is unknown. In this case, as we configured the DSN
with the option “Unknown size” = “Maximum” (“Page 1” dialog of the DSN
configuration), the DSN will report that the maximum length of the field
is the value specified in the “Max Varchar” property of the DSN.

If the length of a ``text`` value, whose field does not have its “Type
size” defined, is longer than the “Max Varchar”, the application that
executes the query may do one of the following things:

-  Leave the value as is.
-  Truncate the value to the “Max Varchar” size.
-  Set the value to NULL.

This behavior changes from application to application.

See how to set the “Source type properties” of a Virtual DataPort view
in the section :ref:`Viewing the Schema of a Base View` of the Administration
Guide.
