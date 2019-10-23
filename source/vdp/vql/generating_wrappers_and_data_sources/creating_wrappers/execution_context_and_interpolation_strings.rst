===========================================
Execution Context and Interpolation Strings
===========================================

As already mentioned in previous sections, the purpose of wrappers is
executing queries and/or updates on data sources.

When Virtual DataPort requests a wrapper to execute a query, it uses two
different ways to provide the data on the query conditions that the
wrapper should execute on the source:

-  As a structured list of query conditions. This is the manner used by
   most wrapper types.
-  As a series of *interpolation variables* included in a run context.
   This form of access is used by JDBC wrappers that use a pattern SQL
   query. The section :ref:`Execution Context of a Query and Interpolation
   Strings` contains more details about interpolation strings.

