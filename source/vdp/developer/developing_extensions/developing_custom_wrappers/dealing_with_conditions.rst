=======================
Dealing with Conditions
=======================

Conditions passed to a Custom wrapper as arguments for its run, delete
and update methods (see sections :ref:`Extending AbstractCustomWrapper` and
:ref:`Overriding AbstractCustomWrapper`) come encapsulated in instances of
``CustomWrapperConditionHolder``. The objects of this class contain two
versions of the conditions passed to the Custom wrapper:

-  A simplified version, available by calling the ``getConditionMap``
   method. This version consists on an association between
   ``CustomWrapperFieldExpressions`` and ``Objects``. For example, if we
   have a condition map like ``{ FIELD1 - 100; FIELD2 - 'foo' }``, it
   means that the condition passed to the CUSTOM wrapper is
   ``FIELD1 = 100 AND FIELD2 = 'foo'``. For a condition to be available
   as a map, it must match the pattern
   ``FIELD_1 = value [AND FIELD_N = value]*``, in other case the
   ``getConditionMap`` method will return null. Conversion to map can be
   forced by calling the ``getConditionMap(boolean force)`` method,
   passing true as the value for force. Take into account that in this
   case the returned map may not be equivalent to the original
   condition.
-  A ``CustomWrapperCondition`` instance. This version is available by
   calling the ``getComplexCondition`` method.


`CustomWrapperCondition <../../../javadoc/index.html?com/denodo/vdb/engine/customwrapper/condition/CustomWrapperCondition.html>`_ is the superclass of all the types of conditions that can be supported by a Custom wrapper:

-  ``CustomWrapperSimpleCondition`` represents a simple condition. It
   holds the left expression (a ``CustomWrapperExpression`` object), an
   operator and the right expression (an array of
   ``CustomWrapperExpression`` objects).
   The right expression is stored in array, which usually contains only
   one expression but may contain more for operators such as
   ``CONTAINSAND`` or ``CONTAINSOR`` that may have several parameters.
-  ``CustomWrapperAndCondition`` represents a series of conditions
   joined by the AND operator. It holds a list of
   ``CustomWrapperCondition`` objects.
-  ``CustomWrapperOrCondition`` represents a series of conditions joined
   by the OR operator. It holds a list of ``CustomWrapperCondition``
   objects.
-  ``CustomWrapperNotCondition`` represents a negated condition. It
   holds a ``CustomWrapperCondition``.

`CustomWrapperExpression <../../../javadoc/index.html?com/denodo/vdb/engine/customwrapper/expression/CustomWrapperExpression.html>`_ is the superclass of all the types of expressions supported by a Custom wrapper:

-  ``CustomWrapperFieldExpression``. This is the most common type of
   expression in a condition’s left side. See section :ref:`Overriding
   AbstractCustomWrapper` for details.
-  ``CustomWrapperSimpleExpression``. This kind of expression has a type
   (one of the types defined in ``java.sql.Types``) and a value.
-  ``CustomWrapperFunctionExpression``. Represents a function with
   parameters. This type of expression has a name, an optional modifier
   (``ALL`` or ``DISTINCT``), a list of parameters (instances of
   ``CustomWrapperFunctionParameter``) and the property of being an
   aggregation function or not. A ``CustomWrapperFunctionParameter``
   contains a list of ``CustomWrapperExpressions``.
-  ``CustomWrapperConditionExpression``. Represents a condition
   parameter in a ``CASE`` function. Contains a
   ``CustomWrapperCondition``.
-  ``CustomWrapperContainsExpression``. Represents an expression with
   the ``CONTAINS`` operator. Contains a literal and a search
   expression.
-  ``CustomWrapperArrayExpression``. Contains a list of
   ``CustomWrapperExpressions``, all of the same kind.
-  ``CustomWrapperRegisterExpression``. Contains a list of
   ``CustomWrapperExpressions`` of any kind.
