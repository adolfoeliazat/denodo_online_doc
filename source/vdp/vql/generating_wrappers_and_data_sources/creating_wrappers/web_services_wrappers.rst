=====================
Web Services Wrappers
=====================

Virtual DataPort supports the creation of wrappers for SOAP Web
services. Through the data contained in a WSDL of a Web service (which
was indicated when creating the Web service data source), the wrapper
selects a specific operation to be modeled as a base view, defines how
the different parameters required for executing the operation are
established and which output data will form part of the wrapper result.

.. code-block:: bnf
   :caption: Syntax of the CREATE WRAPPER WS statement
   :name: Syntax of the CREATE WRAPPER WS statement

   CREATE [ OR REPLACE ] WRAPPER WS <name:identifier>
       [ FOLDER = <literal> ]
       DATASOURCENAME = <name:identifier>
       SERVICENAME = <literal>
       PORTNAME = <literal>
       OPERATIONNAME = <literal>
       [ INPUTMESSAGE = <literal> OUTPUTMESSAGE = <literal> ]
       [ TUPLEROOT = <literal> ]
       [ FROMEXAMPLE ]
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
       | DELEGATEOPERATORSLIST = { DEFAULT | ( <operator:identifier>
                                           [, <operator:identifier> ]* ) }

The modification syntax of a Web service wrapper is similar (see :ref:`Syntax
of the ALTER WRAPPER WS statement`).



.. code-block:: bnf
   :caption: Syntax of the ALTER WRAPPER WS statement
   :name: Syntax of the ALTER WRAPPER WS statement

   ALTER WRAPPER WS <name:identifier>
       [ DATASOURCENAME = <name:identifier> ]
       [ SERVICENAME = <name:literal> ]
       [ PORTNAME = <name:literal> ]
       [ OPERATIONNAME = <operation:literal> ]
       [ INPUTMESSAGE = <input:literal> OUTPUTMESSAGE = <output:literal> ]
       [ TUPLEROOT = <literal> ]
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
       [, <source configuration property> ]* ] ) ]

..

   <field> ::= (see :ref:`Syntax of the CREATE WRAPPER WS statement`)

   <source configuration property> ::= (see :ref:`Syntax of the
   CREATE WRAPPER WS statement`)


-  ``DATASOURCENAME``: Name of the data source used by the wrapper.

   When creating the wrapper, the Server retrieves the WSDL of the Web
   service. If the WSDL specified by the data source cannot be found,
   the Server creates the wrapper but marks it as incomplete and the
   queries to it will fail. Once you execute a query over this wrapper
   and the Server can retrieve the WSDL, the wrapper is marked as
   complete and the wrapper will be “queryable”.
   
-  ``SERVICENAME``: Name of the Web service on which the operation is to
   be invoked. A WSDL file can contain the definition of various Web
   services.
-  ``PORTNAME``: Name of the port containing the specific operation.
-  ``OPERATIONNAME``: name of the operation. There may be several
   different operations with the same name, which are distinguished
   because of the input/output messages they allow. These are indicated
   in the following parameters.
-  ``INPUTMESSAGE``: Name of the message that defines the input
   parameters of the operation of the search method to be modeled
   (optional).
-  ``TUPLEROOT``: If present, it means that the wrappers will use the
   “Stream output at the specified level” approach to transform the data
   returned by the Web service operation. The section :ref:`XML Sources` of
   the Administration Guide explains how this option works.
-  ``OUTPUTMESSAGE``: Name of the message that defines the output
   parameters of the operation of the search method to be invoked
   (optional)

The attributes of the messages of the selected operation define the Web
services wrapper schema, i.e. a Web service wrapper has as a schema the
input, output and input-output attributes with the names defined in the
WSDL file.

.. note:: Operations can also use compound parameters in the input
   message. These parameters will be converted to Virtual DataPort compound types
   (see section :ref:`Management of Compound Values`) in the same way as those
   of the output message and you may specify conditions on them using the
   compound value constructors ``ROW`` and ``{ }`` (see
   section :ref:`Conditions with Compound Values`).

From the list of conditions received the wrapper will create the
parameters required to invoke the Web service and obtain the required
results.

As with the other wrappers, it is possible to explicitly indicate the
output schema of the wrapper (``OUTPUTSCHEMA``) together with the
associations between the external attributes and the parameters of the
Web service. The attribute “name” of a field of the ``OUTPUTSCHEMA``
indicates the name with which the wrapper will export the element. The
“mapping” attribute indicates the name used by the Web service. To
reference the different elements of a Web service in the mappings to be
made the following notation is used:

-  ``$<parameterNumber>`` -> references the parameter of the indicated
   position of the Web service operation.
-  ``$$`` -> references the output parameter returned through
   invocation of the Web service operation.

This is the notation used for the elements of the first level (input and
output parameters and output of the Web service). For the other elements
(fields of a result object or of a Web Service parameter) the mapping is
obtained from the name of the property in the corresponding object.

The *wrapper* creation statement accepts the ``OR REPLACE`` modifier.
Where specified, if there is already a wrapper with the same name, its
definition is replaced by the new one.

Lastly, certain wrapper properties can be specified
(``SOURCECONFIGURATION``). Virtual DataPort takes them into account to
determine the operations that can be made on the wrapper. The applicable
properties are indicated in the corresponding statement declaration
(:ref:`Syntax of the CREATE WRAPPER WS statement`), and are explained in
section :ref:`Wrapper Configuration Properties`.

