==========================
Alternative Specifications
==========================

Sometimes the format in which the tuples of a specific relation are
represented can vary according to the query made to the source, but
without it being possible to determine *a priori* which of the output
formats is going to be used.



**Example**: In many electronic shops, when a search offers just one
product as a result, it jumps directly to the format of product
detail page instead of the usual search results page.
However, before launching the search it is not possible to know if this
will return one or several products as a result, whereby it is not
possible to determine *a priori* which pattern should be used to extract
the tuples from the relation.



DEXTL provides a function to deal with this type of case, which consists
in defining DEXTL elements to extract tuples from all the possible
document formats and separate them with the symbol ``||`` to indicate
which are *alternative elements*. The DEXTL program will try the top
element first. If it does not find tuples that match, it goes on to try
the second and so forth. Given that the program goes to the element in
the top position first, it is a good idea to put the element that
represents the most frequent format in said position.



Thus, for the above example, a specification of the following type could
be constructed:

.. code-block:: none
   :name: Alternative Patterns
   :caption: Alternative Patterns

   {
       //Pattern of the search result page 
       .....
   }
   ||
   {
       //Pattern of the product detail page 
       .....
   }

.. note:: Alternative specifications can only be used in first-level
   patterns. If alternatives are found in deeper levels, the (pattern1 \|
   pattern) format must be used inside of the specific pattern.


