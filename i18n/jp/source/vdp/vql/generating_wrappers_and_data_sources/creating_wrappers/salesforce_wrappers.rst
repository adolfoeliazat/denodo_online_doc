===================
Salesforce Wrappers
===================

This section describes the parameters of the statement CREATE WRAPPER SALESFORCE. We recommend creating Salesforce base views :ref:`from the administration tool <Salesforce Sources>` because it is much easier.

.. code-block:: bnf
   :caption: Syntax of the CREATE WRAPPER SALESFORCE statement
   :name: Syntax of the CREATE WRAPPER SALESFORCE statement

   CREATE [ OR REPLACE ] WRAPPER SALESFORCE <name:identifier>
       [ FOLDER = <literal> ]
       DATASOURCENAME = <name:identifier>
       { OBJECTNAME = <literal> | SOQLSENTENCE = <literal> }
       [ <outputschema> ]
       [ <sourceconfiguration> ]

   <outputschema> ::= OUTPUTSCHEMA ( <field> [, <field> ]* )

   <field> ::=
     <name:identifier> [ = <mapping:literal> ] [ : <type:literal>]
       [ ( { OBL | OPT } ) ]
       [ ( <field property> = <literal> [, <field property> = <literal> ]* ) ]
       [ <inline constraints> ]*
     | <name:identifier> [ = <mapping:literal> ] :
       ARRAY OF ( <register field> )
       [ ( DEFAULTVALUE = <literal> ) ]
       [ <inline constraints> ]*
     | <name:register field>

   <field property> ::=
       DEFAULTVALUE
     | SOURCETYPENAME

   <register field> ::=
     <name:identifier> [ = <mapping:literal> ] :
        REGISTER OF ( <field> [, <field> ]* )
        [ ( <field property> = <literal> [, <field property> = <literal> ]* ) ]
        [ <inline constraints> ]*

   <inline constraint> ::=
       [ NOT ] NULL
     | [ NOT ] UPDATEABLE
     | { SORTABLE [ ASC | DESC ] | NOT SORTABLE }
     | EXTERN
     | SQLFRAGMENT

   <sourceconfiguration> ::= SOURCECONFIGURATION ( [ <source configuration property>  [, <source configuration property> ]* ] )

   <source configuration property> ::=
     DELEGATEFETCH = <property value>
     | DELEGATEGROUPBY = <property value>
     | DELEGATEOFFSET = <property value>
     | DELEGATEAGGREGATEFUNCTIONSLIST = { DEFAULT | ( <function:identifier> [, <function:identifier> ]* ] ) }
     | DELEGATEOPERATORSLIST = { DEFAULT | ( <operator:identifier> [, <operator:identifier> ]* ] ) }
     | SUPPORTSAGGREGATEFUNCTIONSOPTIONS = <property value>

   <property value> ::= true | false  | DEFAULT

To modify an existing Salesforce data source, use the statement ALTER WRAPPER SALESFORCE.

.. code-block:: bnf
   :caption: Syntax of the ALTER WRAPPER SALESFORCE statement
   :name: Syntax of the ALTER WRAPPER SALESFORCE statement

   ALTER WRAPPER SALESFORCE <name:identifier>
     [ DATASOURCENAME = <name:identifier> ]
     [ { OBJECTNAME = <literal> | SOQLSENTENCE = <literal> } ]
     [ <outputschema> ]
     [ <sourceconfiguration> ]

   <outputschema> ::= (see CREATE WRAPPER SALESFORCE for details)

   <sourceconfiguration> ::= (see CREATE WRAPPER SALESFORCE for details)

To define a Salesforce *wrapper*, you have to indicate the name of the Salesforce *data source* (``DATASOURCENAME``). There are two ways to indicate where the wrapper has to retrieve the data from:

#. If the data comes from a Salesforce object, specify the name (``OBJECTNAME``).
#. Alternatively, you can specify a SOQL statement (``SOQLSENTENCE``). The SOQL statement can contain an interpolation string (see section :ref:`Execution Context of a Query and Interpolation Strings`).

The ``OUTPUTSCHEMA`` clause defines the schema of the data that the wrapper will provide (see section :ref:`Wrapper Metadata`). For each simple-type element the type must be specified. Furthermore, an association may be indicated between the name of the field returned by the wrapper and the name of the field in Salesforce (as specified in the mapping).

The field property ``SOURCETYPENAME`` is the name of the type in Salesforce.

If a field has the *inline constraint* ``SQLFRAGMENT`` and a query uses this field in its ``WHERE`` clause, the value given to the field is delegated to the database “as is”, without parsing the condition.

.. note:: The clause ``SQLFRAGMENT`` is deprecated and may be removed in
   future versions of the Denodo Platform.

The wrapper creation statement also accepts the ``OR REPLACE`` modifier. Where specified, if there is already a wrapper with the same name, its definition is replaced by the new one.

Lastly, certain wrapper properties can be specified (``SOURCECONFIGURATION``). Virtual DataPort will take them into account to determine the operations that can be made on the wrapper. The applicable properties are indicated in the corresponding statement declaration (:ref:`Syntax of the CREATE WRAPPER SALESFORCE statement`).

Using WHEREEXPRESSION in SOQL Queries
=====================================

Virtual DataPort provides a predefined interpolation variable called ``WHEREEXPRESSION`` that simplifies the creation of base views that when queried, instead of querying a Salesforce object, execute a specific SOQL statement (``CREATE WRAPPER SALESFORCE`` with the parameter ``SOQLSENTENCE``). At runtime, the Server will replace ``WHEREEXPRESSION`` with the condition sent to the wrapper of the base view.
