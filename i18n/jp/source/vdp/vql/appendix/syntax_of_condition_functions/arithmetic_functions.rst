====================
Arithmetic Functions
====================

Arithmetic functions are applied to values of the types ``decimal``, ``int``,
``long``, ``float`` and ``double``.

In general, if a function accepts numeric arguments of different types,
the type of the result will be the most generic type. For instance, the
addition of an ``int`` value and a ``double`` value returns a ``double``
value.

.. contents:: Arithmetic functions supported by Virtual DataPort
   :depth: 1
   :local:
   :class: threecols

ABS
=================================================================================

.. rubric:: Description

The ``ABS`` function returns the absolute value of a number.

.. rubric:: Syntax

.. code-block:: bnf

   ABS( <value:numeric> ):numeric

-  ``value``. Required. The number of which absolute value will be
   calculated.

.. rubric:: Example



.. code-block:: sql

   SELECT ABS(-5) as absolute_value
   FROM Dual()

+----------------+
| absolute_value |
+================+
| 5              |
+----------------+


ACOS
=================================================================================

.. rubric:: Description

The ``ACOS`` function returns the arc cosine of an angle.

The input has to be a number between ``-1.0`` and ``+1.0``.

.. rubric:: Syntax

.. code-block:: bnf

   ACOS( <value:numeric> ):double

-  ``value``. Required. It cannot be a value of type ``decimal``.

.. rubric:: Example



.. code-block:: sql

   SELECT ACOS(0), ACOS(-1), ACOS(1)





+-------------------------+-------------------------+-------------------------+
| acos                    | acos\_1                 | acos\_2                 |
+=========================+=========================+=========================+
| 1.57                    | 3.14                    | 0.0                     |
+-------------------------+-------------------------+-------------------------+


ASIN
=================================================================================

.. rubric:: Description

The ``ASIN`` function returns the arc sine of an angle.

The input has to be a number between ``-1.0`` and ``+1.0``.

.. rubric:: Syntax

.. code-block:: bnf

   ASIN( <value:numeric> ):double

-  ``value``. Required. It cannot be a value of type ``decimal``.

.. rubric:: Example



.. code-block:: sql

   SELECT ASIN(0), ASIN(1), ASIN(-1)


+-------------------------+-------------------------+-------------------------+
| asin                    | asin\_1                 | asin\_2                 |
+=========================+=========================+=========================+
| 0.0                     | 1.57                    | -1.57                   |
+-------------------------+-------------------------+-------------------------+


ATAN
=================================================================================

.. rubric:: Description

The ``ATAN`` function returns the arc tangent of an angle.

The input has to be a number between ``-1.0`` and ``+1.0``.

.. rubric:: Syntax

.. code-block:: bnf

   ATAN( <value:numeric> ):double

-  ``value``. Required. It cannot be a value of type ``decimal``.

.. rubric:: Example



.. code-block:: sql

   SELECT RADIANS(90), ATAN(RADIANS(-90)), ATAN(RADIANS(90))





+-------------------------+-------------------------+-------------------------+
| radians                 | atan                    | atan\_1                 |
+=========================+=========================+=========================+
| 1.57                    | -1.0                    | 1.0                     |
+-------------------------+-------------------------+-------------------------+

.. note:: The function ``RADIANS`` converts an angle in degrees to
   radians. See more about this function in the section :ref:`RADIANS`.


ATAN2
=================================================================================

.. rubric:: Description

The ``ATAN2`` function converts rectangular coordinates (x, y) to polar
(r, theta). It computes the phase theta by computing an arc tangent of
``y``/``x`` in the range of ``-pi`` to ``+pi``.

.. rubric:: Syntax

.. code-block:: bnf

   ATAN2( <x:numeric>, <y:numeric>):double

-  ``x``. Required.
-  ``y``. Required

Neither ``x`` nor ``y`` can be values of type ``decimal``.


CEIL
=================================================================================

.. rubric:: Description

The ``CEIL`` function returns the smallest integer not less that the
argument.

.. rubric:: Syntax

.. code-block:: bnf

   CEIL( <value:decimal> ):long

   CEIL( <value:int> ):int
   
   CEIL( <value:long> ):long

-  ``value``. Required. The value to round off.

.. rubric:: Example



.. code-block:: sql

   SELECT CEIL(5.08) as ceil_value
   FROM Dual()

