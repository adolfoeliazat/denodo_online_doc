====================
Creating a Base View
====================


.. toctree::
   :hidden:

   query_capabilities_search_methods_and_wrappers/query_capabilities_search_methods_and_wrappers.rst
   modifying_a_base_view/modifying_a_base_view.rst

The statement ``CREATE TABLE`` allows creating a base view. A base view
represents an external source (Web, relational database, etc.) that
supplies data to Virtual DataPort.

.. note:: We strongly recommend using the Administration Tool to create
   the base views graphically instead of manually writing VQL statements

Each base view is composed of a group of attributes. Each attribute has
to a *data type*.

When creating the base view, its name, internationalization
configuration and schema are specified.



.. code-block:: bnf
   :caption: Syntax of the statement CREATE TABLE
   :name: Syntax of the statement CREATE TABLE

   CREATE [ OR REPLACE ] TABLE <name:identifier> I18N <name:identifier> 
       ( <field> [, <field> ]* )
       [ FOLDER = <literal> ]
       [ DESCRIPTION = <literal> ]
       [ <primary_key> ] 
       [ CACHE { 
             OFF
           | PARTIAL [ EXACT ] [ PRELOAD ] 
           | FULL
           | INVALIDATE [ ON CASCADE ] 
             [ NOATOMIC [ INVALIDATEBLOCKSIZE <integer> ] ] 
             [ WHERE <condition> ] } ]
       [ BATCHSIZEINCACHE { <integer> | DEFAULT } ]
       [ TIMETOLIVEINCACHE { <seconds:integer> | DEFAULT | NOEXPIRE } ]
       [ SWAP { ON | OFF | DEFAULT } ]
       [ SWAPSIZE <megabytes:integer> ]
       [ MAXRESULTSIZE <megabytes:integer> ]
       [ <table search method clause> ]*
       [ <table index clause> ]*
       [ DELEGATESTATSQUERY = <boolean> ]
       [ { 
             SMART_ONLY 
           | SMART_THEN_ATSOURCE_THROUGH_VDP 
           | ATSOURCE_THROUGH_VDP_ONLY 
       } ]
   
   <field> ::=
       <name:identifier>:<type:identifier> [ ( <property list> ) ]
       
   <primary_key> ::= 
     [ CONSTRAINT <name:literal> ] 
     PRIMARY KEY ( <field_name:literal> 
     [, <field_name:literal>]* )
       
   <property list> ::=
     <name:identifier> [ = <value:identifier>] 
     [, <name_i:identifier> [ = <value_i:identifier>] ]* 
   
   <table index clause> ::=
       DECLARE { CACHE | VIEW } [ CLUSTER | HASH | OTHER ] INDEX <name:identifier> ON ( <table index field [ ,<table index field> ]* )
     | DELETE { CACHE | VIEW } INDEX <name:identifier>
       
   <table index field> :: = <field name:identifier> [ ASC | DESC ]
   
   <table search method clause> ::=
         ADD SEARCHMETHOD <name:identifier> (
           [ I18N <name:identifier> ]
           [ CONSTRAINTS ( [ <constraint clause> ]+ ) ]
           [ OUTPUTLIST ( <output clause> ) ] 
           [ WRAPPER ( <wrapper type> <wrapper_name:identifier> )
             ALTERNATIVE_WRAPPERS ( JDBC <wrapper_name:identifier> 
                                    [, JDBC <wrapper_name:identifier> ]* ) ]
          )

The modifier ``OR REPLACE`` specifies that, if there is a base view with
the name indicated, it will be replaced by the new view. If you replace
an existing view, the query capabilities of the derived views may change
(e.g. due to the addition of another field or a query restriction that
did not previously exist). This may affect the derived views of this
base view.

If the ``type`` of the field is ``blob``, you can indicate the content
type of the field. This can be either a constant (e.g.
``application/pdf``) or an expression. See more about this in the
section :ref:`Working with Blob Fields of Base Views` of the Administration
Guide.

The search methods of the view are defined with the ``ADD SEARCHMETHOD``
clause. The section :ref:`Query Capabilities: Search Methods and Wrappers`
explains what is a search method.

The ``PRIMARY KEY`` clause sets the definition of the primary key of the
view. See more information about primary keys in the section 
:doc:`/vdp/administration/restful_architecture/primary_keys_of_views/primary_keys_of_views` of the Administration Guide.

The clause ``ALTERNATIVE_WRAPPERS`` defines the alternative wrappers of
the base view. The section :ref:`Alternative Sources` of the Administration
Guide explains what these are.

The figure below is an example of the creation of a base view using the
statement ``CREATE TABLE``. It creates a JDBC wrapper and the base view
``book`` with three fields of type text: ``isbn``, ``title`` and
``author``.

.. code-block:: vql
   :caption: Example of creating a base view
   :name: Example of creating a base view
   
   CREATE WRAPPER JDBC book
       DATASOURCENAME=ds_jdbc_mysql
       CATALOGNAME = 'public' 
       RELATIONNAME = 'book' 
       OUTPUTSCHEMA (
           isbn = 'isbn' :'java.lang.String' (OPT) SORTABLE,
           title = 'title' :'java.lang.String' (OPT)  ESCAPE SORTABLE,
           author = 'author' :'java.lang.String' (OPT) SORTABLE
       );

   CREATE TABLE book I18N us_est (
           isbn:text, 
           title:text, 
           author:text ( DESCRIPTION = 'Author of the book. If there is more than one, the authors are separated by a comma') 
       )
       PRIMARY KEY ( 'isbn' )
       CACHE OFF
       TIMETOLIVEINCACHE DEFAULT
       ADD SEARCHMETHOD book(
           I18N us_est
           CONSTRAINTS (
                ADD isbn (any) OPT ANY
                ADD title (any) OPT ANY
                ADD author (any) OPT ANY
           )
           OUTPUTLIST (isbn, title, author)
           WRAPPER (jdbc book)
       );