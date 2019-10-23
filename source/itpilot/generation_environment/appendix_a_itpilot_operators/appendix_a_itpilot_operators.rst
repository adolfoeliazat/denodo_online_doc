=============================
Appendix A: ITPilot Operators
=============================


This appendix describes the operators provided by ITPilot to create
expressions used in the Condition, Expression and Record Constructor
components.

Logical Operators
=================

Logical operators are used to create Boolean expressions (which are
evaluated as ``true`` or ``false``). The logical operators supported
are:

-  ``AND``: Is the logical “and”. Evaluates two conditions and returns a
   true value only if both are correct.
-  ``OR``: Is the logical “or”. Evaluates two conditions and returns a
   true value, if one of the two is correct.
-  ``NOT``: Is the logical negation. It is applied to a condition and
   negates its value.

Comparison Operators
====================

An operator of this type returns the logical value ``true`` or ``false``
according to the evaluation result of two operands. Depending on the
nature of the operator the operands should be of a specific data type.
When the right operand of an operator is an array of elements, this must
be introduced with the following format:
``[value1, value2, ...., valueN]``.



The comparison operators are the following:


-  ``<``: Receives two operands that can be of the types: ``int``,
   ``long``, ``float``, ``double`` or ``date``. Evaluated as ``true`` if
   the first operand is less than the second.

-  ``<=``: Applied to two operands of the same type as in the operator
   ``<`` and is evaluated as ``true`` if the first operand is less than or
   equal to the second.

-  ``>``: Receives two operands that can be of the types: ``int``,
   ``long``, ``float``, ``double``, or ``date``. Checks if the first
   operand is greater than the second.

-  ``>=``: Applied to two operands of the same types as the operator ``>``
   and is evaluated as ``true`` if the first operand is greater than or
   equal to the second.

-  ``=``: Receives two operands that can be of the types: ``int``,
   ``long``, ``float``, ``double``, ``boolean``, ``string``, ``url``,
   ``page``, ``list``, ``record`` and ``date``. Evaluates the equality of
   the two operands.

-  ``<>``: Applied to two operands of the same types as the operator ``=``
   and is evaluated as ``true`` if the first operand is not equal to the
   second.

-  ``like``: Accepts one ``string``-type element and one *SQL LIKE
   expression* as operands. It checks if the character string matches the
   expression received. The expression must follow standard SQL format for
   the expressions used with the SQL like operator:

   -  The character ``%`` represents a segment of any length within a
      character string.
   -  The character ``_`` represents a string of length 1.

   For example, the expression ``%commerce_`` matches any string ending
   with the substring ``commerce`` followed by any character. If the
   characters ``%`` or ``_`` are included as part of a constant substring,
   they must be escaped by prefixing them with the character ``$``. If the
   escape character is included, it must be escaped as well (e.g. ``$$``).

-  ``regexp_like``: Applied to two operands, a ``string``-type parameter
   and a regular expression (`Java Regular Expressions <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_).
   It checks if the ``string``-type parameter matches the regular
   expression.

   If you want to ignore the case differences, you should use the operator
   ``regexp_ilike`` because the performance will be better than if you use
   a regular expression that ignores the case (with ``?i``).

-  ``regexp_ilike``: Applied to two operands, a ``string``-type parameter
   and a `Java regular expression <https://docs.oracle.com/javase/8/docs/api/index.html?java/util/regex/Pattern.html>`_.
   It checks if the ``string``-type parameter matches the regular
   expression, ignoring case differences. You can achieve the same result
   with the operator ``regexp_like``, but the performance will be worse.

-  ``contains``: Accepts one ``string``-type element and one array of
   ``string``-type elements as operands. Returns ``true`` if the string
   provided as first operand contains all the substrings provided in the
   array on the second operand. Returns ``false`` otherwise.

   **Example**: Book.title contains [’java’,’xml’]

-  ``containsor``: Accepts one ``string``-type element and one array of
   ``string``-type elements as operands. Returns ``true`` if the string
   provided as first operand contains any of the substrings provided in the
   array on the second operand. Returns ``false`` otherwise.

-  ``in``: Applied to two operands. First operand is an element that can
   belong to the following data types: ``int``, ``long``, ``float``,
   ``double``, ``string``, ``date`` or ``url``. Second operand is an array
   of elements of the same type than the first operand. Returns ``true`` if
   the first operand is included in the array of elements provided on the
   second operand.

   **Example**: Book.isbn in [’ 9780782141313’,’ 9780470464892’]

-  ``between``: Applied to two operands. First operand is an element that
   can belong to the following data types: ``int``, ``long``, ``float``,
   ``double``, ``string``, ``date`` or ``url``. Second operand is an array
   of two elements of the same type than the first operand. Returns true if
   the operand on the left side is in the range specified by the two
   elements in the array, including the limit values.

   **Example**: Book.price between [10,20]
