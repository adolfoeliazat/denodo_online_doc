=====================
Modifying a Base View
=====================

By using the statement ``ALTER TABLE`` it is possible to configure the
following properties of a base view:

-  Its internationalization configuration.

-  Its cache configuration (``CACHE``). If ``PARTIAL`` or ``FULL``, the
   tuples extracted from the source as a result of executing the queries
   are stored in the local cache. The section :ref:`Using the Cache` explains
   how each cache mode works.

-  ``BATCHSIZEINCACHE``. The rows obtained from the source are inserted in
   cache in batches. This parameter determines the number of rows per
   batch. The value ``DEFAULT`` is the Batch size defined for the database
   in the “Database Management” dialog (menu “Administration”). If the
   database does not have a specific Batch size, the cache module uses the
   value defined in the “Cache” dialog (“Administration > Server
   configuration” menu).

-  Its memory usage configuration (``SWAP``, ``SWAPSIZE`` and
   ``MAXRESULTSIZE``). That is, the “swapping to disk” policy for queries
   that use the base view and involve a large number of tuples. See section :ref:`Configuring Swapping Policies` for more details.

-  ``DECLARE CACHE INDEX`` defines a cache index. See more about this in
   the section :ref:`administration_guide_cache_indexes` of the Administration Guide.

-  ``DECLARE VIEW INDEX`` defines a view index. See more about this in the
   section :doc:`/vdp/administration/creating_derived_views/advanced_configuration_of_views/indexes_of_views` of the Administration Guide.

-  Add, delete or modify a search method. Search methods are composed of
   rules that represent the restrictions with which a specific query should
   comply in order to be executed using this search method. Furthermore,
   each search method has an associated *wrapper* which contains the data
   necessary to translate the user query for the source and interpret its
   response. The section :ref:`Assigning Wrappers to Search Methods` provides
   more details on this matter.

-  ``DELEGATESTATSQUERY``: if ``false``, the queries executed to gather the
   statistics of this base view are not pushed down to the source. Instead,
   Virtual DataPort retrieves all the data (executes a
   ``SELECT * FROM table``) and executes the aggregation functions locally.
   If ``true`` or not present, the queries are pushed down to the source if
   possible.

   Setting this to ``false`` is equivalent to selecting the check box “Do
   not delegate the generation of the statistics” of the “Statistics” tab
   of the views” “Advanced” wizard.
   
   Find more information about this option in the section 
   :doc:`/vdp/administration/optimizing_queries/cost-based_optimization/gathering_the_statistics_of_views` of the Administration Guide.

-  The clauses ``SMART_ONLY``, ``SMART_THEN_ATSOURCE_THROUGH_VDP`` and
   ``ATSOURCE_THROUGH_VDP_ONLY`` control what the Server will do to obtain
   the statistics of this view:

   -  ``SMART_ONLY``: the Server will obtain the statistics of the view
      from the system tables of the database. The information that can be
      obtained from these tables depends on the database to which the data
      source connects, so not all the statistics may be obtained.
   
      Adding this clause is equivalent to clearing the check box “Complete
      missing statistics…” of the “Statistics” tab.

   -  ``SMART_THEN_ATSOURCE_THROUGH_VDP``: the Server will obtain the
      statistics from the system tables of the database. If a statistic
      cannot be obtained from the catalog, a regular ``SELECT`` query will
      be executed to obtain it.
    
      Adding this clause is equivalent to selecting the check box “Complete
      missing statistics…” of the “Statistics” tab.
   
   -  ``ATSOURCE_THROUGH_VDP_ONLY``: the Server will not try to obtain any
      statistic from the system tables of the database. Instead, the Server
      will execute a regular ``SELECT`` query to obtain these statistics.
    
      This option is not available from the Administration Tool. It can
      only be set via VQL.

   Find more information about this option in the section :doc:`/vdp/administration/optimizing_queries/cost-based_optimization/gathering_the_statistics_of_views` of the Administration Guide.

-  Rename the view: ``ALTER TABLE <name> RENAME...``

-  Add or remove the primary key constraints of the base view
   (``<add key>`` and ``<drop key>`` definitions). See more information
   about primary keys in the section 
   :doc:`/vdp/administration/restful_architecture/primary_keys_of_views/primary_keys_of_views` of the Administration Guide.

-  Modify the columns of the base view:

   -  To rename a column, use
      ``ALTER COLUMN <current field name:identifier>  RENAME <new field name:identifier>``
      
      For example:
   
      .. code-block:: vql
      
         ALTER TABLE internet_inc (
             ALTER COLUMN iinc_id RENAME incidence_id
         );

   -  To modify the type of a column, use
      ``ALTER COLUMN <name:identifier> MODIFY <new type:identifier> <is field nullable>``.
    
      When you change the type of a field, the current source type
      properties of the field get lost. If you need them, you have to add
      them again.
    
      For example:

      .. code-block:: vql
      
         ALTER TABLE internet_inc (
             ALTER COLUMN incidence_id MODIFY decimal FALSE
         );

   -  To add or modify a source type property of a field, use
      ``ALTER COLUMN <field name:identifier> ADD ( <source type    property:identifier> = <source type value:literal> )``
    
      If the property “source type property” already exists, the value
      provided replaces the existing one.
    
      For example:
      
      .. code-block:: vql
      
         ALTER TABLE internet_inc (
             ALTER COLUMN summary ADD ( sourcetypesize = '1000')
         );
      
   -  To add or modify the description of a field, use
   
      .. code-block:: vql
      
         ALTER COLUMN <field name:identifier> ADD ( DESCRIPTION = <description:literal> )
    
      For example:
      
      .. code-block:: vql
      
         ALTER TABLE internet_inc (
             ALTER COLUMN iinc_id ADD ( DESCRIPTION = 'Identifier of the incident' )
         );