+--------------------------------------------------------------------------+
| ceil\_value                                                              |
+==========================================================================+
| 6                                                                        |
+--------------------------------------------------------------------------+


COS
=================================================================================

.. rubric:: Description

The ``COS`` function returns the cosine of an angle in radians.

The output is a ``double`` value between ``-1.0`` and ``+1.0``.

.. rubric:: Syntax

.. code-block:: bnf

   COS( <angle:numeric> ):double

-  ``angle``. Required. It cannot be a value of type ``decimal``.

.. rubric:: Example



.. code-block:: sql

   SELECT COS(0), COS(RADIANS(180))


+--------------------------------------+--------------------------------------+
| cos                                  | cos\_1                               |
+======================================+======================================+
| 1.0                                  | -1.0                                 |
+--------------------------------------+--------------------------------------+


COT
=================================================================================

.. rubric:: Description

The COT function returns the cotangent of an angle in radians.

.. rubric:: Syntax

.. code-block:: bnf

   COT( <angle:numeric> ):double

-  ``angle``. Required.

.. rubric:: Example

.. code-block:: sql

   SELECT COT( RADIANS (45))

+--------------------------------------------------------------------------+
| cot                                                                      |
+==========================================================================+
| 1.0                                                                      |
+--------------------------------------------------------------------------+


DEGREES
=================================================================================

.. rubric:: Description

The ``DEGREES`` function, given an angle in radians, returns the
corresponding angle in degrees.

.. rubric:: Syntax

.. code-block:: bnf

   DEGREES( <angle:numeric> ):double

-  ``angle``. Required. It cannot be a value of type ``decimal``.

.. rubric:: Example


.. code-block:: sql

   SELECT DEGREES(0), DEGREES(3.15 * 2)

+--------------------------------------+--------------------------------------+
| degrees                              | degrees\_1                           |
+======================================+======================================+
| 0.0                                  | 360.96                               |
+--------------------------------------+--------------------------------------+


DIV
=================================================================================

.. rubric:: Description

The ``DIV`` function divides two numbers.

.. rubric:: Syntax

.. code-block:: bnf

   DIV( <dividend:numeric>, <divisor:numeric> ):numeric

-  ``dividend``. Required. The dividend of the operation.
-  ``divisor``. Required. The divisor of the operation.

**Examples**

**Example 1**

.. code-block:: sql

   SELECT DIV(10, 2.5) as div_value
   FROM Dual();

+--------------------------------------------------------------------------+
| div\_value                                                               |
+==========================================================================+
| 4.0                                                                      |
+--------------------------------------------------------------------------+

**Example 2**

.. code-block:: sql

   SELECT (10 / CAST('double', 2)) as div_value
   FROM Dual();


+--------------------------------------------------------------------------+
| div\_value                                                               |
+==========================================================================+
| 5.0                                                                      |
+--------------------------------------------------------------------------+


EXP
=================================================================================

.. rubric:: Description

The ``EXP`` function returns the exponential value of a number.

.. rubric:: Syntax

.. code-block:: bnf

   EXP( <value:numeric> ):double

-  ``value``. Required.

.. rubric:: Example



.. code-block:: sql

   SELECT EXP(0), EXP(1)


+--------------------------------------+--------------------------------------+
| exp                                  | exp\_1                               |
+======================================+======================================+
| 1.0                                  | 2.72                                 |
+--------------------------------------+--------------------------------------+


FLOOR
=================================================================================

.. rubric:: Description

The ``FLOOR`` function returns the largest integer not greater than the
argument.

.. rubric:: Syntax

.. code-block:: bnf

   FLOOR( <value:decimal> ):long

   FLOOR( <value:int> ):int
   
   FLOOR( <value:long> ):long

-  ``value``. Required. Value to round off.

.. rubric:: Example

.. code-block:: sql

   SELECT FLOOR(5.98) as floor_value
   FROM Dual();


+--------------------------------------------------------------------------+
| floor\_value                                                             |
+==========================================================================+
| 5                                                                        |
+--------------------------------------------------------------------------+


LN
=================================================================================

.. rubric:: Description

The ``LN`` function returns the natural logarithm (base e) of a value.

.. rubric:: Syntax

.. code-block:: bnf

   LN( <value:numeric> ):double

