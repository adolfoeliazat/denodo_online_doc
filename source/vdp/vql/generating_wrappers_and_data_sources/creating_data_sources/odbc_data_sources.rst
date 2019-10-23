=================
ODBC Data Sources
=================

To create an ODBC data source, use the statement CREATE DATASOURCE ODBC.

.. important:: The creation of ODBC data sources is disabled by default
   when the Virtual DataPort server runs on Linux. To enable it, follow the
   steps described on the section :ref:`Enabling the Support for ODBC Sources When the Virtual DataPort Server Runs on Linux`
   of the Installation Guide.

The parameters of this command are almost the same as in the command
:doc:`CREATE DATASOURCE JDBC </vdp/vql/generating_wrappers_and_data_sources/creating_data_sources/jdbc_data_sources>` and they work in the same way. The differences are:

-  ``DATABASEURI``: this parameter can be an ODBC Uri or a path to a
   *local* file. If it is a path to a file, it can contain interpolation
   variables. See section :ref:`Execution Context of a Query and
   Interpolation Strings` to learn how to create base views over data
   sources with interpolation variables.
-  ``DSN``: it is the name of an ODBC Data Source Name present in the
   system. In this case, the Server uses the Sun JDBC/ODBC bridge to
   connect to the DSN.

.. note:: In the case of ODBC source types, the accessed data source
   should be located in the local machine of the Virtual DataPort server
   or, where not possible, an ODBC management system must be installed in
   which the ODBC driver of the remote database server should be
   registered.



.. code-block:: bnf
   :caption: Syntax of the CREATE DATASOURCE ODBC statement
   :name: Syntax of the CREATE DATASOURCE ODBC statement

   CREATE [ OR REPLACE ] DATASOURCE ODBC <name:identifier>
       [ FOLDER = <literal> ]
       {   DSN = <literal> 
         | DATABASEURI = <literal> DRIVERCLASSNAME = <literal>
       }
       {
           <credentials>
         | WITH PASS-THROUGH SESSION CREDENTIALS ( <credentials> )
       }
       [ PROPERTIES = <literal> ]
       [ CLASSPATH = <literal> ]
       [ DATABASENAME = <literal> DATABASEVERSION = <literal> ] 
       [ ISOLATIONLEVEL = 
           {   TRANSACTION_NONE 
             | TRANSACTION_READ_COMMITTED 
             | TRANSACTION_READ_UNCOMMITTED
             | TRANSACTION_REPEATABLE_READ
             | TRANSACTION_SERIALIZABLE
       ]
       [ IGNORETRAILINGSPACES = { true | false } ]
       [ FETCHSIZE = <integer> ]
       [
           <pool configuration 1>
         | <pool configuration 2>
         | <pool configuration 3>
       ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]
       [ SOURCECONFIGURATION ( [ <source configuration property> 
                               [, <source configuration property> ]* ] ) ]
   
   <credentials> ::= 
     USERNAME = <literal> USERPASSWORD = <literal> [ ENCRYPTED ]

   <pool configuration 1> ::=
     VALIDATIONQUERY = <literal>
     INITIALSIZE = <integer>
     MAXACTIVE = <integer>

   <pool configuration 2> ::=
     VALIDATIONQUERY = <literal>
     INITIALSIZE = <integer>
     MAXACTIVE = <integer>
     EXHAUSTEDACTION = <integer>

   <pool configuration 3> ::=
     VALIDATIONQUERY = <literal>
     INITIALSIZE = <integer>
     MAXIDLE = <integer>
     MINIDLE = <integer>
     MAXACTIVE = <integer>
     EXHAUSTEDACTION = <integer>
     TESTONBORROW = <boolean>
     TESTONRETURN = <boolean>
     TESTWHILEIDLE = <boolean>
     [ <pool sub-configuration 1> ]

   <pool sub-configuration 1> ::=
     TIMEBETWEENEVICTION = <integer>
     NUMTESTPEREVICTION = <integer>
     MINEVICTABLETIME = <integer>
     [ <pool sub-configuration 2> ]

   <pool sub-configuration 2> ::=
     POOLPREPAREDSTATEMENTS = <boolean>
     MAXSLEEPINGPS = <integer>
     INITIALCAPACITYPS = <integer>

   <source configuration property> ::=
       DELEGATEALLOPERATORS = <property value>
     | DELEGATEARRAYLITERAL = <property value>
     | DELEGATECOMPOUNDFIELDPROJECTION = <property value>
     | DELEGATEGROUPBY = <property value>
     | DELEGATEHAVING = <property value>
     | DELEGATEINNERJOIN = <property value>
     | DELEGATEJOIN = <property value>
     | DELEGATELEFTFUNCTION = <property value>
     | DELEGATELEFTLITERAL = <property value>
     | DELEGATELITERALEXPRESSION = <property value>
     | DELEGATEMIXEDAGGREGATEEXPRESSION = <property value>
     | DELEGATENATURALOUTERJOIN = <property value>
     | DELEGATENOTCONDITION = <property value>
     | DELEGATEORCONDITION = <property value>
     | DELEGATEORDERBY = <property value>
     | DELEGATEPROJECTION = <property value>
     | DELEGATEREGISTERLITERAL = <property value>
     | DELEGATERIGHTFIELD = <property value>
     | DELEGATERIGHTFUNCTION = <property value>
     | DELEGATERIGHTLITERAL = <property value>
     | DELEGATESELECTDISTINCT = <property value>
     | DELEGATESELECTION = <property value>
     | DELEGATEUNION = <property value>
     | SUPPORTSAGGREGATEFUNCTIONSOPTIONS = <property value>
     | SUPPORTSBRANCHOUTERJOIN = <property value>
     | SUPPORTSEQOUTERJOINOPERATOR = <property value>
     | SUPPORTSEXPLICITCROSSJOIN = <property value>
     | SUPPORTSFULLEQOUTERJOIN = <property value>
     | SUPPORTSFULLNOTEQOUTERJOIN = <property value>
     | SUPPORTSFUSINGINUSINGANDNATURALJOIN = <property value>
     | SUPPORTSJOINONCONDITION = <property value>
     | SUPPORTSNATURALJOIN = <property value>
     | SUPPORTSUSINGJOIN = <property value>
     | DELEGATEAGGREGATEFUNCTIONS = { 
           DEFAULT 
         | ( <function:identifier> [, <function:identifier> ]* ] ) 
         }
     | DELEGATESCALARFUNCTIONS = { 
           DEFAULT 
         | ( <function:identifier> [, <function:identifier> ]* ] ) 
         }
     | DELEGATEOPERATORSLIST = { 
           DEFAULT 
         | ( <operator:identifier> [, <operator:identifier> ]* ] ) 
         }

   <property value> ::=
       true 
     | false 
     | DEFAULT

