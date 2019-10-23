========
Examples
========

The following scenario provides an example of why it is important to
enable the uniqueness detection feature:

#. Uniqueness detection is enabled.


#. John works on the database ``DB1`` and Scott on the database ``DB2``.


   a. Both databases are linked to the same database in the VCS repository.


   b. Both databases are currently in the same state and contain the following
      elements:

      i.   A folder named ``F1``.
      ii.  A folder named ``F2``.
      iii. A data source named ``DS1`` located inside ``F1``.
      iv.  A view named ``V1`` also located inside ``F1``.


   c. All the elements are up-to-date and without local modifications.



#. John moves ``V1`` to ``F2`` and checks in the changes.


#. Scott modifies ``V1`` and checks in the changes.

   a. Virtual DataPort detects a uniqueness conflict between the different
      versions of ``V1``: one located in ``F1`` with local changes and the
      remote version which is currently located in ``F2``.
   b. Scott forces the check-in, so ``V1`` is now located in ``F1`` again
      and contains its own changes.

#. John modifies ``V1``, which in his database - ``DB1`` - still is in the
   folder ``F2``, and checks out the database.

   a. Virtual DataPort detects a uniqueness conflict between the different
      versions of ``V1``: one located in ``F2`` with local changes and the
      remote version which is currently located in ``F1``.
   b. John forces the check-out, so ``V1`` is now located in ``F1`` and
      contains the changes corresponding to Scott’s previous check-in (John
      agreed to lose his local changes after forcing the check-out).


.. note:: In step 3, Virtual DataPort detects a movement and requires
   John to confirm the check-in, even if this movement is safe. Currently,
   this is a known limitation of the uniqueness detection feature: all
   movements of elements require explicit confirmation when checking in the
   changes.
