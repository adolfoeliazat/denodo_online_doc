=============
ODBC Wrappers
=============

ODBC wrappers allow querying ODBC data sources.

The statement ``CREATE WRAPPER ODBC`` follows the same structure as that
defined for a JDBC wrapper. To create a wrapper of this type it is
necessary to specify the ODBC data source, the table or SQL statement
used to obtain the data from the source, and the output schema provided
by the wrapper. For more information, see section :ref:`JDBC Wrappers`.


.. code-block:: bnf
   :caption: Syntax of the CREATE WRAPPER ODBC statement
   :name: Syntax of the CREATE WRAPPER ODBC statement

   CREATE [ OR REPLACE ] WRAPPER ODBC <name:identifier>
       [ FOLDER = <literal> ]
       DATASOURCENAME = <name:identifier>
       { 
           [   SCHEMANAME = <name:literal> ] RELATIONNAME = <name:literal> 
             | SQLSENTENCE = <literal>
       }
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ ALIASES ( <alias> [, <alias> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
                               [, <source configuration property> ]* ] ) ]
   
   <field> ::=
         <name:identifier> [ = <mapping:literal> ] : <type:literal>
            [ ( { OBL | OPT } ) ] 
            [ ( <field property> = <literal> 
                [, <field property> = <literal> ]* ) ]
            [ <inline constraints> ]*
       | <name:identifier> [ = <mapping:literal> ] : 
            ARRAY OF ( <register field> )
            [ ( <field property> = <literal>
                [, <field property> = <literal> ]* ) ]
            [ <inline constraints> ]*
       | <name:register field>

   <field property> ::=
         DEFAULTVALUE
       | SOURCETYPENAME
       | SOURCETYPEID
       | SOURCETYPESIZE
       | SOURCETYPEDECIMALS
       | SOURCETYPERADIX
       
   <register field> ::=
       <name:identifier> [ = <mapping:literal> ] :
          REGISTER OF ( <field> [, <field> ]* )
            [ ( <field property> = <literal>
                [, <field property> = <literal> ]* ) ]
            [ <inline constraints> ]*
   
   <inline constraint> ::=
         [ NOT ] NULL
       | [ NOT ] UPDATEABLE
       | { SORTABLE [ ASC | DESC ] | NOT SORTABLE }
       | ESCAPE
       | EXTERN
       | IS_AUTOINCREMENT
   
   <alias> ::= 
     <literal> = <literal>
   
   <source configuration property> ::=
       ALLOWDELETE = { true | false | DEFAULT }
     | ALLOWINSERT = { true | false | DEFAULT }
     | ALLOWUPDATE = { true | false | DEFAULT }
     | DELEGATESQLSELECTION = { true | false | DEFAULT }
     | DATAINORDERFIELDSLIST = { ( <name:identifier> { ASC | DESC }
                             [, <name:identifier> { ASC | DESC } ]* ) | DEFAULT }
     | SUPPORTSDISTRIBUTEDTRANSACTIONS = { true | false | DEFAULT }
   
   <register field> ::=
       <name:identifier> [ = <mapping:literal> ] :
          REGISTER OF ( <field> [, <field> ]* )
            [ ( <field property> = <literal>
                [, <field property> = <literal> ]* ) ]
            [ <inline constraints> ]*
   
   <inline constraint> ::=
       [ NOT ] NULL
     | [ NOT ] UPDATEABLE
     | { SORTABLE [ ASC | DESC ] | NOT SORTABLE }
     | ESCAPE
     | EXTERN
     | IS_AUTOINCREMENT
   
   <alias> ::= 
     <literal> = <literal>
   
   <source configuration property> ::=
       ALLOWDELETE = { true | false | DEFAULT }
     | ALLOWINSERT = { true | false | DEFAULT }
     | ALLOWUPDATE = { true | false | DEFAULT }
     | DELEGATESQLSELECTION = { true | false | DEFAULT }
     | DATAINORDERFIELDSLIST = { ( <name:identifier> { ASC | DESC }
                             [, <name:identifier> { ASC | DESC } ]* ) | DEFAULT }
     | SUPPORTSDISTRIBUTEDTRANSACTIONS = { true | false | DEFAULT }

The wrapper creation statement accepts the ``OR REPLACE`` modifier.
Where specified, if there is already a wrapper with the same name, its
definition is replaced by the new one.

You can specify certain wrapper properties in the ``SOURCECONFIGURATION`` clause that 
determine the operations that can be made on the wrapper. In the figure above you can see what 
properties can be indicated and the section :ref:`Wrapper Configuration Properties` explains their meaning.


.. code-block:: bnf
   :caption: Syntax of the ALTER WRAPPER ODBC statement
   :name: Syntax of the ALTER WRAPPER ODBC statement

   ALTER WRAPPER ODBC <name:identifier>
       [ DATASOURCENAME = <name:identifier> ]
       [
             [ SCHEMANAME = <name:literal> ] RELATIONNAME = <name:literal>
           | SQLSENTENCE = <literal>
       ]
       [ OUTPUTSCHEMA ( <field> [, <field>]* ) ]
       [ ALIASES ( <alias> [, <alias>]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
       [, <source configuration property> ]* ]) ]

..

   <field> ::= (see :ref:`Syntax of the CREATE WRAPPER ODBC statement`)

   <alias> ::= (see :ref:`Syntax of the CREATE WRAPPER ODBC statement`)

   <source configuration property> ::= (see :ref:`Syntax of the CREATE WRAPPER ODBC statement`)

