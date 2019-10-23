.. _itp_gen_environment_guide_case_clause:

===========
Case Clause
===========

The CASE clause is not a function itself but it can be used wherever a
function can be used. It provides an if-then-else type of logic. The
syntax for the case clause is shown in `CASE syntax`_.


.. code-block:: bnf
   :name: CASE syntax
   :caption: CASE syntax

   CASE <value:expression> WHEN <compare_value:expression>     
        THEN result [ WHEN <compare_value:expression> THEN result ... ]
        [ ELSE result ]                                                
   END                                                          
   CASE WHEN <condition>                                        
        THEN result [ WHEN <condition> THEN result ... ]                
        [ ELSE result ]                                                
   END    
   
   <condition> ::=                                              
       <condition> AND <condition>                                  
     | <condition> OR <condition>                                
     | NOT <condition>                                           
     | ( <condition> )                                           
     | <value> <binary operator> <value>                         


The CASE clause can be used in two different ways:

#. CASE evaluates an expression and obtains a value. Then, it compares
   that value with the result of evaluating the expression of every WHEN
   clause. When it founds a match, returns the associated result value.
#. CASE evaluates the condition of every WHEN clause until it founds a
   match (i.e. a condition which evaluates to ``true``). When it does,
   returns the result value.



In both versions, if there is no ELSE clause and there is not any
matching condition, CASE returns NULL.



All the result expressions must have a compatible type. So, for
instance, it is not possible that one result has type ``boolean`` and
other, ``int``. But it is possible that one result has type ``int`` and
the other ``float``.

