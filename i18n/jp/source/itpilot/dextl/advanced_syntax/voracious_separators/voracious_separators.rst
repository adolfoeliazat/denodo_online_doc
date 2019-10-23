====================
Voracious Separators
====================

String-type separators operate by attempting to match the first
occurrence of the separator in the corresponding TEXT-type token.



However, it is sometimes useful for the separators to try to match the
last occurrence of the separator within the token sequence. For this,
the separator is simply prefixed with the character ‘:’. This type of
separator is called "voracious".



**Example**: Imagine that an electronic bookshop that lists the
different authors of a book separated by commas and that uses this same
separator to separate the list of authors from the price of the book:



.. code-block:: html
   :name: Presentation of the Results in a Web Source
   :caption: Presentation of the Results in a Web Source
   
   Ron Rivest, Rick Adleman, 12 euros; ...

If we were to write:

.. code-block:: none
   :name: Erroneous specification
   :caption: Erroneous specification

   ... :AUTHORS "," :PRICE ";" ...


Then the attribute AUTHORS is assigned the value ‘Ron Rivest’ and PRICE
the value ‘Rick Adleman, 12 euros’, which is probably not the required
result.



However, we can indicate to the system that it should match the
attribute AUTHORS with the TEXT found until the final "," which precedes
the pattern :PRICE ";", writing the character ":" before ",":

.. code-block:: none
   :name: Correct specification with voracious separators
   :caption: Correct specification with voracious separators
   
   ... :AUTHORS :"," :PRICE ";" ...


AUTHORS is assigned the value ‘Ron Rivest, Rick Adleman’ and PRICE the
value ‘12 euros’.
