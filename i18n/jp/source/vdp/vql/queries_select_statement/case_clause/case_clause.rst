===========
CASE Clause
===========

The ``CASE`` clause provides an *if-then-else* type of logic. 

.. code-block:: bnf
   :caption: CASE syntax
   :name: CASE syntax

   CASE <value:expression> WHEN <compare_value:expression> 
        THEN result [ WHEN <compare_value:expression> THEN result ...] 
        [ ELSE result ]
   END
   
   CASE WHEN <condition> 
        THEN result [ WHEN <condition> THEN result ...] 
        [ ELSE result ]
   END
   
   <condition> ::= 
       <condition> AND <condition>
     | <condition> OR <condition>
     | NOT <condition>
     | ( <condition> )
     | <value> <binary operator> <value> [ , <value> ]*
     | <value> <binary operator> ( <value> [ , <value> ]* )
     | <value> BETWEEN <value> AND <value>
     | <value> <unary operator>

The ``CASE`` clause can be used in two different ways:

#. ``CASE`` evaluates an expression and obtains a value. Then, it
   compares that value with the expression of every ``WHEN`` clause.
   When it founds a match, returns the *result* value.
#. ``CASE`` evaluates the condition of every ``WHEN`` clause until it
   founds a match. When it does, returns the *result* value.

In both cases, if there is no ``ELSE`` clause and there is not any
matching condition, ``CASE`` returns ``NULL``.

All the *result* expressions must have a compatible type. So, for
instance, it is not possible that one *result* has type *boolean* and
other, *integer*. But it is possible that one *result* has type
*integer* and the other *float*.

See the appendix :ref:`CASE Clause Examples` for more examples on
how to use ``CASE``.



