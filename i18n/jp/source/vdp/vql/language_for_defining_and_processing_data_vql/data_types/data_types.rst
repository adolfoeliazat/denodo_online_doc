==========
Data Types
==========


.. toctree::
   :hidden:

   data_types_for_dates_timestamps_and_intervals.rst
   internationalization.rst

The Virtual DataPort catalog includes a group of predefined data types.
These types can be divided into two groups: basic types and compound
types.



.. code-block:: bnf
   :caption: Virtual DataPort data types
   :name: Virtual DataPort data types

   <Virtual DataPort data type> ::=
         blob
       | boolean
       | date
       | decimal
       | double
       | float
       | int
       | intervaldaysecond
       | intervalyearmonth
       | localdate
       | long
       | text
       | time
       | timestamp
       | timestamptz
       | xml
       | <Virtual DataPort compound data type>

   <Virtual DataPort compound data type> ::=
         array
       | register

The basic data types supported are:

-  ``int``. Integer number. The maximum value is +2^31-1
   (2,147,483,647) and the minimum, -2^31 (-2,147,483,648).
-  ``long``. Long integer number. The maximum value is
   2 :superscript:`63` - 1 (9,223,372,036,854,775,807) and the minimum, -2 :superscript:`63`
   (-9,223,372,036,854,775,808).

-  ``float``. Single-precision 32-bit IEEE 754 floating
   point. Its range of values is explained in the section `Floating-Point Types,
   Formats, and Values <https://docs.oracle.com/javase/specs/jls/se7/html/jls-4.html#jls-4.2.3>`_ of the Java Language Specification.
-  ``double``. Double-precision 64-bit IEEE 754 floating
   point. Its range of values is explained in the section `Floating-Point Types,
   Formats, and Values <https://docs.oracle.com/javase/specs/jls/se7/html/jls-4.html#jls-4.2.3>`_ of the Java Language Specification.
-  ``decimal``. Signed decimal number with
   arbitrary-precision.
-  ``boolean``. Logical value: true or false or unknown (null).
-  ``text``. Character string.
-  ``date`` (deprecated). Timestamp with a time zone displacement. Maintained for compatibility reasons.
-  ``localdate``. Date without a time zone (year, month and day)
-  ``time``. Time without a time zone (hour, minute, second and millisecond).
-  ``timestamp``. Timestamp without a time zone (year, month, day, hour, minute, second and millisecond).
-  ``timestamptz``. Timestamp with a time zone displacement.
-  ``intervaldaysecond``. Duration of a period of time with a precision that can include any set of contiguous fields other than YEAR or MONTH.
-  ``intervalyearmonth``. Duration of a period of time with a precision that includes a YEAR field or a MONTH field, or both.
-  ``blob``. Binary value. These values cannot take part in query conditions.
-  ``xml``. XML document or XML fragment.
   
.. rubric:: Datetime Types
   
The next section (:doc:`data_types_for_dates_timestamps_and_intervals`) explains the datetime types in detail.

.. rubric:: Compound Types

In Virtual DataPort you can define compound data types to model
hierarchical data such as the data obtained from SOAP Web services or
XML documents. The section :ref:`Defining a Data Type` explains how to
define compound types.

The compound data types are:

-  ``register``. Compound data with an
   internal and heterogeneous structure, i.e. the fields into which the
   data are subdivided are not all the same type.
-  ``array``. List of elements of the same ``register`` type.

.. rubric:: Numeric Types

.. todo Rewrite the following paragraph once #44102 (multiplication returns incorrect value in some cases) is completed saying something "before update XXX..."

The numeric type ``int`` does not have overflow protection. Therefore, an arithmetic operation involving two ``int`` values will return an incorrect value when the result of the operation exceeds the range of the data type. The range of this data type goes from -2,147,483,648 to 2,147,483,647. When applying an arithmetic operation over an ``int`` value, consider converting the input values of the operation to a ``long`` or ``decimal`` when the result of the arithmetic operation may exceed its range.

For example, very probably the following query will return an invalid value:

.. code-block:: vql

   SELECT amount_in_millions * 1000000 AS amount

To avoid this issue, convert the column ``amount_in_millions`` to ``long``:

.. code-block:: vql

   SELECT cast('long', amount_in_millions) * 1000000 AS amount

Note that the cast has to be applied over the input value of the operation (``amount_in_millions``). Casting the result of the multiplication will not solve the problem. 

The type ``decimal`` is not affected by this because it has arbitrary precision (i.e. there is no limit on its precision).

The data types ``long``, ``float`` and ``double`` do not have an overflow protection either. However, their range is much higher so it is very difficult to exceed their range.