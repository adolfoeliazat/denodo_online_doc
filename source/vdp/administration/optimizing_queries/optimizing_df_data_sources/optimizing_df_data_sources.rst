==========================
Optimizing DF Data Sources
==========================

It is possible to decrease the processing time of a delimited file by
using the “Use tuple pattern” option of the DF data source instead of
the “Use column delimiter” option (see section :ref:`Delimited File
Sources`)

The “Tuple pattern” is a regular expression that matches the rows that
you need to obtain from the file. The advantage over using a “column
delimiter” is that the lines that do not match the regular expression
are immediately discarded and the Server does not process them. On the
other hand, when using the “Use tuple pattern” option, every line of the
file is parsed.

The section :ref:`Delimited File Sources` contains several examples of tuple
patterns.

