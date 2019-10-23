================
CSV Data Sources
================

To use a CSV file as a data source to assign values to variables in an
ITP, VDP, or JDBC job created using a parameterized query (see section
:ref:`VDP Extraction Section`, a CSV data source needs to be defined that
references to that file.

When creating a CSV data source, the following parameters must be
specified:

-  **File**. The path to the file. Depending on whether the check box
   **Upload to server** is checked or not (checked by default), the file
   will be uploaded to the server or not. In this latter case, the user
   must specify a path that is local to the server (absolute or relative
   to $DENODO\_HOME) where the file is stored. It is important to note
   that the CSV files referenced by CSV data sources created with the
   “Upload to server” check box unchecked will not be included in the ZIP
   file generated when exporting their parent project.
-  **Separator**. The column separator to be used to obtain the file
   tuples. The tuples separator is assumed to be the carriage return.
-  **Header** (optional). If this check box is checked, the first row of
   the file will be used to give a name to the fields of each tuple
   obtained from it.

.. note:: The maximum size for a CSV file uploaded to the server is 100
   MB.

 

