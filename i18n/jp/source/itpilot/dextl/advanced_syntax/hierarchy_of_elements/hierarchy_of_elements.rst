=====================
Hierarchy of Elements
=====================

This section describes the way in which different DEXTL elements can be
arranged inside of a DEXTL program.



A DEXTL program can be composed of several "root" elements (that is,
which are not subelements of another parent element).



A DEXTL element might contain:


-  A name given to the tuples of the relation represented by the element.
   The syntax is:

   NAME="Element Name"

-  A DEXTL pattern. It will be formed by:

   -  The tagset that is going to be used in this pattern. Its declaration
      must appear at the beginning of the pattern with the following
      syntax:
   -  Nested sub-elements which represent subrelations (see section :ref:`Relations and subrelations`). Subelements can have the same
      components than a root element, and they can also specify an
      additional parameter; this represents the attribute name of the
      parent element which will contain all the data tuples of the
      subelements. If not indicated, this parameter will have the same name
      specified for each tuple of the sub-element (through the ``NAME``
      parameter). The syntax is:

      LISTNAME="List Name"

-  Both element types (root or sub-element ones) may have a tag which
   defines the end of the element, called TO pattern (its syntax was
   described in the section :ref:`Relations and
   subrelations`).

   -  In the case of a sub-element, when the TO pattern is found, the
      search for tuples continues back in the parent element.
   -  In the case of a main element, when the TO pattern is found, the page
      continues to be analysed, looking for occurrences of the next main
      element. If there are no more main elements available, the process
      ends. The end of page is also considered as an end-of-element tag for
      any element.


There are two ways for defining sub-elements:

-  Between symbols ``{`` and ``}``: the tuples which have been extracted
   for a sub-element will be considered as the value of an attribute of
   type subrelation, associated with the parent element. For example, if
   we have an element called ``MOVIE`` with atomic attributes ``TITLE``,
   ``ACTOR`` and a subelement ``EDITION`` with atomic attributes
   ``FORMAT`` and ``PRICE``, the schema of the data returned to the
   application would be: ``{TITLE, ACTOR, EDITION{ FORMAT, PRICE}}``.
-  Between symbols {\* and \*}: attributes extracted from the parent
   element are added to each child sub-element. That is, the retrieved
   results are "flattened" before being returned to the application. For
   example, if we have an element called ``MOVIE`` with atomic
   attributes ``TITLE``, ``ACTOR`` and a subelement ``EDITION`` with
   atomic attributes ``FORMAT`` and ``PRICE``, the schema of the data
   returned to the application would be:
   ``{TITLE, ACTOR, FORMAT, PRICE}``.

