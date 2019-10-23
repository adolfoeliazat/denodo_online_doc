==================
Role Preconditions
==================

By setting a role precondition, ``SELECT_NAVIGATIONAL`` and the RESTful
Web service will not display a link to the other end of the association
for the rows that do not match the *Role precondition*.

You only can define a *Role precondition* on an end point when the other
end point has a multiplicity of \* or 0..1. This means that at the other
side of the end point there may not be an element that matches the
*Condition mapping*.

**Example**

Let us say that we have an association between the views ``employee``
and ``supportcase`` and we know that only the employees of the
department 50 (support) have support cases assigned to them.

When querying the view ``employee`` with ``SELECT_NAVIGATIONAL`` or the
RESTful Web service, the output will include the links of the
association for each row. However, only the links of employees of the
department 50 will have support cases associated to them.

Therefore, we can set a role precondition ``DEPT_NO = 50`` in the end
point of the view ``employee``.

By doing this, when the user queries the view ``employee``,
``SELECT_NAVIGATIONAL`` or the RESTful Web service will not display the
link of the association for the employees that do not belong to the
department ``50``.
