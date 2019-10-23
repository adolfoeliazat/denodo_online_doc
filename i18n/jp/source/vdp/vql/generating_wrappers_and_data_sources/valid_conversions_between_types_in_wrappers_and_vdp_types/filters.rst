=======
Filters
=======

After defining the path to a resource, it is possible to establish
filters that will be executed before processing the file.

The available filters are:

-  ``UNZIP``: decompress a ZIP compressed file.
-  ``GUNZIP``: decompress a GZIP compressed file.
-  ``DECRYPT``: decrypts a file that was encrypted with the
   “Password-Based-Encryption with MD5 and DES” algorithm. The section
   :ref:`Compressed or Encrypted Data Sources` of the Administration Guide
   explains how to generate files encrypted with this algorithm.
-  ``CUSTOM``: assigns a custom input filter to the data source. If the
   filter is located in a jar(s) that was loaded into the Server, add
   its name(s) to the ``JARS`` clause.
   If the custom filter has input parameters, add them after the name of
   the class of the filter.
   See more about this in the section :ref:`Custom Input Filters` of the
   Administration Guide.

The syntax of these filters is the following:



.. code-block:: bnf
   :caption: Syntax to set a filter in a DF, JSON or XML data source
   :name: Syntax to set a filter in a DF, JSON or XML data source

   <filter> ::=
       DECRYPT PASSWORD = <literal> [ ENCRYPTED ]
     | UNZIP
     | GUNZIP
     | CUSTOM [ JARS <jar name:literal> [, <jar name:literal> ]* ]
       CLASSNAME = <literal> [ <route filter parameter> ]*

   <route filter parameter> ::= 
     <parameter name:identifier> = <literal> [ ENCRYPTED ] [ HIDDEN ]


For example, the
command ``CREATE DATASOURCE ... ROUTE ... FILTER UNZIP`` will create
a data source that will retrieve the data file, decompress it and
finally process it.