-  ``value``. Required. It cannot be a value of type ``decimal``.

.. rubric:: Example



.. code-block:: sql

   SELECT LN( EXP( 0 ) ), LN ( EXP(1) )
   FROM Dual();


+--------------------------------------+--------------------------------------+
| ln                                   | ln\_1                                |
+======================================+======================================+
| 0.0                                  | 1.0                                  |
+--------------------------------------+--------------------------------------+


LOG
=================================================================================

.. rubric:: Description

The ``LOG`` function returns the logarithm of a number.

.. rubric:: Syntax

.. code-block:: bnf

   LOG( <value:numeric> [, <base:numeric> ]):double

-  ``value``. Required. Positive real number for which you want the
   logarithm. It cannot be a value of type ``decimal``.
-  ``base``. Optional. If not present, the function returns the
   logarithm of the number in base-ten.

.. rubric:: Example



.. code-block:: sql

   SELECT log(100), log(100, 10);
   FROM Dual();

+--------------------------------------+--------------------------------------+
| log                                  | log\_1                               |
+======================================+======================================+
| 2.0                                  | 2.0                                  |
+--------------------------------------+--------------------------------------+


.. _arithmetic_function_max:

MAX
=================================================================================

.. rubric:: Description

The ``MAX`` function returns the maximum value in a list of arguments. Returns NULL
if any of the arguments is NULL.

.. rubric:: Syntax

.. code-block:: bnf

   MAX( <value 1:numeric>, <value 2:numeric> [, <value N:numeric> ]* ):numeric

-  ``value 1``. Required.
-  ``value 2``. Required.
-  ``value N``. Optional. One or more values.

.. rubric:: Example

.. code-block:: sql

   SELECT MAX(5, 10, 3.2) as max_value
   FROM Dual();

+--------------------------------------------------------------------------+
| max\_value                                                               |
+==========================================================================+
| 10.0                                                                     |
+--------------------------------------------------------------------------+

.. note:: 
    In previous versions of Denodo, this function only returns 
    NULL when all of the arguments are NULL. To restore the behavior of previous versions, execute this 
    command from the VQL Shell: 
       
    .. code-block:: sql
    
       SET 'com.denodo.vdb.catalog.view.functions.max_min.ignoreNullValuesForComparisons'='true';

    This change is applied immediately. You do not need to restart.


.. _arithmetic_function_min:

MIN
=================================================================================

.. rubric:: Description

The ``MIN`` function returns the minimum value in a list of arguments. Returns NULL
if any of the arguments is NULL.

.. rubric:: Syntax

.. code-block:: bnf

   MIN( <value 1:numeric> [, <value N:numeric> ]* ):numeric

-  ``value 1``. Required.
-  ``value N``. Optional. One or more values.

.. rubric:: Example



.. code-block:: sql

   SELECT MIN(5, 10, 3.2) as min_value
   FROM Dual();


+--------------------------------------------------------------------------+
| min\_value                                                               |
+==========================================================================+
| 3.2                                                                      |
+--------------------------------------------------------------------------+

.. note:: 
    In previous versions of Denodo, the function ``MIN`` only returns 
    NULL when all of the arguments are NULL. To restore this behavior, you can execute:  
      
    .. code-block:: sql
    
        SET 'com.denodo.vdb.catalog.view.functions.max_min.ignoreNullValuesForComparisons'='true'
   
MOD
=================================================================================

.. rubric:: Description

The ``MOD`` function returns the result of the module operation: the
remainder of the integer division of the first and second arguments.

This function has an infix version and its operator is ``%``.

.. rubric:: Syntax

.. code-block:: bnf

   MOD( <dividend:decimal>, <divisor:decimal> ):decimal

   MOD( <dividend:double>, <divisor:double> ):double
   
   MOD( <dividend:float>, <divisor:float> ):double
   
   MOD( <dividend:int>, <divisor:int> ):int
   
   MOD( <dividend:long>, <divisor:int> ):int
   
   MOD( <dividend:long>, <divisor:long> ):long

-  ``dividend``. Required.
-  ``divisor``. Required.

**Examples**

Consider the following view ``V``:

+--------------------------------------+--------------------------------------+
| int\_sample                          | long\_sample                         |
+======================================+======================================+
| 1                                    | 10                                   |
+--------------------------------------+--------------------------------------+
| -4                                   | -55                                  |
+--------------------------------------+--------------------------------------+
| 8                                    | 70                                   |
+--------------------------------------+--------------------------------------+

