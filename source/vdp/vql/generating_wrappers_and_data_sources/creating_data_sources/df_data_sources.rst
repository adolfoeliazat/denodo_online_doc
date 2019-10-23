===============
DF Data Sources
===============

DF data sources can retrieve data from delimited text files. The most
common delimited files are Comma-Separated-Values (CSV), but you can
also create data sources that retrieve data with other formats.

To create a DF data source, use the statement CREATE DATASOURCE DF.

.. code-block:: bnf
   :caption: Syntax of the CREATE DATASOURCE DF statement
   :name: Syntax of the CREATE DATASOURCE DF statement

   CREATE [ OR REPLACE ] DATASOURCE DF <name:identifier>
       [ FOLDER = <literal> ]
       [ IGNOREMATCHINGERRORS = { TRUE | FALSE } ]
       ROUTE <route> [ CHARSET = <literal> ] [ <route filters> ]
       {   COLUMNDELIMITER = <literal>
         | TUPLEPATTERN = <literal> [ HEADERPATTERN = <literal> ]
       }
       [ ENDOFLINEDELIMITER = <literal> ]
       [ BEGINDELIMITER = { <literal> | VAR <variable name:identifier> }
           [ ISDATA ]
       ]
       [ ENDDELIMITER = <literal> [ ISDATA ] ]
       [ HEADER = <boolean> ]
       [ MULTI_CHARACTER_DELIMITER = <boolean> ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]

   <route_filters> ::=
     FILTER ( <filter> [, <filter> ]* )

..

   <route> ::= (see :ref:`Syntax to set the path in a DF, JSON or XML data source`)

   <filter> ::= (see :ref:`Syntax to set a filter in a DF, JSON or XML data source`)


To modify a DF data source, use the statement ALTER DATASOURCE DF.

.. code-block:: bnf
   :caption: Syntax of the ALTER DATASOURCE DF statement
   :name: Syntax of the ALTER DATASOURCE DF statement

   ALTER DATASOURCE DF <name:identifier>
       [ IGNOREMATCHINGERRORS = { TRUE | FALSE } ]
       ROUTE <route> [ CHARSET = <literal> ] [ <route_filters> ]
       {   COLUMNDELIMITER = <literal>
         | TUPLEPATTERN = <literal> [ HEADERPATTERN = <literal> ]
       }
       [ ENDOFLINEDELIMITER = <literal> ]
       [ BEGINDELIMITER = { <literal> | VAR <variable name:identifier> }
           [ ISDATA ]
       ]
       [ ENDDELIMITER = <literal> [ISDATA] ]
       [ HEADER = <boolean> ]
       [ MULTI_CHARACTER_DELIMITER = <boolean> ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]

..

   <route> ::= (see :ref:`Syntax to set the path in a DF, JSON or XML data source`)

   <route_filters> ::= (see :ref:`Syntax of the CREATE DATASOURCE DF statement`)

|

Explanation of some of the parameters of these statements:  

-  ``OR REPLACE``: If present and a data source with the same name exists,
   the current definition is substituted with the new one.
   
-  ``IGNOREMATCHINGERRORS``: If ``TRUE``, when a query involves this
   data source, the Server will ignore the lines of this data file that
   do not have the expected structure. I.e. rows that do not have the
   expected number of columns or, if you are providing a tuple pattern,
   rows that do not match the pattern. If ``FALSE``, the Server will
   return an error if there is a row that does not have the expected
   structure.
   
   If the value is ``TRUE``, you can check if the Server has ignored any
   row in a query. To do this, execute the query from the Administration
   Tool. Then, click “View execution trace” and click the “Route” node.
   You will see the attribute “Number of invalid tuples”.
   
   If you do not provide this parameter, the Server will assume
   ``TRUE``.
   
