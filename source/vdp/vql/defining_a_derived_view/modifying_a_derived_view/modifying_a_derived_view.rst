========================
Modifying a Derived View
========================

Once a derived view has been created, it is possible to modify some of
its properties:

-  Its internationalization options through option ``i18n`` (see section :ref:`Internationalization`).
-  Cache configuration through the ``CACHE`` and ``TIMETOLIVEINCACHE``
   options (see section :ref:`Using the Cache`).
-  Virtual DataPort swapping policy configuration through the ``SWAP``,
   ``SWAPSIZE`` and ``MAXRESULTSIZE`` parameters (see section :ref:`Configuring Swapping Policies`).
-  Execution strategy configuration for the joins involved in defining
   the view through the ``QUERYPLAN`` option (see section :ref:`Dynamic
   Choice of Join Strategy`).
-  Rename the view: ``ALTER VIEW ... RENAME ...``
-  ``DECLARE CACHE INDEX`` defines a cache index. See more about this in
   the section :ref:`administration_guide_cache_indexes` of the Administration Guide.
-  With ``DATAMOVEMENTPLAN``, you can change the data movement plans
   defined for the view. If one of the subviews has several data
   movement plans and you want to leave some of them unchanged, leave
   the parenthesis empty.
   For example,
   
   .. code-block:: vql
   
      ALTER VIEW test
      DATAMOVEMENTPLAN = { internet_inc: () (JDBC ds1) }
      
-  Add or remove the primary key constraints of the base view
   (``<add key>`` and ``<drop key>`` definition). See more information
   about primary keys in the section :ref:`Primary Keys of Views` of the Administration Guide.
-  ``DELEGATESTATSQUERY``: if ``false``, the queries executed to gather
   the statistics of this view are not pushed down to the source.
   Instead, Virtual DataPort retrieves all the data (executes a
   ``SELECT * FROM table``) and executes the aggregation functions
   locally. If true or not present, the queries are pushed down to the
   source if possible.
   Setting this to false is equivalent to selecting the check box “Do
   not delegate the generation of the statistics” of the “Statistics”
   tab of the views’ “Advanced” wizard. You can find more information
   about this option in the section :ref:`Gathering the Statistics of Views`
   of the Administration Guide.
-  ``LAYOUT``: on the administration tool, when editing a derived view,
   the “Model” tab displays the subviews of the view. This parameter
   sets the position of these subviews in this tab. If this parameter is
   not set, the Tool positions each subview in a default location.

   For interface views, it sets the position of the implementation view on the “Implementation” tab.

   .. note:: Tis parameter does not affect in any way the behavior of the view.

The statement ``ALTER VIEW`` allows the Virtual DataPort administrator
to execute all these operations.

.. code-block:: bnf
   :caption: Syntax of the ALTER VIEW statement
   :name: Syntax of the ALTER VIEW statement

   ALTER VIEW <name:identifier>
       [ CACHE { 
             OFF 
           | PARTIAL [ EXACT ] [ PRELOAD ]
           | FULL
           | INVALIDATE [ ON CASCADE ]
               [ NOATOMIC [ INVALIDATEBLOCKSIZE <integer> ] ] 
               [ WHERE <condition> ] 
           } 
       ]
       [ BATCHSIZEINCACHE { <integer> | DEFAULT } ]
       [ TIMETOLIVEINCACHE { <seconds:integer> | DEFAULT | NOEXPIRE } ]
       [ SWAP { ON | OFF | DEFAULT } ]
       [ SWAPSIZE <megabytes:integer> ]
       [ MAXRESULTSIZE <megabytes:integer> ] 
       [ <table index clause> ]*
       [ DATAMOVEMENTPLAN = { <data movement plans> } ]
       [ QUERYPLAN = <query plan> ]
       [ 
       [ DESCRIPTION = <literal> ]
   | ALTER VIEW <name:identifier> RENAME <new_name:identifier>
   
   | ALTER VIEW <view:identifier>
       LAYOUT ( <subview1:identifier> = <bounds> 
           [, <subview:identifier> = <bounds> ]* )
   
   <table index clause> ::=
         DECLARE CACHE INDEX <name:identifier> 
             ON ( <table index field [ ,<table index field> ]* )
       | DROP CACHE INDEX <name:identifier>
   
   <table index field> :: = <field name:identifier> [ ASC | DESC ]
   
   <data movement plan> ::=
     [ <view name:identifier> : <data movement view plans> ]+

   <data movement view plans> ::=
         <data movement view plan>
       | [ ( [ <data movement view plan> ] ) ]+
   
   <data movement view plan> ::= 
     JDBC <data source name:identifier> | OFF
   
   <bounds> ::= 
     <x1:int>, <y1:int>, <x2:int>, <y2:int>
      
.. 

   <query plan> ::= (see :ref:`QUERYPLAN syntax`)