=====================================
Referential Integrity in Associations
=====================================

An association may represent a referential constraint (foreign key),
which means that every row of the *Dependent* view has a match in the
*Principal* view following the *Condition mapping*.

As with primary keys, Virtual DataPort does not enforce referential
integrity of the data. That is, that Virtual DataPort does not enforce
that each row of the dependent view has a match in the principal view of
the association. It is the responsibility of the source to keep this
integrity and of the user to define the associations correctly. Failing
to do so correctly, could even lead to getting the wrong results in some
queries.

To mark an association as a referential constraint, select the
**Referential constraint** check box in the “View Association” dialog
and then, select the **Principal** view of the association.

The JDBC and ODBC interfaces of Virtual DataPort publish the
associations marked as referential constraints, as foreign keys of the
dependent view of the association.
