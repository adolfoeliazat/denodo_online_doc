=============================================
Attributes with FIXED Value and Default Value
=============================================

In the specification of a pattern it is possible to indicate default
values for the attributes of the tuples to be extracted with the format
``NAME="Value"``, where NAME is the name of the attribute and
``Value`` is the value to be assigned to it. These assignments should go
in the specification at the beginning of the pattern.



This function is useful in three main cases:

-  When one of the attributes of the relation appears only in certain
   tuples (and, thus, appears in an optional part of the pattern) and
   you wish to assign it a default value in those cases in which it does
   not appear.
-  When in a certain source all the tuples take the same value for a
   specific attribute, whereby said attribute no longer appears in the
   results. In this case, the attribute will not appear in the pattern.
-  When an auxiliary value is required to execute an operation or
   calculation. If we do not want these to be returned as attributes of
   the tuples in the relation, as is normal in these cases, simply
   prefix the attribute with ‘::’ instead of the usual ‘:’.



**Example**: Consider a bookstore on the Internet modeled on the
relation R={TITLE, AUTHOR, PRICE, SHIPPINGCOSTS}. Imagine that the store
normally charges an additional 4 euros on each book for shipping costs.
However, in those cases in which the book is particularly big or heavy a
higher cost may be charged and, in this case, this is indicated in the
data provided on the book.



To deal with this situation a pattern could be defined in the following
manner:

.. code-block:: none
   :name: Pattern
   :caption: Pattern

   {NAME="BOOK"
   SHIPPINGCOSTS="4 euros"
   ...
   /?.... :SHIPPINGCOSTS....?/
   ...
   }

As it can be seen, the pattern contains the attribute SHIPPINGCOSTS in
an optional portion of the pattern. For those tuples in which said part
appears, SHIPPINGCOSTS will take the value obtained by the pattern.
Otherwise, it will take the value set by default, which is the character
string "4 euros".
