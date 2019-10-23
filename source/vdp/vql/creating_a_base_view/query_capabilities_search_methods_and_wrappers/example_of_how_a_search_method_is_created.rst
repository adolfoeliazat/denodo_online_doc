=========================================
Example of How a Search Method Is Created
=========================================

The figure `Example of how a search method is created with
ALTER TABLE`_ shows an example of how to add a search method to a
view.



.. code-block:: vql
   :caption: Example of how a search method is created with ALTER TABLE
   :name: Example of how a search method is created with ALTER TABLE

   CREATE TABLE bookview
   ...
   ...
   ...
       ADD SEARCHMETHOD bookview_sm1 (
           CONSTRAINTS (
               ADD TITLE (any) OBL ONE
               ADD AUTHOR (like) OPT ONE
               ADD FORMAT NOS ZERO ()
               ADD PRICE NOS ZERO ()
           )
           OUTPUTLIST (TITLE, AUTOR, FORMAT, PRICE)
       WRAPPER (itp booktest)
   );


This example adds a search method called ``bookview_sm1`` to the base
view ``bookview``. This search method has four query constraints. They
indicate that to make a query to the source, the query has to provide a
value for ``TITLE`` (specifying any number of values). Optionally, a
search can be made for the attribute ``AUTHOR`` (specifying any number
of values) and the operator ``like``. Direct queries for the rest of the
attributes (``FORMAT``, ``PRICE``, etc.) are not admitted. Furthermore,
the search method definition indicates that all the attributes appear in
the output. Finally, the WWW-type *wrapper* (wrapper created with
ITPilot) called ``booktest`` is associated with the search method. It
will be responsible for extracting the results, when a query is executed
using this search method.

.. important:: Although the source does not natively support queries for
   specific attributes (in the previous example this occurs with
   ``FORMAT``, ``PRICE``, etc.), Virtual DataPort can execute some of the
   queries on those attributes through post-processing of the results
   obtained from the sources. For example, if the server receives the query
   ``SELECT * FROM BOOKVIEW WHERE TITLE like 'java' AND FORMAT = 'eBook'``,
   Virtual DataPort is capable of responding by extracting from the source
   the books that contain the word ``java`` in the title (as the source
   does allow this query) and later filter the results to remain with those
   whose field ``FORMAT`` is ``eBook``.

