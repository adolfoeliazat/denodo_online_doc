===========
DF Wrappers
===========

Virtual DataPort supports the creation of wrappers for CSV-delimited
files and other flat text files with data that can be extracted by
applying regular expressions.

The following figure shows the creation syntax of a wrapper of delimited
files.



.. code-block:: bnf
   :caption: Syntax of the CREATE WRAPPER DF statement
   :name: Syntax of the CREATE WRAPPER DF statement

   CREATE [ OR REPLACE ] WRAPPER DF <name:identifier>
       [ FOLDER = <literal> ]
       DATASOURCENAME = <name:identifier>
       [ TUPLEROOT <XML node or path:literal> ]
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
                               [, <source configuration property> ]* ] ) ]
   
   <field> ::=
         <name:identifier> [ = <mapping:literal> ] [ : <type:literal> ]
           [ ( { OBL | OPT } ) ]
           [ ( DEFAULTVALUE <literal> ) ]
           [ EXTERN ]
           [ <inline constraints> ]*
       | <name:identifier> [ = <mapping:literal> ] : 
           ARRAY OF ( <register field> )
           [ ( DEFAULTVALUE <literal> ) ]
           [ <inline constraints> ]*
       | <name:register field>
   
   <register field> ::=
       <name:identifier> [ = <mapping:literal> ] :
           REGISTER OF ( <field> [, <field> ]* )
             [ ( DEFAULTVALUE <literal> ) ]
             [ <inline constraints> ]*
   
   <inline constraint> ::=
         [ NOT ] NULL
       | [ NOT ] UPDATEABLE
       | { SORTABLE [ ASC | DESC ] | NOT SORTABLE }
       | NULLVALUE <literal>
       | URIPARAM
   <source configuration property> ::=
       DATAINORDERFIELDSLIST = { DEFAULT | ( <name:identifier> { ASC | DESC }
                                       [, <name:identifier> { ASC | DESC } ]* ) }

-  If you execute a ``CREATE WRAPPER DF xxx`` statement and there is
   another DF wrapper with the same name, the creation fails. If you add
   the ``OR REPLACE`` modifier, the current definition is replaced by
   the new one.
-  Provide the name of the data source: ``DATASOURCENAME``.
-  Optionally, as with the other wrappers, you can specify the schema of
   the data returned by the wrapper: ``OUTPUTSCHEMA``.
-  The clause ``URIPARAM`` is always used along with the clause
   ``EXTERN``, which indicates that this field represents the value of
   an interpolation variable. By adding ``URIPARAM``, the value of the
   field will be encoded as a query parameter of a URL. ``URIPARAM``
   should only be added when the wrapper is created over DF data sources
   whose path is HTTP and the variable is the value of a URL’s query
   parameter. See more details about this in the section :ref:`HTTP Path`
   (subsection of :ref:`Path Types in Virtual DataPort`) of the
   Administration Guide.
-  By default, no value obtained by a DF wrapper is ``NULL``. If you
   want to consider a certain string as ``NULL``, add to the definition
   of the field, the modifier ``NULLVALUE`` along with the string you
   want to consider ``NULL``.
   For example, let us say that we define the field field1 like this:
   ``field1 = 'field1' (OPT) NULLVALUE ''``.
   
   At runtime, when a value of field1 is an empty string, the wrapper
   changes this value to ``NULL``.
-  Lastly, certain wrapper properties can be specified
   (``SOURCECONFIGURATION``). Virtual DataPort will take them into account to
   determine the operations that can be made on the wrapper. The
   applicable properties are indicated in the corresponding statement
   declaration (:ref:`Syntax of the CREATE WRAPPER DF statement`), and
   are explained in the section :ref:`Wrapper Configuration Properties`.

.. note:: In this type of wrappers, registers or arrays are not
   supported as elements of the output schema.

The syntax of the modification statement of a delimited file wrapper is
similar.



.. code-block:: bnf
   :caption: Syntax of the ALTER WRAPPER DF statement
   :name: Syntax of the ALTER WRAPPER DF statement

   ALTER WRAPPER DF <name:identifier>
       [ DATASOURCENAME = <name:identifier> ]
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
       [, <source configuration property> ]* ] ) ]

.. 

   <field> ::= (see :ref:`Syntax of the CREATE WRAPPER DF statement`)

   <source configuration property> ::= (see :ref:`Syntax of the CREATE WRAPPER DF statement`)

