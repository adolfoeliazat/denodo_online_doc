===================================
Multidimensional Databases Wrappers
===================================

A wrapper for a multidimensional database connects to a multidimensional
database, through a multidimensional DB data source, execute a query and
return the results.

As we mentioned in the section :ref:`Multidimensional Data Sources`, although
the Administration Tool only lists “Multidimensional DB” sources,
Virtual DataPort provides four types of multidimensional data sources:

-  ``SAPBWBAPI``: to access SAP BI and SAP BW using the SAP JCo
   connector.
-  ``ESSBASE``: to access Oracle Essbase databases.
-  ``OLAP``: to access any multidimensional database that provides an
   XMLA interface. E.g., Mondrian and Microsoft SQL Analysis Services.

It does the same with multidimensional wrappers.

The following figures contain the syntax of the commands to create and
modify the wrappers that connect to multidimensional databases:

-  SAPBWBAPI
-  ESSBASE
-  OLAP

The appendix :doc:`/vdp/administration/appendix/mapping_multidimensional_data_to_a_relational_model/mapping_multidimensional_data_to_a_relational_model`
of the Administration Guide explains how the results of MDX queries are
mapped to a relational structure.


.. code-block:: bnf
   :caption: Syntax of the CREATE WRAPPER SAPBWBAPI statement
   :name: Syntax of the CREATE WRAPPER SAPBWBAPI statement

   CREATE [ OR REPLACE ] WRAPPER SAPBWBAPI <name:identifier>
       [ FOLDER = <literal> ]
       DATASOURCENAME = <name:identifier>
       {
             MDXSENTENCE = <name:literal>
           | SCHEMANAME = <name:literal> CUBENAME = <name:literal>
           [ DIMENSIONKEYFIELDS = { TRUE | FALSE } ]
           [ LEAFLEVELMEMBERSONLY = { TRUE | FALSE } ]
           [ INCLUDEEMPTYROWS = { TRUE | FALSE } ]
       }
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
       [, <source configuration property> ]* ] ) ]

   <field> ::=
       <name:identifier> [ = <mapping:literal> ] : <type:literal>
       [ ( { OBL | OPT } ) ]
       [ ( DEFAULTVALUE = <literal> ) ]
       [ EXTERN ]
       [ 
             <inline constraints> ]*
           | <name:identifier> [ = <mapping:literal> ] : ARRAY OF ( <register
             field> )
       [ ( DEFAULTVALUE = <literal> ) ]
       [ 
         <inline constraints> ]*
       | <name:register field>

   <register field> ::=
       <name:identifier> [ = <mapping:literal> ] :
       REGISTER OF ( <field> [, <field> ]* )
       [ ( DEFAULTVALUE = <literal> ) ]
       [ <inline constraints> ]*

   <inline constraint> ::=
         [ NOT ] NULL
       | [ NOT ] UPDATEABLE
       | { SORTABLE [ ASC | DESC ] | NOT SORTABLE }
       | ATTRIBUTE
       | DIMENSIONNAME = <literal>
       | HIERARCHYNAME = <literal>
       | LEVELPOSITION = <position:integer>
       | MEASURE
       | VARIABLE

   <source configuration property> ::=
   DATAINORDERFIELDSLIST = {
         DEFAULT
       | ( <name:identifier> { ASC | DESC }
           [, <name:identifier> { ASC | DESC } ]* ) }


Adding the clause ``LEAFLEVELMEMBERSONLY = TRUE`` is equivalent to
selecting the “Include leaf levels of hierarchies only” check box in the
“Multidimensional Data Source” dialog of the Administration Tool.

Adding the clause ``INCLUDEEMPTYROWS = TRUE`` is equivalent to selecting
the “Include empty rows” check box in the “Multidimensional Data Source”
dialog of the Administration Tool.

Adding the clause ``DIMENSIONKEYFIELDS = TRUE`` is equivalent to
selecting the “Include Member Keys” check box in the “Multidimensional
Data Source” dialog of the Administration Tool. 

These options are explained
in the section :ref:`Creating a Base View over a Multidimensional Data
Source, Graphically` of the Administration Guide.



.. code-block:: bnf
   :caption: Syntax of the ALTER WRAPPER SAPBWBAPI statement
   :name: Syntax of the ALTER WRAPPER SAPBWBAPI statement

   ALTER WRAPPER SAPBWBAPI <name:identifier>
       [ DATASOURCENAME = <name:identifier> ]
       [
             MDXSENTENCE = <name:literal>
           | SCHEMANAME = <name:literal> CUBENAME = <name:literal>
             [ DIMENSIONKEYFIELDS = { TRUE | FALSE } ]
             [ LEAFLEVELMEMBERSONLY = { TRUE | FALSE } ]
             [ INCLUDEEMPTYROWS = { TRUE | FALSE } ]
       ]
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
       [, <source configuration property> ]* ] ) ]

.. 

   <field> ::= (see :ref:`Syntax of the CREATE WRAPPER SAPBWBAPI statement`)

   <source configuration property> ::= (see :ref:`Syntax of the CREATE WRAPPER SAPBWBAPI statement`)


