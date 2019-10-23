==============
Field Grouping
==============

Lucene supports using parentheses to group multiple clauses to a single
field.



To search for a title that contains both the word "return" and the
phrase "pink panther" use the query:


.. code-block:: none
   
   title:(+return +"pink panther")
