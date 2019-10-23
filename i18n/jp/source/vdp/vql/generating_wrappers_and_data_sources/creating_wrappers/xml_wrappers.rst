============
XML Wrappers
============

Virtual DataPort supports the creation of wrappers from XML data
sources. See :ref:`below <Syntax of the CREATE WRAPPER XML statement>` the syntax for creating XML wrappers.



.. code-block:: bnf
   :caption: Syntax of the CREATE WRAPPER XML statement
   :name: Syntax of the CREATE WRAPPER XML statement

   CREATE [ OR REPLACE ] WRAPPER XML <name:identifier>
       [ FOLDER = <literal> ]
       DATASOURCENAME = <name:identifier>
       [ TUPLEROOT <XML node or path:literal> ]
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
                               [, <source configuration property> ]* ] ) ]
   
   <field> ::=
         <name:identifier> [ = <mapping:literal> ] [ : <type:literal> ]
           [ { OBL | OPT } ]
           [ ( DEFAULTVALUE <literal> ) ]
           [ EXTERN ]
           [ <inline constraints> ]*
       | <name:identifier> [ = <mapping:literal> ] : 
            ARRAY OF ( <register field> )
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

See :ref:`below <Syntax of the ALTER WRAPPER XML statement>` the syntax to modify XML wrappers.



.. code-block:: bnf
   :caption: Syntax of the ALTER WRAPPER XML statement
   :name: Syntax of the ALTER WRAPPER XML statement

   ALTER WRAPPER XML <name:identifier>
       [ DATASOURCENAME = <name:identifier> ]
       [ TUPLEROOT <XML node or path:literal> ]
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
       [, <source configuration property> ]* ] ) ]

..

   <field> ::= (see :ref:`Syntax of the CREATE WRAPPER XML statement`)

   <source configuration property> ::= (see :ref:`Syntax of the
   CREATE WRAPPER XML statement`)


An XML wrapper is defined through an XML *data source* that identifies a
local or remote XML resource.

The XML wrapper analyzes the structure of the XML document and returns
as attributes the XML tags of the first level (using its name as
attribute name), encapsulating the other elements in compound types.

Optionally, it is possible to indicate an XPath route
(`XPath Language <https://www.w3.org/TR/xpath/>`_) to an XML document node using the ``TUPLEROOT``
parameter. This is useful for Virtual DataPort to access only a portion of the
document instead of the entire document. In this case, the node
indicated by the path will be considered the root node for extraction.
Each subelement of the indicated node will be considered a field in the
tuples extracted. For example, if we import an RSS document and want the
wrapper to return a tuple for each ``item`` element, the path ``/rss/channel/item`` may be used. Although an equivalent effect is
possible by accessing the full XML document and subsequently using
projection and flattening operations (see section :ref:`FLATTEN View
(Flattening Data Structures)`) to get the required data, specifying the
XPath route at the time of creation of the base relation will make the
query process more efficient.

As with the other wrappers, the output schema of the data provided by
the wrapper can be specified. This way it is possible to select only the
elements of interest from the XML document to change their name
(*mapping* represents the new name used in the wrapper; *name*
is the original name in the XML document).

The clause ``URIPARAM`` is always used along with the clause ``EXTERN``,
which indicates that this field represents the value of an interpolation
variable. By adding ``URIPARAM``, the value of the field will be encoded
as a query parameter of a URL. ``URIPARAM`` should only be added when
the wrapper is created over XML data sources whose path is HTTP and the
variable is the value of a URL’s query parameter. See more details about
this in the section :ref:`HTTP Path` (subsection of :ref:`Path Types in Virtual
DataPort`) of the Administration Guide.

The *wrapper* creation statement accepts the ``OR REPLACE`` modifier.
Where specified, if there is already a wrapper with the same name, its
definition is replaced by the new one.

Lastly, certain wrapper properties can be specified
(``SOURCECONFIGURATION``). Virtual DataPort will take them into account to
determine the operations that can be made on the wrapper. The applicable
properties are indicated in the corresponding statement declaration
(:ref:`Syntax of the CREATE WRAPPER XML statement`), and are explained
in the section :ref:`Wrapper Configuration Properties`.

