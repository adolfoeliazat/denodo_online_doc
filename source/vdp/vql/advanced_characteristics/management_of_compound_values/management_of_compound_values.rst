=============================
Management of Compound Values
=============================


.. toctree::
   :hidden:

   processing_of_compound_types_example.rst

Virtual DataPort has two classes of data types: simple types and
compound types. Compound types (``array`` and ``register``) represent
hierarchical data in the Virtual DataPort base relations and views.

.. note:: In Virtual DataPort, an ``array``-type element must be viewed
   as a subview. An ``array`` will always have a ``register`` type
   associated. Each subelement contained in the array will belong to this
   ``register`` data type. Hence, the fields of this register may be seen
   as the schema of the subview being modeled. It is important to bear this
   in mind when applying operators to subelements of a compound field.

Each attribute value of a view in the Server can be uniquely identified
within a tuple using an expression called URI. The URI associated with
the value of an attribute belonging to a simple type simply consists of
the name of the attribute. On the other hand, the value of a compound
attribute is represented using a tree, in which the leaves are atomic
values (i.e. belonging to simple data types). There are two types of
non-leaf nodes in these trees:

-  **Arrays (array type)**: from these, an arch runs to each of the nodes
   that represent the subelements that comprise the array (all belong to
   the same ``register`` data type). Each arch is tagged with the
   position index of the array subelement being indicated, written
   between the symbols “[” and “]”.
-  **Registers (register type)**: from these, an arch runs to each of the
   nodes that represent the subelements that comprise the register (each
   subelement is related to a field of the record that can belong to a
   different data type). Each arch is tagged with the name of the field.

Furthermore, an arch with the attribute name indicates the root of the
tree.

Given this tree, a URI that identifies a node of same is obtained
starting with the root and moving down the tree, concatenating
(separated by the character “.” except in the case of array indexes, in
which it will be indicated the index value between brackets) the names
of the different arches until arriving at the required node. Finally,
the name of the attribute is concatenated at the beginning of the
string. Furthermore, if in a URI for an array-type node no index is
specified, then the URI indicates the list of values of the array.

Therefore, we can distinguish two types of URIs:

1. Those that indicate a simple type or a register type value.

   This type of URIs can be used in the ``SELECT`` clause of the queries or
   as group-by attributes in a ``GROUP BY`` clause. If, in addition, a
   simple type value is pointed, then this URI can be used in the same
   manner as any other simple-type attribute in a query statement: in the
   clauses ``SELECT``, ``WHERE``, ``GROUP BY``, etc. It is also possible to
   use the ``ROW`` and ``{ }`` constructors (see section :ref:`Conditions with Compound Values`) to build compound values and use
   them in the right side of a condition. In this case, the operators ``=``
   and ``<>`` are the only ones allowed, and the data types of the URIs on
   the right and left side of the condition must be compatible (that is,
   their trees must be equal except for the arc names).


2. Those that indicate a list of values. These URIs correspond to ``array``
   values and therefore, can be seen as a subrelation, where each array
   element is a tuple and the schema of this tuple is defined by the
   ``register`` element fields associated with the ``array``.


   This type of URIs may appear in the following scenarios:

   a. In conditions of the ``WHERE`` clause, these URIs can appear on the
      left side of a condition. In this case, the conditions are evaluated,
      as if they were a condition on the subrelation modeled by the URI.
   b. In a Flatten view used in the ``FROM`` clause. See section :ref:`FLATTEN
      View (Flattening Data Structures)`.
   c. Aggregation functions (see section :ref:`Use of Aggregation Functions`)
      support this type of URIs.
   