.. code-block:: bnf
   :caption: Syntax of the statement ALTER TABLE
   :name: Syntax of the statement ALTER TABLE

   ALTER TABLE <name:identifier>
     [ I18N <name:identifier> ]
     [ CACHE { 
         OFF 
       | PARTIAL [ EXACT ] [ PRELOAD ]
       | FULL [ INCREMENTAL <incremental_condition:literal> ] 
       | INVALIDATE [ ON CASCADE ] 
         [ NOATOMIC [ INVALIDATEBLOCKSIZE <integer> ] ] 
         [ WHERE <condition> ] 
       } 
     ]
     [ BATCHSIZEINCACHE { <integer> | DEFAULT } ]
     [ TIMETOLIVEINCACHE { <seconds:integer> | DEFAULT | NOEXPIRE } ]
     [ SWAP { ON | OFF | DEFAULT} ]
     [ SWAPSIZE <megabytes:integer> ]
     [ MAXRESULTSIZE <megabytes:integer> ] 
     [ <table index clause> ]*
     [ <table search method clause> ]*
     [ { <add key> | <drop key> }]
     [ DELEGATESTATSQUERY = <boolean> ] 
     [ { 
             SMART_ONLY 
           | SMART_THEN_ATSOURCE_THROUGH_VDP 
           | ATSOURCE_THROUGH_VDP_ONLY 
       } ]
     [ DESCRIPTION = <literal> ] 
   | ALTER TABLE <name:identifier> ( <alter column clause>+ ) 
   | ALTER TABLE <name:identifier> RENAME <new_name:identifier> 
   
   <table index clause> ::=
       DECLARE { CACHE | VIEW } [ CLUSTER | HASH | OTHER ] 
       INDEX <name:identifier> 
       ON ( <table index field [ ,<table index field> ]* )
     | DROP { CACHE | VIEW } INDEX <name:identifier> 
   
   <table index field> :: = <field name:identifier> [ ASC | DESC ]
   
   <table search method clause> ::=
       ADD SEARCHMETHOD <name:identifier> (
           [ I18N <name:identifier> ]
           [ CONSTRAINTS ( [ <constraint clause> ]+ ) ]
           [ OUTPUTLIST ( <output clause> ) ]
           [ <wrapper clause> ]
          )
     | ALTER SEARCHMETHOD <name:identifier> (
           [ I18N { <name:identifier> | DEFAULT } ]
           [ CONSTRAINTS ( [ <constraint clause> ]+ ) ]
           [ OUTPUTLIST ( <output clause> ) ]
           [ <wrapper clause> ]
          )
     | DROP SEARCHMETHOD <name:identifier>
   
   <alter column clause> ::=
       {   ALTER COLUMN <name:identifier> RENAME <new name:identifier>
         | ALTER COLUMN <name:identifier> MODIFY <new type:identifier>
               [ <nullable clause> ]
         | ALTER COLUMN <field name:identifier> ADD ( 
               <source type property:identifier> = <source type value:literal> 
           )
         | ALTER COLUMN <field name:identifier> ADD (
               DESCRIPTION = <description:literal> )
       }
   
   <nullable clause> ::= { TRUE | FALSE }
   
   <constraint clause> ::=
       ADD <field> ( [ <operator> [, <operator> ]* ] ) 
           { 
              <obligatoriness> <multiplicity> 
                               [ ( <value_1:value> [ , <value_i:value> ]* ) ]
            | 
              NOS { ZERO | 0 } () 
           }
     | DROP <integer>
   
   <output clause> ::= 
     <field> [ ,<field> ]*
   
   <wrapper clause> ::=
       WRAPPER ( <wrapper type> <name:identifier> ) 
       ALTERNATIVE_WRAPPERS ( JDBC <wrapper_name:identifier> [, JDBC <wrapper_name:identifier> ]* ) ]
     | DROP WRAPPER
   
   <wrapper type> ::= 
       ARN 
     | CUSTOM
     | DF 
     | ESSBASE 
     | GS 
     | ITP 
     | JDBC 
     | JSON
     | LDAP 
     | ODBC 
     | SALESFORCE
     | SAPBWBAPI 
     | SAPERP 
     | WS 
     | XML
     
   <field> ::= 
     <identifier>[.<identifier>]*
   
   <obligatoriness> ::= 
       OPT 
     | OBL
   
   <multiplicity> ::= 
       ZERO 
     | ONE 
     | ANY 
     | <integer>

   <operator> includes “any” to represent any operator.
   
   <add key> ::= 
     ADD [ CONSTRAINT <name:literal> ] 
     PRIMARY KEY ( <field_name:literal> [, <field_name:literal>]*)
                 
   <drop key> ::= 
       DROP CONSTRAINT <name:literal>
     | DROP PRIMARY KEY
     
..

   <condition clause> ::= (see :ref:`Rules for forming functions`)
   <query plan> ::= (see :ref:`QUERYPLAN syntax`)
