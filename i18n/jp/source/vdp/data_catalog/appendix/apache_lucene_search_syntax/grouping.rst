========
Grouping
========

Lucene supports using parentheses to group clauses to form subqueries.
This can be very useful, if you want to control the Boolean logic for a
query.



To search for either "jakarta" or "apache", and "Web" use the query:


.. code-block:: none
   
   (jakarta OR apache) AND Web


This eliminates any confusion and makes you sure that "Web" must
exist and either term "jakarta" or "apache" may exist.