-  ``ROUTE``: path to the delimited file that contains the data. See
   more information about paths to data files in the section :ref:`Specifying
   Paths in Virtual DataPort`.
   
   Note that the parameter ``FILENAMEPATTERN`` is only allowed in the
   path to delimited file data sources and not for XML or JSON ones.
   
   For LOCAL and FTP routes, if ``uri`` points to a directory, when you
   query the base views created over this data source, the Server will
   retrieve the data from all the files in the directory and not just
   one file. The value of ``FILENAMEPATTERN`` is a regular expression
   and if present, the Server will only obtain data from the files which
   name matches this expression.
   
   When retrieving data from several files, they all must have the same
   schema.
   
-  ``FILTER``: List of filters that will be applied to the data file
   before processing it. The available filters are ``UNZIP``,
   ``GUNZIP``, ``DECRYPT`` and ``CUSTOM``. See more about this in
   section :doc:`/vdp/vql/generating_wrappers_and_data_sources/valid_conversions_between_types_in_wrappers_and_vdp_types/filters`.
-  ``CHARSET``: It specifies the charset encoding used by the file. Any
   charset encoding supported by Java can be used.

-  ``COLUMNDELIMITER``: Character that separates the values of a row. It is only used if the ``TUPLEPATTERN`` is not indicated.

   To indicate a tab, enter ``\t``. This represents a single character, the tab.
   
   If you enter more than one character, all these characters will be considered a delimiter. For example, if you enter ``,|``, each value can be separated by the comma (,) or the vertical bar (|).   

-  ``TUPLEPATTERN``: Regular expression that specifies the format of the
   tuples that will be extracted from the delimited file. This regular
   expression has to match the whole line that wants to capture, not
   only part of it.
   
   The format used is that of regular expressions in 
   `Java language <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.
   
   The fields of the views will be the capturing groups of the regular
   expression.
   
   The section :ref:`Delimited File Sources` of the Virtual DataPort
   Administration Guide contains examples of tuple patterns.
   
   .. note:: The tuple pattern can contain interpolation variables.
   
-  ``ENDOFLINEDELIMITER``: Character string used as data tuple separator
   in the delimited file (the carriage return \\n will be used by
   default).
-  ``BEGINDELIMITER``: A Java regular expression that identifies where
   the Server must begin searching for tuples (or searching for the
   header if the parameter ``HEADER`` is ``TRUE``). If the parameter is
   not present, the parsing of the file will start at the beginning of
   the file.
   
   This parameter can be a literal or a name of an interpolation
   variable (see section :ref:`Execution Context of a Query and Interpolation
   Strings`). If you want to pass an interpolation variable, you have
   to add the parameter ``VAR`` followed by the name of the variable.
   
   If ``ISDATA`` is present, the text matching this regular expression
   is considered part of the data.
   
-  ``ENDDELIMITER``: A Java regular expression identifying the position
   in the file where the system must stop searching for tuples. If no
   value is specified, the search will continue until the end of the
   file. If the ``ISDATA`` modifier is added then the text matching with
   the regular expression will be considered as part of the search
   space. This may include interpolation variables to parameterize the
   access path depending on the conditions of the query executed on the
   data source (see section :ref:`Execution Context of a Query and
   Interpolation Strings`).
   
-  ``HEADER``. If ``true``, it is assumed that the first tuple extracted
   from the file data area contains the names of the fields. These names
   will be used to create the attributes of the base relation for
   Virtual DataPort.

-  ``MULTI_CHARACTER_DELIMITER``. If ``true``, the delimiter can consist of
   multiple characters. For example, ``|~`` could be used as the character
   delimiter.

-  ``HEADERPATTERN``. This indicates a regular expression-type pattern
   to be used to extract the name of the fields forming the header. This
   must only be specified if the pattern to be used to extract the
   header is different to that used to extract the tuples. The format of
   the regular expressions is the same as that used for the Tuple
   Pattern. This field can only be used when the Header check box is
   marked.

-  ``TRANSFER_RATE_FACTOR``: relative measure of the speed of the network connection between the Denodo server and the data source. Use the default value (e.g. 1 for JDBC databases located on premises) if the data source is accessible through a conventional 100 Mbps LAN. Use higher values for faster networks and lower values for data sources accessible through a WAN.

|

See the section :ref:`Optimizing DF Data Sources` of the Administration
Guide to get details on how to improve the performance of the DF data
sources.