=========================
Examples of Data Movement
=========================

Let us say that you have the following base views:

-  ``view_ds1`` over the JDBC data source ``ds1``
-  ``view_ds2`` over the JDBC data source ``ds2``
-  ``view_ds3`` over the JDBC data source ``ds3``

And these derived views:

.. code-block:: vql

   CREATE VIEW dataMoveView AS
       SELECT view_ds1.id
       FROM view_ds1
           INNER JOIN view_ds2
           INNER JOIN view_ds3
           ON (view_ds1.id = view_ds2.id)
       CONTEXT( DATAMOVEMENTPLAN = view_ds2: JDBC ds1 
                                   view_ds3: JDBC ds1 );

.. code-block:: vql
                         
   CREATE VIEW p_dataMoveView AS
       SELECT * 
       FROM dataMoveView
       CONTEXT( DATAMOVEMENTPLAN = view_ds2 : JDBC ds3 );

.. code-block:: vql

   CREATE VIEW p_dataMoveView_j_view_ds2 AS
       SELECT * 
       FROM p_dataMoveView a 
       INNER JOIN view_ds2 b ON (a.id = b.id);

-  ``dataMoveView`` sets that, at runtime, the data of the views
   ``view_ds2`` and ``view_ds3`` will be inserted in a table
   created in the data source ``ds1``.
-  ``p_dataMoveView`` overrides the data movement plan for the view
   ``view_ds2`` set in the view ``dataMoveView``. The data of the view
   ``view_ds2`` will be moved to the data source ``ds3``, instead of to
   ``ds1``.

.. note:: In these examples, we define the data movements with VQL, for
   clarity. However, we recommended defining the data movements of a view
   from the “Execution plan” tab of the “Options” dialog of the views.

**Query 1)** 

.. code-block:: sql

   SELECT * 
   FROM dataMoveView

Moves ``view_ds2`` and ``view_ds3`` to the data source ``ds1``.

|

**Query 2)** 

.. code-block:: sql

   SELECT * 
   FROM p_dataMoveView

Moves ``view_ds2`` to ``ds3`` and ``view_ds3`` to ``ds1``.

|

**Query 3)**

.. code-block:: vql

   SELECT * 
   FROM p_dataMoveView   
   CONTEXT( DATAMOVEMENTPLAN = view_ds2: OFF )

Moves ``view_ds3`` to ``ds1``. Although the view defines that
``view_ds2`` should be moved to ``ds3``, this movement is disabled in
the ``DATAMOVEMENTPLAN`` clause of the query with the ``OFF`` option.

|

**Query 4)**

.. code-block:: vql

   SELECT * 
   FROM p_dataMoveView_j_view_ds2    
   CONTEXT( DATAMOVEMENTPLAN = view_ds2: JDBC ds1 view_ds2: JDBC ds2 )

Moves ``view_ds2`` on the first branch to ``ds1`` and ``view_ds2`` on
the second, to ``ds2``. ``view_ds3`` is moved to ``ds1``. Note that the
data will be moved to the data sources set in the ``DATAMOVEMENTPLAN``
clause and not to the ones set in the definition of the view.

|

**Query 5)**

.. code-block:: vql

   SELECT * 
   FROM p_dataMoveView_j_view_ds2
   CONTEXT( DATAMOVEMENTPLAN = view_ds2: OFF view_ds2: JDBC ds2 )

Moves ``view_ds2`` on the second branch to ``ds2``, and ``view_ds3``
to ``ds1``. ``view_ds2`` on the first branch is not moved.

|

**Query 6)**

.. code-block:: vql

   SELECT * 
   FROM p_dataMoveView_j_view_ds2    
   CONTEXT( DATAMOVEMENTPLAN = view_ds2: JDBC ds2 )

This query fails because the definition of the data movement is
ambiguous. ``view_ds2`` is present in both branches of the join so the
data movement of the view ``view_ds2`` has to be defined twice.
