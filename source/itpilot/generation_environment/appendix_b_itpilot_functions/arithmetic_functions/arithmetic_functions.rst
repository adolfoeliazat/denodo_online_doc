.. _itp_gen_environment_guide_arithmetic_functions:

====================
Arithmetic Functions
====================

Arithmetic functions are applied to numeric-type attributes and
constants: ``int``, ``long``, ``float`` and ``double``. If a function
receives as arguments two values of different type, the returned value
will usually be of the most general type. For instance, if a sum between
an ``int`` and a ``float`` is executed, a ``float`` will be returned as
a result.



The supported arithmetic functions are:

-  ``ABS``: The ``abs`` function receives one sole numeric-type argument
   and returns as a result its absolute value.
-  ``CEIL``: This function receives a numeric argument and returns the
   smallest integer, greater than or equal to the argument, closest to
   the argument.
-  ``DIV``: The div function receives two numeric-type arguments and
   returns a new element of the same type with the result of dividing
   the first argument by the second. If the arguments are integers, the
   result of the division will also be an integer.
-  ``FLOOR``: This function receives a numeric argument and returns the
   biggest integer, lesser than or equal to the argument, closest to the
   argument.
-  ``LOG``: This function is given a numeric argument and returns a
   ``double``-type value with the result of the base 10 logarithm of the
   argument.
-  ``MAX``: The *max* function receives two or more numeric arguments
   and returns as a result the value of the biggest argument of the
   list.
-  ``MIN``: The *min* function receives two or more numeric arguments
   and returns as a result the value of the smallest argument of the
   list.
-  ``MOD``: The *mod* function receives two ``int``- or ``long``-type
   arguments and returns the result of the module operation between the
   first argument and the second (the remainder of the full division of
   the first and second arguments).
-  ``MULT``: The *mult* function receives two or more numeric arguments
   and returns a new element of the same type with the result of
   multiplying the different arguments.
-  ``POWER``: This function is given two numeric arguments, the second
   of which must be an integer. It returns a ``double``-type value
   result obtained through the exponentiation of the first argument with
   the second as the exponent.
-  ``RAND``: The rand function receives no arguments and returns a
   pseudorandom double value uniformly distributed between 0.0 and 1.0.
-  ``ROUND``: This function receives a numeric argument and returns as a
   result the integer number closest to the argument.
-  ``SQRT``: This function is given a numeric argument and returns a
   ``double``-type value with the result of the square root of the
   argument.
-  ``SUBTRACT``: The subtract function receives two arguments and
   returns a new element of the same type with the result of subtracting
   the value of the second argument from that of the first.
-  ``SUM``: The sum function receives two or more numeric arguments and
   returns as a result a new element of the same type containing the sum
   of those preceding.
