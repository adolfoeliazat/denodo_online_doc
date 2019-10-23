============================
Multiplicity of Associations
============================

The multiplicity of an association represents how many rows there are in
one view for each row of the other.

For example, let us say that you create an association between the view
``customer`` and the view ``order``, the multiplicity at the endpoint
``customer`` (role name ``orders``) is ``1`` and at the endpoint
``order`` (role name ``customer``) is ``*``. This indicates that for
each row of the ``customer`` view there are zero, one or more rows in
the ``order`` table; and for each row in the ``order`` view there is one
and only one row in the view ``customer``.

-  ``0..1`` indicates a multiplicity of 0 or 1.
-  ``1`` indicates a multiplicity of 1.
-  \* indicates a multiplicity of 0 or more.
-  ``+`` indicates a multiplicity of 1 or more.