And the view ``modView`` created with the command:


.. code-block:: sql
   :caption: **Example 1**

   CREATE VIEW mod_view AS
       SELECT int_sample
           , MOD(int_sample, 2) AS s1
           , long_sample
           , MOD(long_sample, 2) as s2
   FROM V;

.. code-block:: sql

   SELECT * 
   FROM mod_view

+--------------------+--------------------+--------------------+--------------------+
| int\_sample        | s1                 | long\_sample       | s2                 |
+====================+====================+====================+====================+
| 1                  | 1                  | 10                 | 0                  |
+--------------------+--------------------+--------------------+--------------------+
| -4                 | 0                  | -55                | -1                 |
+--------------------+--------------------+--------------------+--------------------+
| 8                  | 0                  | 70                 | 0                  |
+--------------------+--------------------+--------------------+--------------------+

**Example 2**



.. code-block:: sql

   SELECT 10%2 
   FROM mod_view

+--------------------------------------------------------------------------+
| mod                                                                      |
+==========================================================================+
| 0                                                                        |
+--------------------------------------------------------------------------+
| 0                                                                        |
+--------------------------------------------------------------------------+
| 0                                                                        |
+--------------------------------------------------------------------------+


MULT
=================================================================================

.. rubric:: Description

The ``MULT`` function multiplies its arguments.

.. rubric:: Syntax

.. code-block:: bnf

   MULT ( <value 1:numeric>, <value 2:numeric> [, <value N:numeric> ]* ):numeric

-  ``value 1``. Required. First number to be multiplied.
-  ``value 2``. Required. Second number to be multiplied.
-  ``value N``. Optional. One or more arguments to be multiplied.

**Examples**

**Example 1**


.. code-block:: sql

   SELECT MULT(10, 2.5) as mult_value
   FROM Dual();


+--------------------------------------------------------------------------+
| mult\_value                                                              |
+==========================================================================+
| 25.0                                                                     |
+--------------------------------------------------------------------------+

**Example 2**

.. code-block:: sql

   SELECT (10 * 2.5) as mult_value
   FROM Dual();

+--------------------------------------------------------------------------+
| mult\_value                                                              |
+==========================================================================+
| 25.0                                                                     |
+--------------------------------------------------------------------------+


PI
=================================================================================

.. rubric:: Description

The ``PI`` function returns the Pi number with precision ``double``.

.. rubric:: Syntax

.. code-block:: bnf

   PI():double

.. rubric:: Example

.. code-block:: sql

   SELECT PI() as pi_constant
   FROM Dual();


+--------------------------------------------------------------------------+
| pi\_constant                                                             |
+==========================================================================+
| 3.141592653589793                                                        |
+--------------------------------------------------------------------------+


POWER
=================================================================================

.. rubric:: Description

The ``POWER`` function returns the result of a number raised to a power.

.. rubric:: Syntax

.. code-block:: bnf

   POWER( <number:numeric>, <power:numeric> ):double

-  ``number``. Required. Base number.
-  ``power``. Required. Exponent to which the base number is raised.

Neither ``number`` nor ``power`` can be values of type ``decimal``.

.. rubric:: Example

.. code-block:: sql

   SELECT POWER(5, 2) as power_value
   FROM Dual();


+--------------------------------------------------------------------------+
| power\_value                                                             |
+==========================================================================+
| 25                                                                       |
+--------------------------------------------------------------------------+


RADIANS
=================================================================================

.. rubric:: Description

The ``RADIANS`` function, given an angle in degrees, returns the
corresponding angle in radians.

.. rubric:: Syntax

.. code-block:: bnf

   RADIANS( <angle:numeric> ):double

-  ``angle``. Required. It cannot be a value of type ``decimal``.

.. rubric:: Example



.. code-block:: sql

   SELECT RADIANS(0), RADIANS(360)


+--------------------------------------+--------------------------------------+
| radians                              | radians\_1                           |
+======================================+======================================+
| 0.0                                  | 6.28                                 |
+--------------------------------------+--------------------------------------+


RAND
=================================================================================

.. rubric:: Description

The ``RAND`` function returns a random value between zero and one.

.. rubric:: Syntax

