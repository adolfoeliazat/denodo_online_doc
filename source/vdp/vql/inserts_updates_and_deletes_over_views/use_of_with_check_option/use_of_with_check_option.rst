========================
Use of WITH CHECK OPTION
========================

When creating a view, you can use the SQL standard clause
``WITH CHECK OPTION [ CASCADE ]``. If a view has been created with this
option, the data updates that are inconsistent with the definition of
the view will be rejected. For example, if the ``incidents_acme`` view
is defined using the following statement:

.. code-block:: sql

   CREATE VIEW incidents_acme AS 
      SELECT * FROM Internet_inc WHERE taxid = 'B78596011'
   WITH CHECK OPTION

The following ``INSERT`` statement will fail:

.. code-block:: sql

   INSERT INTO incidents_acme (
   iinc_id
   , summary
   , ttime
   , taxid
   , specific_field1
   , specific_field2)
   VALUES (
   6
   , 'Error in ADSL Router'
   , '31-mar-2005 22h 35m 24s'
   , 'B78596015'
   , '5'
   , '6')

This statement fails because the value of the ``taxid`` field does not
meet the ``WHERE`` condition of the ``incidents_acme`` view.

If the view is created with the ``WITH CHECK OPTION CASCADE`` clause,
the Server checks that the inserted values meet the ``WHERE`` condition
of the view and also, the ``WHERE`` conditions of the subviews.

If the “Automatic simplification of queries” is enabled, when executing
an ``INSERT``/``UPDATE``/``DELETE`` query over a derived view, the
Server assumes that this view was created with the option
``WITH CHECK OPTION``. As a result, the Server checks that the data
inserted/updated/deleted meets the ``WHERE`` condition of the definition
of the view.
