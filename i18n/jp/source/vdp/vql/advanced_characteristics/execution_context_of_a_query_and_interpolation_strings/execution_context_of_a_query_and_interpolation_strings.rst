======================================================
Execution Context of a Query and Interpolation Strings
======================================================

This section describes the concept of *interpolation strings*. They are
used in Virtual DataPort to parameterize certain expressions used by the
*wrapper* or the *data source* of a base view depending on the queries
made to this view (see section :ref:`Generating Wrappers and Data Sources`).

An *interpolation string* is an expression that uses execution context
variables, which generates a string as a result. A variable in an
interpolation string is specified with the prefix ``@`` followed
by the name of the variable, provided that this name is a string of
alphanumeric characters (letters and the characters ``#`` and
``_``). Variables with a name that includes any other character
can be specified by including the name between the symbols ``@{``
and ``}``.

.. note:: When any of the symbols ``@``, ``\``, ``^``, ``{``, ``}``
   appear in the constant parts of the interpolation string, they must be
   escaped by the character ``\`` (i.e. ``\@``, ``\\``,
   ``\^``, ``\{``, ``\}``). Note that this implies that,
   on specifying local file-type paths in Windows Operating Systems, the
   character ``\`` must be escaped as ``\\``.

**Example**: Suppose you have a Web server that allows accessing to
certain reports from the departments of a particular company encoded
into XML. The path to access the report from each department is the
same, except for the name of the file that matches the name of the
department. For example:

   | \http://examplesite.com/exampleroute/reports/DPT1.xml
   | \http://examplesite.com/exampleroute/reports/DPT2.xml ...

Now suppose that you want to build a base view that retrieves these
reports. To do so you must create an XML data source (see section :ref:`XML Data Sources`), an XML wrapper (see section :ref:`XML Wrappers`) and a
base view that uses this wrapper. This base view (called
``DPT_REPORTS``) contains a tuple for each department. Each tuple will
have two attributes: ``DPT_NAME`` (``text`` type) and ``REPORT``, which
will contain the report data.

When creating the *data source* for this base view, the problem arises
that the data file to be accessed depends on the department referred to
by the query. To solve this problem, you can add an interpolation
variable to the ``ROUTE`` parameter of the ``CREATE DATASOURCE XML``
statement that creates the data source. E.g. in this example, we add the
variable ``DPT_NAME`` to the URL of the Service, instead of putting the
actual name of the file:

   \http://examplesite.com/exampleroute/reports/\@{DPT\_NAME}.xml

The wrappers and base views created over this data source will have a
field ``DPT_NAME``. Hence, queries such as the following can be
executed:

.. code-block:: sql

   SELECT REPORT 
   FROM DPT_REPORTS 
   WHERE DPT_NAME = 'DPT3'

And the Server will retrieve the XML document from this path 

   \http://examplesite.com/exampleroute/reports/DPT3.xml

When an interpolation variable has a list of elements as a value (this
happens in the cases of operators allowing for a list of values as
operands), the value associated with the variable will be the linking of
the single elements separated by the character ``+``. This can be
used in the parameterization of certain aspects of the WWW wrappers (see
section :ref:`WWW Wrappers`).

There are situations where you may need to replace an interpolation
variable with the value provided in the query *only* if this value is
not ``NULL`` and in that case, replace it with other text. In this
scenario, use the *interpolation function* ``ExecuteIfIsNotNull``.

The section :ref:`Using the WHEREEXPRESSION Variable` of the
Administration Guide explains how to use this function. Although the
examples provided use the interpolation variable ``@WHEREEXPRESSION``,
it can be used with any other variable.