It does not receive any parameter.

.. rubric:: Example

Consider the view ``V``:

+--------------------------------------------------------------------------+
| int\_sample                                                              |
+==========================================================================+
| 1                                                                        |
+--------------------------------------------------------------------------+
| -4                                                                       |
+--------------------------------------------------------------------------+
| 8                                                                        |
+--------------------------------------------------------------------------+

.. code-block:: sql

   SELECT int_sample, int_sample * RAND() AS random
   FROM V


+--------------------------------------+--------------------------------------+
| int\_sample                          | random                               |
+======================================+======================================+
| -4                                   | -3.551409143605859                   |
+--------------------------------------+--------------------------------------+
| 1                                    | 0.6443357973998833                   |
+--------------------------------------+--------------------------------------+
| 8                                    | 1.5061178485934867                   |
+--------------------------------------+--------------------------------------+


ROUND
=================================================================================

.. rubric:: Description

The ``ROUND`` function returns a number rounded to the specified number
of places to the right or left of the decimal place.

.. rubric:: Syntax

.. code-block:: bnf

   ROUND( <value:numeric [, n : integer ] ):numeric

-  ``value``. Required. Value to round off.
-  ``n``. Optional.

If ``n`` is omitted, ``value`` is rounded to 0 places. If the argument
has ``int`` type, it returns an ``int`` value. If the argument has
``long`` type, ``float`` or ``double``, it returns a ``long`` value.

If ``n`` is negative, ``value`` is rounded to digits left of the decimal
point. In this case, the function will return a value of the same type
as the ``value`` parameter. I.e., if the type of ``value`` is ``int``,
it returns an ``int`` value, if it is ``float``, it returns a ``float``
value, etc.

.. rubric:: Example

.. code-block:: sql

   SELECT ROUND(5.98), ROUND(7.08733, 2), ROUND(315.28, -2)
   FROM Dual();
   
+-------------------------+-------------------------+-------------------------+
| round                   | round\_1                | round\_2                |
+=========================+=========================+=========================+
| 6                       | 7.09                    | 300.0                   |
+-------------------------+-------------------------+-------------------------+


SIGN
=================================================================================

.. rubric:: Description

The ``SIGN`` function returns ``-1``, ``0`` or ``1``, depending on
whether the value is negative, zero, or positive respectively. Use this
function to know the sign of a number.

.. rubric:: Syntax

.. code-block:: bnf

   SIGN( <value:numeric> ):int

-  ``value``. Required. If ``NULL``, the function returns ``NULL``.

.. rubric:: Example



.. code-block:: sql

   SELECT SIGN(100), SIGN(-50)


+--------------------------------------+--------------------------------------+
| sign                                 | sign\_1                              |
+======================================+======================================+
| 1                                    | -1                                   |
+--------------------------------------+--------------------------------------+


SIN
=================================================================================

.. rubric:: Description

The ``SIN`` function returns the sine of an angle in radians.

The output is a ``double`` value between ``-1.0`` and ``+1.0``.

.. rubric:: Syntax

.. code-block:: bnf

   SIN( <angle:numeric> ):double

-  ``angle``. Required. It cannot be a value of type ``decimal``.

.. rubric:: Example



.. code-block:: sql

   SELECT SIN(0), SIN(RADIANS(90))


+--------------------------------------+--------------------------------------+
| sin                                  | sin\_1                               |
+======================================+======================================+
| 0.0                                  | 1.0                                  |
+--------------------------------------+--------------------------------------+


SQRT
=================================================================================

.. rubric:: Description

The ``SQRT`` function returns a positive square root.

.. rubric:: Syntax

.. code-block:: bnf

   SQRT( <value:numeric> ):double

-  ``value``. Required. Number for which you want the square root. It
   cannot be a value of type ``decimal``.

.. rubric:: Example



.. code-block:: sql

   SELECT SQRT(25) as sqrt_value
   FROM Dual();

+--------------------------------------------------------------------------+
| sqrt\_value                                                              |
+==========================================================================+
| 5.0                                                                      |
+--------------------------------------------------------------------------+

.. _vql-guide-arithmetic-processing-functions-subtract:

SUBTRACT
=================================================================================

.. rubric:: Description

The ``SUBTRACT`` function subtracts two numbers or two datetime typed values.

