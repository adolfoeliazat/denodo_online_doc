==================
Three-valued Logic
==================

Starting with Denodo 5.5, Virtual DataPort is fully conformant with the
three-valued logic defined in the SQL standard. Previous versions of the
Denodo Platform are not conformant with this standard. This change may
lead to queries returning different results.

The three-valued logic defines that the data type boolean comprises the
values *True*, *False* and *Unknown*, which is represented as the *NULL*
value. Any comparison involving the *null* value or an *Unknown* truth
will return an *Unknown* result.

The following example explains how this change can lead to different
results. Let us say that we execute a query like this one:

.. code-block:: sql

   SELECT ... WHERE NOT (a = b)
     
In one of the rows of the result, ``a`` is ``NULL`` and ``b`` is ``1``.

In previous versions of Virtual DataPort, ``(a = b)`` evaluates to
``false`` and ``NOT(a = b)`` to ``true``. Therefore, this row is added
to the result.

Virtual DataPort, as stated by the SQL standard, evaluates ``(a = b)``
to *Unknown* because ``a`` is ``NULL``. The result of evaluating NOT
(*Unknown*) is also *Unknown*. Therefore, this row is not added to the
result.
