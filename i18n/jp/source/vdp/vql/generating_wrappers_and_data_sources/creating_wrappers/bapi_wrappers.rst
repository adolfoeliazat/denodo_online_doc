=============
BAPI Wrappers
=============

BAPI wrappers can connect to an SAP system, using a BAPI data source,
execute a BAPI and return its results.

`Syntax of the CREATE WRAPPER SAPERP statement (BAPI)`_ and `Syntax
of the ALTER WRAPPER SAPERP statement (BAPI)`_ contain the syntax of
the commands to create and modify BAPI wrappers.



.. code-block:: bnf
   :caption: Syntax of the CREATE WRAPPER SAPERP statement (BAPI)
   :name: Syntax of the CREATE WRAPPER SAPERP statement (BAPI)

   CREATE [ OR REPLACE ] WRAPPER SAPERP <name:identifier>
       [ FOLDER = <literal> ]
       DATASOURCENAME = <name:identifier>
       BAPINAME = <name:literal> 
       [ WITH_COMMIT = { TRUE | FALSE } ] // FALSE by default
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
                               [, <source configuration property> ]* ] ) ]
   
   <field> ::=
          <name:identifier> = <mapping:literal> [ VALUE <literal> ]
             [ ( { OBL | OPT } ) ]
             [ ( DEFAULTVALUE <literal> ) ]
             [ EXTERN ] 
             [ <inline constraints> ]*
        | <name:identifier> = <mapping:literal> : ARRAY OF ( <register field> )
             [ ( DEFAULTVALUE <literal> ) ]
             [ <inline constraints> ]*
        | <name:register field>
   
   <register field> ::=
       <name:identifier> = <mapping:literal> :
          REGISTER OF ( [ <field> [, <field> ]* ] )
             [ ( DEFAULTVALUE <literal> ) ]
             [ <inline constraints> ]*
   
   <inline constraint> ::=
        [ NOT ] NULL
      | [ NOT ] UPDATEABLE
      | { SORTABLE [ ASC | DESC ] | NOT SORTABLE }
   
   <source configuration property> ::=
         DATAINORDERFIELDSLIST = { DEFAULT | ( <name:identifier> { ASC | DESC }
                                         [, <name:identifier> { ASC | DESC } ]* ) }


.. code-block:: bnf
   :caption: Syntax of the ALTER WRAPPER SAPERP statement (BAPI)
   :name: Syntax of the ALTER WRAPPER SAPERP statement (BAPI)

   ALTER WRAPPER SAPERP <name:identifier>
       [ DATASOURCENAME = <name:identifier> ]
       [ BAPINAME = <name:literal> ]
       [ WITH_COMMIT = { TRUE | FALSE } ] // FALSE by default
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
       [, <source configuration property> ]* ] ) ]

.. 

   <field> ::= (see :ref:`Syntax of the CREATE WRAPPER SAPERP statement (BAPI)`)

   <source configuration property> ::= (see :ref:`Syntax of the CREATE WRAPPER SAPERP statement (BAPI)`)


If ``WITH_COMMIT`` is ``TRUE``, when this base view is queried, Virtual
DataPort will run the BAPI ``BAPI_TRANSACTION_COMMIT`` after running the
BAPI of this wrapper. Setting this clause to ``TRUE`` is equivalent to
selecting the check box “Commit after BAPI execution” of the “Create
base view from BAPI” dialog. The section :ref:`BAPI sources` of the
Administration Guide explains this option in more detail.

