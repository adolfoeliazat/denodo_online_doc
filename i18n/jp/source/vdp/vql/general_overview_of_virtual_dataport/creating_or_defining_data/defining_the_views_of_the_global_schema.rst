=======================================
Defining the Views of the Global Schema
=======================================

Once the base relations have been defined and their corresponding
wrappers constructed, each relation of the global schema is
defined through a query on the base relations in a manner similar to
that used to define views on a conventional database.

It is important to highlight that when defining a view, in addition to
the base relations, previously defined views can also be used.

Example: Take three base relations

|  A = {TITLE, AUTHOR, FORMAT, PRICE}
|  B = {TITLE, AUTHOR, FORMAT, PRICE}
|  C = {TITLE, AUTHOR, AVERAGE\_RELEVANCE}

A and B represent two electronic bookshops on the Internet.
C represents a source in which users review books and the system
allows the average review for a specific book to be searched for.
Imagine that we want to obtain a relation of the global schema

   R = {TITLE, AUTHOR, PRICE, AVERAGE\_RELEVANCE)

that contains all the books of ``A`` and ``B``, together with the
average review according to ``C`` and the minimum value found for the
``PRICE`` attribute, amongst the occurrences of the book found in both
sources. ``R`` can be defined in the two following steps:

#. Creating the view ``bookview`` like the union of A and B.

.. code-block:: sql

   CREATE VIEW bookview AS
      SELECT * FROM A
      UNION
      SELECT * FROM B;

2. Creating the view ``R`` as the join of ``bookview`` and ``C`` by
   applying the ``GROUP BY`` operation to select the minimum price for
   each book.

.. code-block:: sql

   CREATE VIEW R AS
      SELECT bookview.TITLE
         , bookview.AUTHOR
         , AVERAGE_RELEVANCE,
      MIN(PRICE) AS MINIMUM
      FROM
         bookview JOIN C
         ON bookview.TITLE = C.TITLE
         AND bookview.AUTHOR = C.AUTHOR
      GROUP BY bookview.TITLE, bookview.AUTHOR, AVERAGE_RELEVANCE;

As mentioned above, the base views can present limitations in their
query capabilities, which are expressed through search methods. When
creating views Virtual DataPort can automatically calculate its search
methods from those of the base relations and from the statement used to
define the view. This allows the system to know *a priori* if a specific
query can be answered.

Post-Processing
=================================================================================

When considering the query capabilities of a source, it is also
important to bear in mind that the Virtual DataPort server can carry out
post-processing operations on the results obtained from said source.
From the query constraints of a source it is possible to obtain its list
of capabilities as a superset of same by applying post-processing. This
task is carried out automatically by the server.

