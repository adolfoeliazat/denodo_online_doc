==============================
Modifying SOAP Web Services
==============================

To modify a SOAP web service use command ALTER SOAP WEBSERVICE. With this statement, you can add, replace or drop operations from a SOAP web service.

.. code-block:: bnf
   :caption: Syntax of the ALTER SOAP WEBSERVICE statement
   :name: Syntax of the ALTER SOAP WEBSERVICE statement

   ALTER SOAP WEBSERVICE <name:identifier>
      SET OPERATION <name:literal> (
           TYPE { SELECT | INSERT | UPDATE | DELETE }
           SCHEMA { VIEW | WRAPPER <wrapper type> | STOREDPROCEDURE } <schema name:literal>
           VQL = <literal>
           INPUTS ( [ <input parameter> ]* )
           OUTPUT <return parameter>
      )

   ALTER SOAP WEBSERVICE <name:identifier>
      DROP OPERATION [ IF EXISTS ] <name:literal>

   <input parameter> ::=
       [ <input parameter type> ] <name:literal> <name in the view/query:literal>
       [ : <type:literal> ] <operator:literal> [ OBL ] [ ( <renamed field> ) ]

   <input parameter type> ::= ORDERBY | OFFSET | FETCH

   <return parameter> ::=
         <simple type:literal>
       | <register type:literal> : ARRAY OF ( <return parameter register> )
         [ <renamed field> [, <renamed field> ]* ]

   <return parameter register> ::=
       <name:literal> : REGISTER OF ( <register field> [, <register field> ]* )

   <register field> ::=
       <name:literal> [ : <type:literal> ]

   <renamed field> ::=
       <XPath of the field:literal> = <displayed Name:literal>
       [ : [ <displayed array type name:literal> / ]
       <displayed type name: literal> ]

   <wrapper type> ::= { ARN | CUSTOM | DF | ESSBASE | GS | ITP | JDBC | JSON
       | LDAP | ODBC | OLAP | SALESFORCE | SAPBWBAPI | SAPERP | WS | XML }


With this command, you can add, replace or drop operations to existent SOAP
web services.

To add or replace a resource define the operation you want to
publish in the ``SET OPERATION`` clause. Each operation will
run a VQL statement that is indicated in the ``VQL`` property of the
operation. The operation may act on a view, a wrapper, a stored
procedure or execute a specific VQL statement (``SCHEMA`` property). The
type of the statement (``TYPE`` property) can be: ``SELECT`` (most
common), ``INSERT``, ``UPDATE`` or ``DELETE``. Each operation contains a
list of input parameters and one output parameter. The output parameter
of query operation will be an array of registers containing the results
of the query run. Insert / update / delete operations return the number
of tuples affected by the operation.

To remove an operation, use the ``DROP OPERATION`` clause.
