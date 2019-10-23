=============
JDBC Wrappers
=============

A JDBC *wrapper* extracts data from a database via JDBC. 

.. code-block:: bnf
   :caption: Syntax of the CREATE WRAPPER JDBC statement
   :name: Syntax of the CREATE WRAPPER JDBC statement

   CREATE [ OR REPLACE ] WRAPPER JDBC <name:identifier>
       [ FOLDER = <literal> ]
       DATASOURCENAME = <name:identifier>
       { 
             [ CATALOGNAME = <name:literal> ] [ SCHEMANAME = <name:literal> ] 
             RELATIONNAME = <name:literal>
           | [ CATALOGNAME = <name:literal> ] [ SCHEMANAME = <name:literal> ] 
             [ PACKAGENAME = <name:literal> ] PROCEDURENAME = <name:literal>
           | SQLSENTENCE = <literal>
       }
       [ OUTPUTSCHEMA ( <field> [, <field> ]* ) ]
       [ ALIASES ( <alias> [, <alias> ]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
                               [, <source configuration property> ]* ] ) ]
       [ <primary_key> ]
       [ <foreign_key> [, <foreign_key> ] ]*
       [ <index> [, <index> ] ]*
   
   <field> ::=
       <name:identifier> [ = <mapping:literal> ] : <type:literal>
             [ ( { OBL | OPT } ) ]
             [ ( <field property> = <literal> 
                 [, <field property> = <literal> ]* ) ]
             [ <inline constraints> ]*
     | <name:identifier> [ = <mapping:literal> ] : 
             ARRAY OF ( <register field> )
             [ ( DEFAULTVALUE = <literal> ) ]
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
          [ ( <field property> = <literal> [, <field property> = <literal> ]* ) ]
          [ <inline constraints> ]*
   
   <inline constraint> ::=
       [ NOT ] NULL
     | [ NOT ] UPDATEABLE
     | { SORTABLE [ ASC | DESC ] | NOT SORTABLE }
     | ESCAPE
     | EXTERN
     | EXTERNWHEREEXPRESSION
     | IS_AUTOINCREMENT
     | ISCURSOR
     | ISTABLE
     | MAXLEN = <max. length of the field:integer> (only for JDBC wrappers 
                 obtaining data from Oracle PL/SQL. See below)
     | PARAMINDEX = <integer>
     | SQLFRAGMENT
   
   <alias> ::= 
     <literal> = <literal>
   
   <source configuration property> ::=
       ALLOWDELETE = { true | false | DEFAULT }
     | ALLOWINSERT = { true | false | DEFAULT }
     | ALLOWUPDATE = { true | false | DEFAULT }
     | DELEGATESQLSELECTION = { true | false | DEFAULT }
     | DELEGATESQLSENTENCEASSUBQUERY = { true | false | DEFAULT }
     | DATAINORDERFIELDSLIST = { ( <name:identifier> { ASC | DESC }
       [, <name:identifier> { ASC | DESC } ]* ) | DEFAULT }
     | SUPPORTSDISTRIBUTEDTRANSACTIONS = { true | false | DEFAULT }
   
   <primary_key> ::= 
     [ CONSTRAINT <name:literal> ] 
     PRIMARY KEY ( <field_name:literal> [, <field_name:literal> ]* )
   
   <foreign_key> ::= 
     [ CONSTRAINT <name:literal> ] 
     FOREIGN KEY ( <field_name:literal> [, <field_name:literal> ]* )
     REFERENCES [ <schema_name:literal>.]<table_name:literal>  
     ( <field_name:literal> [, <field_name:literal> ]* )
     [ ON UPDATE <action_rule> ]
     [ ON DELETE <action_rule> ]
     [ <constraint_check_time> ]
                 
   <action_rule> ::= 
       CASCADE
     | SET NULL 
     | SET DEFAULT
     | NO ACTION
     | RESTRICT
   
   <constraint_check_time> := 
       NOT DEFERRABLE
     | INITIALLY DEFERRED
     | INITIALLY IMMEDIATE
                           
   <index> ::= 
     INDEX <name:literal> { HASH | CLUSTER | OTHER }
     [ UNIQUE ] [ PRIMARY ]
     ( <field_name:literal> [ { ASC | DESC } ] [, <field_name:literal> [ { ASC | DESC } ] ]* )


.. code-block:: bnf
   :caption: Syntax of the ALTER WRAPPER JDBC statement
   :name: Syntax of the ALTER WRAPPER JDBC statement

   ALTER WRAPPER JDBC <name:identifier>
       [ DATASOURCENAME = <name:identifier>]
       [
             [ CATALOGNAME = <name:literal> ] [ SCHEMANAME = <name:literal> ]
               RELATIONNAME = <name:literal>
           | SQLSENTENCE = <literal>
       ]
       [ OUTPUTSCHEMA ( <field> [, <field>]* ) ]
       [ ALIASES ( <alias> [, <alias>]* ) ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
       [, <source configuration property> ]* ] ) ]