.. code-block:: bnf
   :caption: Syntax of the CREATE WRAPPER ESSBASE statement
   :name: Syntax of the CREATE WRAPPER ESSBASE statement

   CREATE [ OR REPLACE ] WRAPPER ESSBASE <name:identifier>
       [ FOLDER = <literal> ]
       DATASOURCENAME = <name:identifier>
       SERVERNAME = <name:literal> [ NONAPSSERVERNAME ]
       APPLICATIONNAME = <name:literal>
       CUBENAME = <name:literal>
       [   MDXSENTENCE = <name:literal>
         | INCLUDEEMPTYROWS = { TRUE | FALSE } 
           DIMENSIONALIASFIELDS = { TRUE | FALSE } 
           DIMENSIONCOMMENTSFIELDS = { TRUE | FALSE } ]     OUTPUTSCHEMA ( <field> [, <field> ]* )
       [ SOURCECONFIGURATION ( [ <source configuration property>
                                 [, <source configuration property> ]* ] ) ]
   <field> ::=
         <name:identifier> [ = <mapping:literal> ] : <type:literal>
             [ ( { OBL | OPT } ) ]
             [ ( DEFAULTVALUE = <literal> ) ]
             [ EXTERN ]
             [ <inline constraints> ]*
       | <name:identifier> [ = <mapping:literal> ] : ARRAY OF ( <register field> )
             [ ( DEFAULTVALUE = <literal> ) ]
             [ <inline constraints> ]*
       | <name:register field>
   
   <register field> ::=
       <name:identifier> [ = <mapping:literal> ] :
          REGISTER OF ( <field> [, <field> ]* )
          [ ( DEFAULTVALUE = <literal> ) ]
          [ <inline constraints> ]*
   
   <inline constraint> ::=
         [ NOT ] NULL
       | [ NOT ] UPDATEABLE
       | { SORTABLE [ ASC | DESC ] | NOT SORTABLE }
       | ATTRIBUTE
       | HIERARCHYNAME = <literal>
       | LEVELPOSITION = <position:integer>
       | LEVELTYPE = <literal>
       | MEASURE
       | NOAGGREGATE
       | VARIABLE
   
   <source configuration property> ::=
         DATAINORDERFIELDSLIST = { DEFAULT | ( <name:identifier> { ASC | DESC }
                                         [, <name:identifier> { ASC | DESC } ]* ) }

If the parameter ``DIMENSIONALIASFIELDS`` is ``TRUE``, it indicates that
the wrapper and the base view will have a field for each selected
hierarchy that represents the alias of the hierarchy. Adding this
parameter is equivalent to selecting the check box **Include member
alias** in the wizard to create Essbase base views in the Administration
Tool.

If the parameter ``DIMENSIONCOMMENTSFIELDS`` is ``FALSE``, it indicates
that the wrapper and the base view will have a field for each selected
hierarchy that represents the comment of the hierarchy. Adding this
parameter is equivalent to selecting the check box **Include member
comments** in the wizard to create Essbase base views in the
Administration Tool.



.. code-block:: bnf
   :caption: Syntax of the ALTER WRAPPER ESSBASE statement
   :name: Syntax of the ALTER WRAPPER ESSBASE statement

   ALTER WRAPPER ESSBASE <name:identifier>
       [ FOLDER = <literal> ]
       [ DATASOURCENAME = <name:identifier> ]
       [ SERVERNAME = <name:literal> [ NONAPSSERVERNAME ] ]
       [ APPLICATIONNAME = <name:literal> ]
       [ CUBENAME = <name:literal> ]
       [ 
             MDXSENTENCE = <name:literal>
           | INCLUDEEMPTYROWS = { TRUE | FALSE }
             DIMENSIONALIASFIELDS = { TRUE | FALSE }
             DIMENSIONCOMMENTSFIELDS = { TRUE | FALSE }
       ]
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
       [, <source configuration property> ]* ] ) ]

..

   <field> ::= (see :ref:`Syntax of the CREATE WRAPPER ESSBASE statement`)

   <source configuration property> ::= (see :ref:`Syntax of the CREATE WRAPPER ESSBASE statement`)



.. code-block:: bnf
   :caption: Syntax of the CREATE WRAPPER OLAP statement
   :name: Syntax of the CREATE WRAPPER OLAP statement

   CREATE [ OR REPLACE ] WRAPPER OLAP <name:identifier>
       [ FOLDER = <literal> ]
       DATASOURCENAME = <name:identifier>
       {   MDXSENTENCE = <name:literal>
         | CATALOGNAME = <name:literal> 
           SCHEMANAME = <name:literal>
           CUBENAME = <name:literal> 
       }
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
             [ ( DEFAULTVALUE <literal> ) ]
             [ <inline constraints> ]*
   
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
   :caption: Syntax of the ALTER WRAPPER OLAP statement
   :name: Syntax of the ALTER WRAPPER OLAP statement

   ALTER WRAPPER OLAP <name:identifier>
       [ DATASOURCENAME = <name:identifier> ]
       { 
             MDXSENTENCE = <name:literal>
           | CATALOGNAME = <name:literal>
             SCHEMANAME = <name:literal>
             CUBENAME = <name:literal>
       }
       [ OUTPUTSCHEMA ( <field> [, <field>]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
       [, <source configuration property> ]* ] ) ]

..

   <field> ::= (see :ref:`Syntax of the CREATE WRAPPER OLAP statement`)

   <source configuration property> ::= (see :ref:`Syntax of the CREATE WRAPPER OLAP statement`)
