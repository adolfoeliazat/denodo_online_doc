==============
JOIN Operation
==============

The Join operation combines records from two or more views. The
following construction must be used to do so:

FROM view1 JOIN view2 ON (joinCondition)

Where ``joinCondition`` specifies the required join condition. Usually,
this condition only includes comparisons between the fields of the views
involved in the ``JOIN``. But it can also include expressions with
functions, comparisons with literals, etc.

The following modifiers can be used on the ``JOIN`` clause:

-  ``INNER``: The join operation made will be of the *inner* type. The
   "inner joins" only include in the result the tuples built from the
   tuples of both relations associated according to the join conditions.
   This is the most common join type and is used by default. Examples:
   
   .. code-block:: sql
   
      FROM view1 JOIN view2 ON (joinCondition) 
      FROM view1 INNER JOIN view2 ON (joinCondition)
      
-  ``OUTER``: The join operation made will be of the *outer* type. There
   are three options for “outer” joins (one of them must always be
   used): ``FULL``, ``LEFT`` and ``RIGHT``. If the ``FULL`` option is
   used, the tuples of both relations will be included in the result,
   although they do not have an associated tuple in the other relation
   according to the join condition; the attributes of the other relation
   will be completed with ``NULL`` in the resulting tuple.
   
   If the ``LEFT`` option is used, only the tuples of the first relation
   that do not have associated tuples in the second are included. If the
   ``RIGHT`` option is used, only the tuples of the second relation that
   do not have associated tuples in the first are included. Examples:

   .. code-block:: sql

      FROM view1 FULL OUTER JOIN view2 ON (joinCondition) 
      FROM view1 LEFT OUTER JOIN view2 ON (joinCondition)
      FROM view1 RIGHT OUTER JOIN view2 ON (joinCondition)
   
   
-  ``NATURAL``: The natural join operation will be executed. Conditions
   will not be indicated in this type of join, as this will be done by
   associating the attributes with the same name in both input relations
   using the operator ``=``. This can be used with both “inner”
   and “outer” joins. Examples:

   .. code-block:: sql

      FROM view1 NATURAL JOIN view2 
      FROM view1 NATURAL LEFT OUTER JOIN view2 
   
-  ``CROSS``: The cross product of the specified views will be made.
   This is equivalent to listing the relations in the FROM clause
   without using JOIN. Example:

   .. code-block:: sql
   
      FROM view1 CROSS JOIN view2

Instead of specifying a join condition, it is also possible to use the
``USING`` clause to specify a list of attributes with the same name and
type in both relations. If any of the attributes specified does not
exist in some branch of the join tree, or types are not coincident in
both branches, an error will be raised. **Example**:

.. code-block:: none

   FROM view1 JOIN view2 USING (attribute1,…,attributeN)

Lastly, it is also possible to establish an execution strategy for a
specific join operation. See section :ref:`Dynamic Choice of Join Strategy`
for more details on this matter.

