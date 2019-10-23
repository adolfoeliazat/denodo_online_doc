=============
JSON Wrappers
=============

This section explains the syntax used to create JSON wrappers over JSON
data sources.



.. code-block:: bnf
   :caption: Syntax of the CREATE WRAPPER JSON statement
   :name: Syntax of the CREATE WRAPPER JSON statement

   CREATE [ OR REPLACE ] WRAPPER JSON <name:identifier>
       [ FOLDER = <literal> ]
       DATASOURCENAME = <name:identifier>
       [ TUPLEROOT <JSON path:literal> ]
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
                               [, <source configuration property> ]* ] ) ]
   
   <field> ::=
         <name:identifier> [ = <mapping:literal> ] [ : <type:literal>]
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
       | URIPARAM
   
   <source configuration property> ::=
       DATAINORDERFIELDSLIST = { DEFAULT | ( <name:identifier> { ASC | DESC }
                                         [, <name:identifier> { ASC | DESC } ]* ) }

The syntax of the modification statement of a JSON wrapper is similar.



.. code-block:: bnf
   :caption: Syntax of the ALTER WRAPPER JSON statement
   :name: Syntax of the ALTER WRAPPER JSON statement

   ALTER WRAPPER JSON <name:identifier>
       [ DATASOURCENAME = <name:identifier> ]
       [ TUPLEROOT <JSON path:literal> ]
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
       [, <source configuration property> ]* ] ) ]

..

   <field> ::= (see :ref:`Syntax of the CREATE WRAPPER JSON statement`)

   <source configuration property> ::= (see :ref:`Syntax of the CREATE WRAPPER JSON statement`)


Virtual DataPort supports the creation of wrappers on documents in JSON
format. To create a wrapper of this type the name of the data source
must be indicated (``DATASOURCENAME`` parameter).

The JSON wrapper analyzes the structure of the document and returns the
JSON tags of the first level as attributes (using their name as the
attribute name), encapsulating the other elements in compound types. As
with the other wrappers, the schema returned by the wrapper may be
specified (``OUTPUTSCHEMA``).

The wrapper creation statement also accepts the ``OR REPLACE`` modifier.
Where specified, if there is already a wrapper with the same name, its
definition is replaced by the new one.

The clause ``URIPARAM`` is always used along with the clause ``EXTERN``,
which indicates that this field represents the value of an interpolation
variable. By adding ``URIPARAM``, the value of the field will be encoded
as a query parameter of a URL. ``URIPARAM`` should only be added when
the wrapper is created over JSON data sources whose path is HTTP and the
variable is the value of a URL’s query parameter. See more details about
this in the section :ref:`HTTP Path` (subsection of :ref:`Path Types in Virtual
DataPort`) of the Administration Guide.

Lastly, certain wrapper properties can be specified
(``SOURCECONFIGURATION``). Virtual DataPort will take them into account to
determine the operations that can be made on the wrapper. The applicable
properties are indicated in the corresponding statement declaration
(:ref:`Syntax of the CREATE WRAPPER JSON statement`), and are
explained in the section :ref:`Wrapper Configuration Properties`.

