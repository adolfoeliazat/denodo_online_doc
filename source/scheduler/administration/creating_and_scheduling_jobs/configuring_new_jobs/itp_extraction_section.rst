======================
ITP Extraction Section
======================

The extraction section for ITP-type jobs allows specifying an ITPilot
wrapper.

 

The ITP extraction section is similar to the previous description for
VDP-type jobs (see section :ref:`VDP Extraction Section`). The idea is the same, i.e. it
allows a set of queries to be defined against the same wrapper of an ITP
server, allowing concurrency features and a maximum limit for queries to
be configured. The only differences with regard to the VDP wrapper are
as follows:

-  A previously created ITP-type **Data source** needs to be selected.
-  Instead of specifying a parameterized query the name of the wrapper
   to be queried is indicated (**Wrapper name**) and the list of wrapper
   fields that you want to retrieve (**Output fields**). The list of
   wrapper fields plays the role of the variables in the parameterized
   query from the VDP extraction section, and it is for those fields for
   which it is possible to specify data sources (in the case of the
   sources for parameters of ITP wrappers, implicit associations are not
   applicable; it is necessary to always specify associations
   explicitly).

.. important:: ITP jobs are deprecated and will be removed in a later version of the Denodo Platform.
   
   The section :ref:`Features Deprecated in Scheduler 7.0` lists all the features that are deprecated.