If you subtract two values of type ``localdate``, ``timestamp``, ``timestamptz`` or ``date``, you get the number of whole days between the
second date and the first date. If both dates belong to the same day,
the function returns 0 even if the time is different. When subtracting two values of type ``time``, the function will return the number of milliseconds between the two values.

This function also provides an infix notation. I.e. ``subtract(a, b)``
is equivalent to ``a - b``.

.. rubric:: Syntax

.. code-block:: bnf

   SUBTRACT( <value 1:numeric>, <value 2:numeric> ):numeric

   SUBTRACT( <value 1:localdate>, <value 2:localdate> ):long

   SUBTRACT( <value 1:timestamp>, <value 2:timestamp> ):long

   SUBTRACT( <value 1:timestamptz>, <value 2:timestamptz> ):long

   SUBTRACT( <value 1:time>, <value 2:time> ):long

   SUBTRACT( <value 1:date>, <value 2:date> ):long
      

-  ``value 1``. Required. First value to be subtracted from.
-  ``value 2``. Required. Second value to be subtracted.

**Examples**

**Example 1**

.. code-block:: sql

   SELECT SUBTRACT(10, 2.5) as subtract_value
   FROM Dual();

+--------------------------------------------------------------------------+
| subtract\_value                                                          |
+==========================================================================+
| 7.5                                                                      |
+--------------------------------------------------------------------------+

**Example 2**


.. code-block:: sql

   SELECT (10 - CAST('int', 2.5)) as subtract_value
   FROM Dual();

+--------------------------------------------------------------------------+
| subtract\_value                                                          |
+==========================================================================+
| 8                                                                        |
+--------------------------------------------------------------------------+

**Example 3**

.. code-block:: sql

   SELECT
       SUBTRACT (
             DATE '2015-01-02'
           , DATE '2015-01-01') as value_1
       , SUBTRACT (
             TIMESTAMP '2015-01-01 01:00'
           , TIMESTAMP '2015-01-01 08:00') as value_2


+--------------------------------------+--------------------------------------+
| value\_1                             | value\_2                             |
+======================================+======================================+
| 1                                    | 0                                    |
+--------------------------------------+--------------------------------------+


SUM
=================================================================================

.. rubric:: Description

The ``SUM`` function adds its arguments.

.. rubric:: Syntax

.. code-block:: bnf

   SUM( <value1:numeric, value2:numeric [, valueN:numeric ]* ):numeric

-  ``value1``. Required. First number to be added.
-  ``value2``. Required. Second number to be added.
-  ``valueN``. Optional. One or more arguments to be added.

**Examples**

**Example 1**


.. code-block:: sql

   SELECT SUM(1, CAST('double', 2.5), 4.6) as sum_value
   FROM Dual();


+--------------------------------------------------------------------------+
| sum\_value                                                               |
+==========================================================================+
| 8.1                                                                      |
+--------------------------------------------------------------------------+

**Example 2**



.. code-block:: sql

   SELECT (1 + CAST('int', 2.9) + 4.6) as sum_value
   FROM Dual();

+--------------------------------------------------------------------------+
| sum\_value                                                               |
+==========================================================================+
| 7.6                                                                      |
+--------------------------------------------------------------------------+


TAN
=================================================================================

The ``TAN`` function returns the tangent of an angle in radians.

.. rubric:: Syntax

.. code-block:: bnf

   TAN ( <angle:numeric> ):double

-  ``angle``. Required.

.. rubric:: Example



.. code-block:: sql

   SELECT TAN(0), TAN(radians(45))

+--------------------------------------+--------------------------------------+
| tan                                  | tan\_1                               |
+======================================+======================================+
| 0.0                                  | 1.0                                  |
+--------------------------------------+--------------------------------------+


TRUNC
=================================================================================

The ``TRUNC`` function returns the integer part of a number.

.. rubric:: Syntax

.. code-block:: bnf

   TRUNC( <value:numeric> ):long

-  ``value``. Required.

.. rubric:: Example



.. code-block:: sql

   SELECT TRUNC(1), TRUNC(2.8), TRUNC(-3.9)


+-------------------------+-------------------------+-------------------------+
| trunc                   | trunc\_1                | trunc\_2                |
+=========================+=========================+=========================+
| 1                       | 2                       | -3                      |
+-------------------------+-------------------------+-------------------------+