.. 

   <field> ::= (see :ref:`Syntax of the CREATE WRAPPER JDBC statement`)

   <alias> ::= (see :ref:`Syntax of the CREATE WRAPPER JDBC statement`)

   <source configuration property> ::= (see :ref:`Syntax of the CREATE WRAPPER JDBC statement`)

To define a JDBC *wrapper*, you have to indicate the name of the JDBC
*data source* (``DATASOURCENAME``). There are three ways to indicate
where the wrapper has to retrieve the data from:

#. If the data comes from a table or a view of the database, specify the
   name of the table (``RELATIONNAME``) and optionally, its catalog
   (``CATALOGNAME``) and schema (``SCHEMANAME``) in the database.
#. If the data comes from a stored procedure of the database, specify
   the name of the procedure (``PROCEDURENAME``). If the database is
   Oracle and the procedure belongs to a package of procedures, add the
   parameter ``PACKAGENAME``. In addition, optionally, specify its
   catalog (``CATALOGNAME``) and schema (``SCHEMANAME``) in the
   database.
#. Or, specify a SQL statement (``SQLSENTENCE``). The SQL statement can
   contain an interpolation string (see section :ref:`Execution Context of a
   Query and Interpolation Strings`).

The ``OUTPUTSCHEMA`` clause defines the schema of the data that the
wrapper will provide (see section :ref:`Wrapper Metadata`). For each
simple-type element the type must be specified. Furthermore, an
association may be indicated between the name of the field returned by
the wrapper and the name of the field in the database (as specified in
the mapping). If this clause is not defined, the results returned by the
wrapper must be compatible with the schema of the base view the wrapper
is associated with. More specifically, the names of the attributes
obtained as results of the query must match those of the base view, and
their values must be compatible with their data types in relation base.

The field properties ``SOURCETYPENAME``, ``SOURCETYPEID``,
``SOURCETYPESIZE``, ``SOURCETYPEDECIMALS`` and ``SOURCETYPERADIX`` store
metadata about the field returned by the JDBC driver of the database:

-  ``SOURCETYPENAME`` is the name of the type in the database.
   Its value is the value of the ``TYPE_NAME`` property returned by the
   JDBC driver of the database.
-  The value of the ``SOURCETYPEID`` property correspond with the
   constants defined in the class `java.sql.Types <https://docs.oracle.com/javase/8/docs/api/index.html?java/sql/Types.html>`_ of the Java API.
   For example, if the type of the field in the
   database is VARCHAR, the value of this property is 12. If the field
   is INTEGER, the value is 4. If DECIMAL, 3, etc.
   Its value is the value of the ``DATA_TYPE`` property returned by the
   JDBC driver of the database.
-  ``SOURCETYPESIZE`` is the specified column size for the column. For
   numeric fields, this is the maximum precision. For character fields,
   this is the maximum length. For datetime datatypes, the length in
   characters of the String representation. For binary data, the length
   in bytes.
   It is null if the property is not applicable to the data type of this
   field.
   Its value is the value of the ``COLUMN_SIZE`` property returned by
   the JDBC driver of the database.
-  ``SOURCETYPEDECIMALS`` is the number of fractional digits. Its value
   is null when the property is not applicable to the data type of this
   field.
   Its value is the value of the ``DECIMAL_DIGITS`` property returned by
   the JDBC driver of the database.
-  ``SOURCETYPERADIX`` the radix of the field (typically either 10 or
   2).
   Its value is the value of the ``NUM_PREC_RADIX`` property of the
   returned by the JDBC driver of the database.

A field with the *inline constraint* ``EXTERNWHEREEXPRESSION`` is a
field that belongs to the schema of the view, but its value is not
projected. It is only used to delegate conditions with this field, to
the database.

If a field has the *inline constraint* ``SQLFRAGMENT`` and a query uses
this field in its ``WHERE`` clause, the value given to the field is
delegated to the database “as is”, without parsing the condition.

.. note:: The clause ``SQLFRAGMENT`` is deprecated and may be removed in
   future versions of the Denodo Platform.

For example, if you create a base view from the following query:



.. code-block:: vql

   SELECT *
   FROM internet_inc
   WHERE @condition

The base view will have a field called ``condition``. If you add the clause ``SQLFRAGMENT`` to the definition of the field, when you query
this view, the Server will not parse the value of ``condition`` nor
escape the quotes in it and it will be delegated “as is” to the
database. This is useful if you want to use an operator that is not
supported by Virtual DataPort.



.. code-block:: vql

   SELECT *
   FROM new_base_view
   WHERE condition = '<complex condition>'

The ``ALIASES`` clause is useful when the ``SQLSENTENCE`` option and the special interpolation variable ``WHEREEXPRESSION`` (see section :ref:`Using
WHEREEXPRESSION`) are used.

