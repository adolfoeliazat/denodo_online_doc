===============
CUSTOM Wrappers
===============

Custom wrappers provide access to a source through a specific
implementation. The CUSTOM wrappers are associated with a CUSTOM data
source. In the creation process for this type of data sources (see
section :doc:`../creating_data_sources/custom_data_sources`), a class implementing the wrappers of
this type must be specified.

The section :doc:`/vdp/developer/developing_extensions/developing_custom_wrappers/developing_custom_wrappers` 
of the Developer Guide explains
how to develop custom wrappers.

:ref:`Syntax of the CREATE WRAPPER CUSTOM statement` shows the syntax
for creating a CUSTOM-type wrapper. The only mandatory parameter
received in its creation - as well as a name to identify it by - is the
name of the data source from which it will be created (see section :doc:`../creating_data_sources/custom_data_sources`).

Where the data source wrappers accept configuration parameters, the
``PARAMETERS`` clause allows specifying them.

The clause ``URIPARAM`` is always used along with the clause ``EXTERN``,
which indicates that this field represents the value of an interpolation
variable. By adding ``URIPARAM``, the value of the field will be encoded
as a query parameter of a URL. ``URIPARAM`` should only be added when
the wrapper uses an HTTP path and the variable is the value of a URL’s
query parameter. See more details about this in the section :ref:`HTTP Path`
(subsection of :ref:`Path Types in Virtual DataPort`) of the Administration
Guide.

Lastly, certain wrapper properties can be specified
(``SOURCECONFIGURATION``). They will be used to determine the operations
that can be executed over the wrapper. The applicable properties are
indicated in the corresponding statement declaration (`Syntax of the
CREATE WRAPPER CUSTOM statement`_), and are
explained in the section :ref:`Wrapper Configuration Properties`.



.. code-block:: bnf
   :caption: Syntax of the CREATE WRAPPER CUSTOM statement
   :name: Syntax of the CREATE WRAPPER CUSTOM statement

   CREATE [ OR REPLACE ] WRAPPER CUSTOM <name:identifier>
       [ FOLDER = <literal> ]
       DATASOURCENAME = <name:identifier>
       [ PARAMETERS ( <param name:identifier> = <param value>
                 [, <param name:identifier> = <param value> ]* ) ] 
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
   
   <param value> ::=
         <text:literal> [ ENCRYPTED ]
       | <boolean value:boolean>
       | <number value:number>
       | ROUTE <route> [ <route_filters> ]
   
   <field> ::=
         <name:identifier> [ = <mapping:literal> ] : <type:literal>
           [ ( { OBL | OPT } ) ] 
           [ DEFAULTVALUE <literal> ] 
           [ EXTERN ]
           [ <inline constraints> ]*
       | <name:identifier> [ = <mapping:literal> ] : ARRAY OF ( <register field> )
           [ DEFAULTVALUE <literal> ]
           [ <inline constraints> ]*
       | <name:register field>
   
   <register field> ::=
       <name:identifier> [ = <mapping:literal> ] :
         REGISTER OF ( <field> [, <field> ]* )
           [ DEFAULTVALUE <literal> ]
           [ <inline constraints> ]*
   <inline constraint> ::=
         [ NOT ] NULL
       | [ NOT ] UPDATEABLE
       | { SORTABLE [ ASC | DESC ] | NOT SORTABLE } 
       | URIPARAM
      
..

   <route> ::= (see :ref:`Syntax to set the path in a DF, JSON or XML data source`)

   <route_filters> ::= (see :ref:`Syntax to set a filter in a DF, JSON or XML data source`)

:ref:`Below <Example of creating a Custom wrapper>` there is an example of how to create a
CUSTOM wrapper. The wrapper is given the name ``testcustom`` and is
associated with the CUSTOM data source known as ``testcustomds``. The
``testcustomds`` data source wrappers receive two configuration
parameters known as ``ENTERPRISE`` and ``YEAR``. The new wrapper is
configured using the values ``'enterprise1'`` and
``'2006'``, respectively.



.. code-block:: vql
   :caption: Example of creating a Custom wrapper
   :name: Example of creating a Custom wrapper

   CREATE WRAPPER CUSTOM testcustom
       FOLDER '/wrappers/custom'
       DATASOURCENAME = testcustomds
       PARAMETERS (
       ENTERPRISE = 'enterprise1', YEAR = '2006'
   ) ;


See :ref:`below <Syntax of the ALTER WRAPPER CUSTOM statement>` the syntax to 
modify custom wrappers. The options available are the same as for the creation of the
wrapper.



.. code-block:: bnf
   :caption: Syntax of the ALTER WRAPPER CUSTOM statement
   :name: Syntax of the ALTER WRAPPER CUSTOM statement

   ALTER WRAPPER CUSTOM <name:identifier>
       [ DATASOURCENAME = <name:identifier> ]
       [ PARAMETERS ( <param name:identifier> = <param value>
       [, <param name:identifier> = <param value> ]* ) ]
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]

..

   <param value> ::= (see :ref:`Example of creating a Custom
   wrapper`)

   <field> ::= (see :ref:`Example of creating a Custom
   wrapper`)