To modify an ODBC data source, use the statement ALTER DATASOURCE ODBC.


.. code-block:: bnf
   :caption: Syntax of the ALTER DATASOURCE ODBC statement
   :name: Syntax of the ALTER DATASOURCE ODBC statement

   ALTER DATASOURCE ODBC <name:identifier>
       [   DSN = <literal> 
         | DATABASEURI = <literal> DRIVERCLASSNAME = <literal>
       ]
       [
           <credentials>
         | WITH PASS-THROUGH SESSION CREDENTIALS ( <credentials> )
       ]    
       [ PROPERTIES = <literal> ]
       [ CLASSPATH = <literal> ]
       [ DATABASENAME = <literal> DATABASEVERSION = <literal> ]
       [ ISOLATIONLEVEL = 
           {   TRANSACTION_NONE 
             | TRANSACTION_READ_COMMITTED 
             | TRANSACTION_READ_UNCOMMITTED
             | TRANSACTION_REPEATABLE_READ
             | TRANSACTION_SERIALIZABLE
       ] 
       [ IGNORETRAILINGSPACES = { true | false } ]
       [ FETCHSIZE = <integer> ]
       [
         VALIDATIONQUERY = <literal>
         INITIALSIZE = <integer>
         MAXACTIVE = <integer>
         [ TESTONBORROW = <boolean> ]
       ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]
       [ SOURCECONFIGURATION ( [ <source configuration property> 
                               [, <source configuration property> ]* ] ) ]
   <credentials> ::= 
     USERNAME = <literal> USERPASSWORD = <literal> [ ENCRYPTED ]
   
   <source configuration property> ::= (see CREATE DATASOURCE ODBC for details)

Explanation of some of the parameters of these statements:


-  ``MAXACTIVE``: maximum number of active connections the pool can manage
   at the same time. If the ``DATABASEURI`` parameter contains an
   interpolation variable, the connection pool is not used.

   -  If it is a negative value (e.g. ``-1``), there is no maximum number
      of active connections.
   -  If it is 0, the pool is disabled. Therefore, for every query Virtual
      DataPort sends to this source, it will open a new connection to this
      database. Once the query finishes, it will close this connection.
-  ``TRANSFER_RATE_FACTOR``: relative measure of the speed of the network connection between the Denodo server and the data source. Use the default value (e.g. 1 for JDBC databases located on premises) if the data source is accessible through a conventional 100 Mbps LAN. Use higher values for faster networks and lower values for data sources accessible through a WAN.


   
