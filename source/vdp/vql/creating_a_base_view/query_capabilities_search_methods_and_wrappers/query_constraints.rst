=================
Query Constraints
=================

To specify search methods, a series of 5-uples, which we will call
“query constraints”, must be specified. The following elements should be
indicated for each query constraint:

-  *Attribute -* is an attribute of the relation.

-  *Operators -* is the group of operators that can be used in the queries
   to this source and with this search method. ``ANY`` represents any
   operator admitted by the data type of the attribute. If the
   obligatoriness field (explained later) is ``NOS``, the value is not
   specified.

-  *Obligatoriness -* it can have one of these three values:

   -  ``OBL`` (obligatory): indicates that the attribute has to appear in
      any query on the source.
   -  ``OPT`` (optional) indicates that the attribute can appear or not in
      the query (it is optional).
   -  ``NOS`` (no searchable) indicates that the source does not allow
      querying by this attribute.

   .. important:: When you use ``OBL``, indicate a multiplicity of 1 or
      higher; not 0.

   When you use NOS, indicate multiplicity 0.

-  *Multiplicity* - indicates how many values the source can be queried
   simultaneously for the attribute and the given operator. The values
   ``ZERO`` (which is equivalent to “0”), ``ONE`` (which is equivalent to
   “1”), ``ANY`` and any integer number can be specified. If it is not
   possible to make queries for this attribute (value ``NOS`` in the
   *obligatoriness* field), the value is necessarily ``0`` or ``ZERO``.
   ``ANY`` indicates a number of values greater than ``0`` but without an
   upper limit.

-  *Possible Values* - is the list of values with which the attribute can
   be queried. If it contains the value ``ANY`` (or it is not specified),
   this means that it can be queried using any value (within the range
   associated with the data type of the attribute). If the *obligatoriness*
   field is set in the 5-uple to the value ``NOS``, then it necessarily
   takes the value of an empty set.


After specifying the query constraints, the attributes that appear in
the output of the queries made through the search method are indicated.
The output attributes of a search method are specified by enumerating
the attributes and separating them with commas.
