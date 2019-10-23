==========
Local Path
==========

Use this type of path to obtain the data from a file or a set of files
located in the same directory. This path can be:

-  On the *local file system of the Virtual DataPort Server* (not to the
   local file system where the Administration Tool is running)
-  On a Windows shared drive accessible from the host where Virtual
   DataPort is running. Note that the user account that launches the
   Server has to have privileges to read this file.

To enter the path to a file, we recommend you to click the **Browse**
button, enter the path in the **File name** box and click **Ok**. The
reason for this is that in Virtual DataPort some characters have a
special meaning and have to be escaped. By doing it this way, these
characters are automatically escaped.

Instead of specifying the path to the file, you can use interpolation
variables (see section :ref:`Paths and Other Values with Interpolation
Variables`) if you do not know the path to the file at the time of
creating the base view and you want to provide it at runtime.

For example, you can enter the path ``/tmp/datafiles/@REPORT_ID.txt``.
The base views created over this data source will have an extra field
called ``report_id`` and at runtime you will have to provide the value
of this field in the ``WHERE`` clause of the query.

Paths Pointing to a Directory
=================================================================================

The **File path** can point to a file or to a directory and it can be in
the local file system or in a Windows shared drive.

When you create a base view over a data source that points to a
directory, Virtual DataPort infers the schema of the new view from the
first file in the directory and it assumes that all the other files have
the same schema.

Only for delimited-file data sources: if the path points to a directory
and you enter a value in **File name pattern**, the data source will
only process the files whose name matches the regular expression entered
in this box. For example, if you only want to process the files with the
extension ``log``, enter ``(.*)\\.log``.

.. note:: For XML data sources, if a *Validation file* has been
   provided, all files in the directory have to match that **Schema** or
   **DTD**.

