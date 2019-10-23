===========================
Input and Output Parameters
===========================

The following types of input and output parameters can be used by the
components:

-  *Pages*: Some components such as the navigation sequence (Sequence)
   component return a page as result. A Page element can be used by
   other components to extract data from it or to continue a navigation
   sequence from that point.
-  *Records*: Other components (e.g. the Record Constructor) return a
   structured group of information (in this example, this may be the
   structured representation of an information item).
-  *Lists of records*: ITPilot allows for some components (e.g. the
   Extractor component) to return lists of registers at their output or
   to accept them at their input.
-  *Values*: Other components can return simple values at their output
   such as the Expression component. Simple values can be of the
   following types: string, integer, date, double, long, float, url,
   boolean.
-  *Browsers*: Special simple values used by the browser persistence
   management components (see section :ref:`Create Persistent Browser`).

The input parameters are selected by mean of drop down lists which
contain all the output variables whose type is compatible with the type
of the input parameter (note that an input parameter can accept values
of a single type or several types). It is also possible to type text in
these drop down lists, to search the variables starting with the typed
text. This is especially useful in processes containing a lot of
components (and therefore, a lot of output variables that can be used as
input parameters in other components).



