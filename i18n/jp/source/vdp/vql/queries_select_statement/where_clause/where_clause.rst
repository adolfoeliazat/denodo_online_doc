============
WHERE Clause
============

The ``WHERE`` clause specifies the conditions the results of the query
should comply with. See :ref:`below <Syntax for a list of conditions>` the syntax for specifying conditions.


.. code-block:: bnf
   :caption: Syntax for a list of conditions
   :name: Syntax for a list of conditions

   <condition> ::=
       <condition> AND <condition>
     | <condition> OR <condition>
     | NOT <condition>
     | ( <condition> )
     | <value> <binary operator> <value> [ , <value> ]*
     | <value> <binary operator> ( <value> [ , <value> ]* )
     | <value> BETWEEN <value> AND <value>
     | <value> <unary operator>


A condition is a sequence of condition elements separated by the logical
operators ``AND``, ``OR`` or ``NOT``. At evaluation time, it obtains a
``boolean`` result. The conditions can be grouped between the symbols
``(`` and ``)`` to vary their priority.

A condition is comprised of three elements: a left-side operator which
will be the one to which the condition is applied, an operator and zero,
one or several right-side operands, depending on the operator used. The
comparison operators supported by Virtual DataPort are specified in
section :doc:`/vdp/vql/language_for_defining_and_processing_data_vql/comparison_operators/comparison_operators`; they include operators of equality,
greater/lesser comparison, string contention, etc.

A condition operand can be the name of an attribute, a constant, an
expression to be evaluated or a compound value (see the section :ref:`below <Conditions
with Compound Values>`).

Conditions with Compound Values
===============================

The ``ROW`` constructor creates ``register`` values (see section :ref:`Management of Compound Values` for more details about compound types).
For example:

.. code-block:: vql

   ROW (value1, ... ,valueN)

creates a register value with ``N`` fields. Each value can be an
attribute, a literal, a number, a logical value, an expression to
evaluate or a new ``ROW`` element.

To create ``array`` values, use the ``ROW`` construct combined with ``{``
and ``}``. For example:

.. code-block:: vql
   
   { ROW (value1, ... ,valueN), ROW (valueN+1, ... ,value2N) }

This creates an array value containing two ``register`` values. All the
registers of an array have to have the same type.

.. note:: See :ref:`Rules for forming functions` for a formal description of
   the syntax to create compound values.

Conditions with compound values can only be used with the equality
(``=``) and inequality (``<>``) operators. Both operands must have
compatible types for the comparison to be possible.