The *wrapper* creation statement accepts the ``OR REPLACE`` modifier.
Where specified, if there is already a wrapper with the same name, its
definition is replaced by the new one.

Certain wrapper properties can be specified (``SOURCECONFIGURATION``).
Virtual DataPort will take them into account to determine the operations
that can be made on the wrapper. The applicable properties are indicated
in the corresponding statement declaration (:ref:`Syntax of the
CREATE WRAPPER JDBC statement`), and are explained in the section :ref:`Wrapper Configuration Properties`.

The ``PRIMARY KEY`` clause sets the definition of the primary key of the
view. See more information about primary keys in the section :ref:`Primary Keys of Views` 
of the Administration Guide.

The clauses ``INDEX`` store the indexes of the table, in the database.
This clause is not compatible with the clause ``SQLSENTENCE``. At
runtime, clients can obtain the information of the index using the
appropriate methods of the JDBC API.

The section :ref:`Indexes of Views` of the Administration Guide gives more
details about the support of indexes in Virtual DataPort.

Specification of a Table in the Remote Database
=================================================================================

The first alternative for specifying the data to be obtained from the
remote database is to indicate the name of the table or view in the
database from which the data should be extracted.


Using a SQL Statement
=================================================================================

The other mechanism for creating JDBC wrappers is defining a SQL
statement that will be sent to the database when the wrapper is queried.
We can use this mechanism to invoke stored procedures of the database or
execute complex queries.

The specified SQL statement is an interpolation string susceptible to
being parameterized with variables received from the execution context
(see section :ref:`Execution Context of a Query and Interpolation
Strings` for details on
same).

Using WHEREEXPRESSION
-----------------------------------------------------------------------------------------------------

Virtual DataPort provides a predefined interpolation variable called
``WHEREEXPRESSION`` that simplifies the creation of base views that when
queried, instead of querying a table or a view of the database, execute
a specific SQL statement (``CREATE WRAPPER JDBC`` with the parameter
``SQLSENTENCE``). At runtime, the Server will replace
``WHEREEXPRESSION`` with the condition sent to the wrapper of the base
view.

The use of ``WHEREEXPRESSION`` in the SQL statement of the base view may
benefit the performance of the queries to this base view. E.g. if a join
view uses the ``NESTED`` execution method and the view on the right side
of the join is of the SQL statement type, this view should be created
using ``WHEREEXPRESSION``. That is because in this case, the Server can
apply optimizations that cannot be used if the SQL statement does not
use ``WHEREEXPRESSION``.

See more about this in the section :ref:`Using the WHEREEXPRESSION
Variable` of the Administration Guide.


Importing an Oracle PL/SQL Stored Procedure
-----------------------------------------------------------------------------------------------------

To create a JDBC wrapper that invokes a stored procedure, we have to
create the wrapper using a SQL statement.

If we invoke a PL/SQL procedure from an Oracle database, we can define
the maximum length of the returned fields.



.. code-block:: vql
   :caption: Example of JDBC wrapper that invokes an Oracle PL/SQL procedure
   :name: Example of JDBC wrapper that invokes an Oracle PL/SQL procedure

   CREATE WRAPPER JDBC pl_sql_sample
       FOLDER = '/oracle'
       DATASOURCENAME = ds_jdbc_oracle_sample
       SQLSENTENCE = 'CALL sampleStoredProcedureWithTable(?)' ISPROCEDURE
       OUTPUTSCHEMA (
           O_ID_RECORD: ARRAY OF (
               VALUE: REGISTER OF (
                   VALUE:'java.lang.String' (OPT) NOT NULL 
                                                  NOT SORTABLE 
                                                  NOT UPDATEABLE
                                                  MAXLEN=100
               ) NOT SORTABLE 
                 NOT UPDATEABLE
           ) NOT NULL 
             NOT SORTABLE
             NOT UPDATEABLE
             MAXLEN=50
       )
   ;

This VQL statement creates a JDBC wrapper that invokes a PL/SQL procedure
called ``sampleStoredProcedureWithTable``. This procedure returns an
element that is an array of registers. Each register has a ``String``
field. Each of these fields can have a maximum length of 100 characters
(``MAXLEN = 100``) and the array can have fifty elements at most
(``MAXLEN = 50``).

The following properties of the file
:file:`{<DENODO_HOME>}/conf/VDBConfiguration.properties` define:

-  ``com.denodo.vdb.engine.wrapper.raw.jdbc.adapter.plugins.OraclePlugin.storedProcedure.table.maxlen``:
   default value for the maximum number of registers of an array. In this
   example, this value would be used if ``MAXLEN = 50`` was not defined.

-  ``com.denodo.vdb.engine.wrapper.raw.jdbc.adapter.plugins.OraclePlugin.storedProcedure.register.maxlen``:
   default value for the maximum length of the fields of a register. In
   this example, this value would be used if ``MAXLEN = 100`` was not
   defined.
