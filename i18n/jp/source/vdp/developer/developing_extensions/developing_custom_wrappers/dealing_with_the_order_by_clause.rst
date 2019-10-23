================================
Dealing with the ORDER BY Clause
================================

If the custom wrapper declares in the ``CustomWrapperConfiguration``
that it supports ``ORDER BY`` delegations (see section :ref:`Configuring a
Custom Wrapper`), the developer can invoke the method
``getOrderByExpressions()`` to obtain the expressions in the
``ORDER BY`` clause that is delegated to the custom wrapper. This method
returns a list of ``CustomWrapperOrderByExpression`` objects that have
the following attributes:

-  ``field``: the field that the rows should be sorted by.
-  ``order``: the order (ASC or DESC) of the sorting.
