=================
JDBC Data Sources
=================

To create a JDBC data source, use the statement CREATE DATASOURCE JDBC.


.. code-block:: bnf
   :caption: Syntax of the CREATE DATASOURCE JDBC statement
   :name: Syntax of the CREATE DATASOURCE JDBC statement

   CREATE [ OR REPLACE ] DATASOURCE JDBC <name:identifier>
       [ FOLDER = <literal> ]
       [ DRIVERCLASSNAME = <literal> ]
       [ DATABASEURI = <literal> ]
       [
           <credentials>
         | USE_KERBEROS ( <kerberos_credentials> )
         | <credentials> USE_KERBEROS_AT_RUNTIME ( <kerberos_credentials> )
         | <credentials> USE_KERBEROS_AT_INTROSPECTION ( <kerberos_credentials> )
         | WITH PASS-THROUGH SESSION CREDENTIALS ( <pass_through_options> )
         | WITH PASS-THROUGH SESSION CREDENTIALS ( )
               USE_KERBEROS_AT_INTROSPECTION ( <kerberos_credentials> )
       ]
       [ WITH PROXY_CONNECTIONS ]
       [ CLASSPATH = <literal> ]
       [ DATABASENAME = <literal> DATABASEVERSION = <literal>]
       [ ISOLATIONLEVEL =
           {   TRANSACTION_NONE
             | TRANSACTION_READ_COMMITTED
             | TRANSACTION_READ_UNCOMMITTED
             | TRANSACTION_REPEATABLE_READ
             | TRANSACTION_SERIALIZABLE
           }
       ]
       [ IGNORETRAILINGSPACES = { TRUE | FALSE } ]
       [ FETCHSIZE = <integer> ]
       [ BATCHINSERTSIZE = <integer> ]     
       [
           <pool configuration 1>
         | <pool configuration 2>
         | <pool configuration 3>
       ]
       [ PROPERTIES ( <literal> = <literal> [, <literal> = <literal> ]* ) ]
       [ KERBEROSPROPERTIES ( <literal> = <literal>
           [, <literal> = <literal> ]* ) ]
       [ USEEXTERNALTABLES (
             ONMOVEREAD = <boolean>,
             ONMOVEWRITE = <boolean>
         )
       ]
       [ DATAMOVEMENT_TARGET = { true | false } ]
       [ TARGET_CATALOG = <literal> [ ESCAPE ] ]
       [ TARGET_SCHEMA = <literal> [ ESCAPE ] ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ PROCESSING_UNITS = <integer> ]
       [ CPUS_PER_PROCESSING_UNIT = <integer> ]
       [ INTERNAL_TRANSFER_RATE = <double> ]
       [ DESCRIPTION = <literal> ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
                               [, <source configuration property> ]* ] ) ]

   <pass_through_options> ::=
       [ <credentials> ]
       [ USE_KERBEROS ]
       [ PASSTHROUGH_KERBEROS_TARGET_PRINCIPAL = <literal> ]
       [ CONSTRAINED_DELEGATION_PROPERTY_NAME = <literal> ]

   <credentials> ::=
     USERNAME = <literal> USERPASSWORD = <literal> [ ENCRYPTED ]

   <kerberos_credentials> ::=
       KRB_USERNAME = <literal> KRB_USERPASSWORD = <literal> [ ENCRYPTED ]
     | KRB_USERNAME = <literal> KRB_KEYTAB = <literal>

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
     [ <pool sub-configuration 2>]

   <pool sub-configuration 2> ::=
     POOLPREPAREDSTATEMENTS = <boolean>
     MAXSLEEPINGPS = <integer>
     INITIALCAPACITYPS = <integer>

   <source configuration property> ::=
       ALLOWLITERALASPARAMETER = <property value>
     | DELEGATE_BINARY_ORDERBY_COLLATION = <property value>
     | DELEGATE_ORDERBY_COLLATION_MODIFIER = <property value>
     | DELEGATEAGGREGATEFUNCTIONS = {
           DEFAULT
         | ( <function:identifier> [, <function:identifier> ]* ] )
         }
     | DELEGATEALLOPERATORS = <property value>
     | DELEGATEARRAYLITERAL = <property value>
     | DELEGATECOMPOUNDFIELDPROJECTION = <property value>
     | DELEGATEGROUPBY = <property value>
     | DELEGATEHAVING = <property value>
     | DELEGATEINNERJOIN = <property value>
     | DELEGATEINTERSECTION = <property value>
     | DELEGATEINVALIDNUMBERLITERALSASNULL = <property value>
     | DELEGATEJOIN = <property value>
     | DELEGATELEFTFUNCTION = <property value>
     | DELEGATELEFTLITERAL = <property value>
     | DELEGATELITERALEXPRESSION = <property value>
     | DELEGATEMIXEDAGGREGATEEXPRESSION = <property value>
     | DELEGATENATURALOUTERJOIN = <property value>
     | DELEGATENOTCONDITION = <property value>
     | DELEGATE_OFFSET_RESTRICTION = <delegate offset restriction value> 
     | DELEGATEOPERATORSLIST = {
           DEFAULT
         | ( <operator:identifier> [, <operator:identifier> ]* ] )
         }
     | DELEGATEORCONDITION = <property value>
     | DELEGATEORDERBY = <property value>
     | DELEGATEPROJECTION = <property value>
     | DELEGATEREGISTERLITERAL = <property value>
     | DELEGATERIGHTFIELD = <property value>
     | DELEGATERIGHTFUNCTION = <property value>
     | DELEGATERIGHTLITERAL = <property value>
     | DELEGATESCALARFUNCTIONS = {
           DEFAULT
         | ( <function:identifier> [, <function:identifier> ]* ] )
         }
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
     | SUPPORTSPREPAREDSTATEMENT = <property value>
     | SUPPORTSUSINGJOIN = <property value>

   <property value> ::=
       true
     | false
     | DEFAULT

   <delegate offset restriction value> ::= 
       DEFAULT
     | 'NONE'
     | 'FETCH' 
     | 'ORDER_BY'
     | 'FETCH_ORDER_BY'
     | 'NO_ORDER_BY'
     | 'FETCH_NO_ORDER_BY'

To modify a JDBC data source, use ALTER DATASOURCE JDBC.

.. code-block:: bnf
   :caption: Syntax of the ALTER DATASOURCE JDBC statement
   :name: Syntax of the ALTER DATASOURCE JDBC statement

   ALTER DATASOURCE JDBC <name:identifier>
       [ DRIVERCLASSNAME = <literal> ]
       [ DATABASEURI = <literal> ]
       [
           <credentials>
         | USE_KERBEROS ( <kerberos_credentials> )
         | <credentials> USE_KERBEROS_AT_RUNTIME ( <kerberos_credentials> )
         | <credentials> USE_KERBEROS_AT_INTROSPECTION ( <kerberos_credentials> )
         | WITH PASS-THROUGH SESSION CREDENTIALS ( <pass_through_options> )
         | WITH PASS-THROUGH SESSION CREDENTIALS ( ) USE_KERBEROS_AT_INTROSPECTION ( <kerberos_credentials> )
         [ WITH PROXY_CONNECTIONS ]
       ]
       [ CLASSPATH = <literal> ]
       [
         DATABASENAME = <literal>
         DATABASEVERSION = <literal>
       ]
       [ ISOLATIONLEVEL =
           TRANSACTION_NONE
         | TRANSACTION_READ_COMMITTED
         | TRANSACTION_READ_UNCOMMITTED
         | TRANSACTION_REPEATABLE_READ
         | TRANSACTION_SERIALIZABLE
       ]
       [ IGNORETRAILINGSPACES = { true | false } ]
       [ FETCHSIZE = <integer> ]
       [ BATCHINSERTSIZE = <integer> ]
       [ 
           <pool configuration 1>
         | <pool configuration 2>
         | <pool configuration 3>
       ]
       [ PROPERTIES ( <literal> = <literal> [, <literal> = <literal> ]* ) ]
       [ KERBEROSPROPERTIES ( <literal> = <literal> [, <literal> = <literal> ]* ) ]
       [ USEEXTERNALTABLES (
             ONMOVEREAD = <boolean>,
             ONMOVEWRITE = <boolean>
         )
       ]
       [ DATAMOVEMENT_TARGET = { true | false } ]
       [ TARGET_CATALOG = <literal> [ ESCAPE ] ]
       [ TARGET_SCHEMA = <literal> [ ESCAPE ] ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ PROCESSING_UNITS = <integer> ]
       [ CPUS_PER_PROCESSING_UNIT = <integer> ]
       [ INTERNAL_TRANSFER_RATE = <double> ]
       [ DESCRIPTION = <literal> ]
       [ SOURCECONFIGURATION ( [ <source configuration property>
                               [, <source configuration property> ]* ] ) ]
..

   <pool configuration 1> ::= (see :ref:`CREATE DATASOURCE JDBC <Syntax of the CREATE DATASOURCE JDBC statement>`)
         
   <pool configuration 2> ::= (see :ref:`CREATE DATASOURCE JDBC <Syntax of the CREATE DATASOURCE JDBC statement>`)
         
   <pool configuration 3> ::= (see :ref:`CREATE DATASOURCE JDBC <Syntax of the CREATE DATASOURCE JDBC statement>`)
   
   <pool sub-configuration 1> ::= (see :ref:`CREATE DATASOURCE JDBC <Syntax of the CREATE DATASOURCE JDBC statement>`)
         
   <pool sub-configuration 2> ::= (see :ref:`CREATE DATASOURCE JDBC <Syntax of the CREATE DATASOURCE JDBC statement>`)
   
   <source configuration property> ::= (see :ref:`CREATE DATASOURCE JDBC <Syntax of the CREATE DATASOURCE JDBC statement>`)
   
   <credentials> ::= (see :ref:`CREATE DATASOURCE JDBC <Syntax of the CREATE DATASOURCE JDBC statement>`)

..

Explanation of some of the parameters of these statements:  

-  ``OR REPLACE``: If present and a data source with the same name exists,
   the current definition is substituted with the new one.

-  ``FOLDER``: name of the folder where the data source will be stored.

-  ``DRIVERCLASSNAME``: The driver class to be used for connection to the
   data source.

-  ``DATABASEURI``: The connection URL to the database.

-  The authentication methods available to connect to a database are the
   following:

   a. ``<credentials>``: provide the ``USERNAME`` and ``PASSWORD`` to
      connect to the database to execute queries and for the introspection
      process (i.e. to display the tables/views of the database in the
      “Create base view” dialog of the data source).
   b. ``USE_KERBEROS ( <kerberos_credentials> )``: use Kerberos to connect
      to the database to execute queries and for the introspection process
      (i.e. to display the tables/views of the database in the “Create base
      view” dialog of the data source).
   c. ``<credentials> USE_KERBEROS_AT_RUNTIME ( <kerberos_credentials> )``:
      use Kerberos to connect to the database to execute queries, but login
      and password for the introspection process.
   d. ``<credentials> USE_KERBEROS_AT_INTROSPECTION ( <kerberos_credentials> )``:
      use login and password to connect to the database to execute queries,
      but use Kerberos for the introspection process.
   e. ``WITH PASS-THROUGH SESSION CREDENTIALS ( <pass_through_options> )``: use
      login and password for the introspection process and the credentials
      of the client that connected to the Virtual DataPort server to
      execute queries. The credentials used to run queries can be Kerberos
      or login/password depending on the authentication the client used to
      connect to the Virtual DataPort server. If ``<pass_through_options>``
      specifies ``USE_KERBEROS``, the Server will use the login/password to
      create the Kerberos ticket.
      
      If you create a data source with this option, but you want to
      query a view of this data source with other credentials than the ones
      used to connect to the Server, add the parameters ``USERNAME``
      and ``PASSWORD`` to the ``CONTEXT``. These two parameters are only
      taken into account when the data source has been created with the option
      ``WITH PASS-THROUGH SESSION CREDENTIALS``.
      
      For example, if ``view1`` has been created with the option ``WITH PASS-THROUGH SESSION CREDENTIALS`` and you
      execute this:
      
      .. code-block:: vql
      
         SELECT * 
         FROM view1
         CONTEXT(
             USERNAME = 'admin'
           , PASSWORD = 'd4GvpKA5BiwoGUFrnH92DNq5TTNKWw58I86PVH2tQIs/q1RH9CkCoJj57NnQUlmvgvvVnBvlaH8NFSDM0x5fWCJiAvyia70oxiUWbToKkHl3ztgH1hZLcQiqkpXT/oYd' ENCRYPTED
           , DOMAIN = 'ACME_DOMAIN')
      
      the Server will connect to the Web service with the username
      ``admin``, password ``password`` and domain
      ``acme_domain``, ignoring the credentials used by the user to
      connect to the Server.
      
      It is mandatory to add the token ``ENCRYPTED`` and enter the password encrypted. To encrypt the password, execute the statement ``ENCRYPT_PASSWORD``. For example:
      
      .. code-block:: vql
      
         ENCRYPT_PASSWORD 'my_secret_password';

      
   f. ``WITH PASS-THROUGH SESSION CREDENTIALS () USE_KERBEROS_AT_INTROSPECTION ( <kerberos_credentials> )``:
      use Kerberos authentication for the introspection process and the
      credentials of the client that connected to the Virtual DataPort
      server to execute queries. The credentials used to run queries can be
      Kerberos or login/password depending on the authentication the client
      used to connect to the Virtual DataPort server.

   .. important:: There are important implications of using “pass-through
      session credentials”. To read about them, search “pass-through
      credentials” on the section :ref:`Importing JDBC Sources` of the
      Administration Guide.

-  ``WITH PROXY_CONNECTIONS``: if present, the data source will use the feature "proxy authentication" of Oracle. The section :ref:`How Oracle Proxy Authentication Works` of the Administration Guide explains how this feature works.

-  ``CLASSPATH``: Path to the JAR file containing the JDBC driver for the
   specified source (optional).

-  ``DATABASENAME`` and ``DATABASEVERSION``: Name and version of the
   database to be accessed.

-  ``ISOLATIONLEVEL``: sets the desired isolation level for the queries and
   transactions executed in the database. If not present, the data source
   uses the default isolation level of the database.

-  ``IGNORETRAILINGSPACES``: If ``true``, the Server removes the space
   characters at the end of ``text`` type values of the results returned by
   these data source’s views.

-  ``FETCHSIZE``: gives the JDBC driver a hint as to the number of rows
   that should be fetched from the database when more rows are needed.

-  ``BATCHINSERTSIZE``: when the data source has to insert several rows
   into the database of this data source, it can insert them in batches.
   This number sets the number of queries per batch.

   This value is used only when inserting rows into the database of this
   data source as a result of moving data from another data source into
   this one. See more about this in the section :ref:`Data Movement` of the
   Administration Guide.

-  The section :ref:`The Pool of Connections of the JDBC Data Sources` below explains the parameters of "<pool configuration 1>", "<pool configuration 2>" and "<pool configuration 3>".

-  Parameters of the pool of stored procedures:

   -  ``POOLPREPAREDSTATEMENTS``: if ``true``, the pool of prepared
      statements is enabled.
   -  ``INITIALCAPACITYPS``: initial size of the pool of prepared
      statements. Only useful if ``POOLPREPAREDSTATEMENTS`` is set to
      ``true``.
   -  ``MAXSLEEPINGPS``: maximum number of idle prepared statements in the
      pool of prepared statements. Only useful if
      ``POOLPREPAREDSTATEMENTS`` is set to ``true``.


-  ``USEEXTERNALTABLES``: options regarding the use of the database’s
   proprietary APIs to read and write data from/to this database.

   Writing data into a database using its proprietary API is called “Bulk
   data load”.

   There are two reasons that the Server may write data using the bulk data
   load APIs of the database:

   a. When performing a data movement. The section :ref:`Data Movement` of the
      Administration Guide explain what they are.
   b. When loading the cache of a table.

   The section :ref:`Bulk data load` of the Administration Guide explains in
   detail how this process works.

   -  ``ONMOVEREAD`` (only taken into account when the database is
      Netezza): if ``true``, when the Execution Engine reads data from the
      Netezza database to perform a data movement, it will do so using its
      “External tables” feature.
      Setting this to ``true`` is equivalent to selecting the check box
      “Use external tables for data movement” of the “Read settings”, on
      the “Read & Write” tab of the data source.
   -  ``ONMOVEWRITE``: if ``true``, when the Execution Engine writes data
      to this database to perform a data movement, it does so using its
      proprietary API. Setting this to ``yes`` is equivalent to selecting
      the check box “Use Bulk Data Load APIs” of the “Write settings”, on
      the “Read & Write” tab of the data source.

-  ``DATAMOVEMENT_TARGET``: if ``true``, the data source can be the target
   of a data movement. Setting this to ``true`` is equivalent to
   selecting the check box “Can be data movement target” of the “Write
   settings”, on the “Read & Write” tab of the data source.

-  ``BULK_LOAD_CONFIGURATION``: settings of the bulk data load API of the
   database. The settings you can indicate depend on the database adapter
   so it is better to change them from the administration tool.

-  Data source configuration parameters (``SOURCECONFIGURATION``). Virtual
   DataPort allows indicating specific characteristics of the underlying
   data sources, so that they are taken into account when executing
   statements on them. See section :doc:`/vdp/vql/generating_wrappers_and_data_sources/creating_data_sources/data_source_configuration_properties`
   for further details.

-  ``PROPERTIES``: list of name/value pairs that will be passed to the JDBC
   driver when creating connection with this database.

-  ``KERBEROSPROPERTIES``: list of name/value pairs that will be passed to
   the JDBC driver when creating connection with this database. The
   properties on this list are meant to configure the Kerberos
   authentication mechanism between the Virtual DataPort server and the
   database. See the section :ref:`Connecting to a JDBC Source with Kerberos
   Authentication` of the Administration Guide for more details about
   Kerberos authentication in JDBC data sources.


-  The following optional parameters specify information about the data source. The cost optimizer uses these values when evaluating the cost of an execution plan. The default values are usually correct so you should not specify these parameters unless you have a deep knowledge of the cost optimizer. 

   -  ``TRANSFER_RATE_FACTOR``: relative measure of the speed of the network connection between the Denodo server and the data source. Use the default value (1) if the data source is accessible through a conventional 100 Mbps LAN. Use higher values for faster networks and lower values for data sources accessible through a WAN.

   -  ``PROCESSING_UNITS``: In parallel databases, the number of SPUs.
   
   -  ``CPUS_PER_PROCESSING_UNIT``: In parallel databases, the number of CPUs per SPU.
   
   -  ``INTERNAL_TRANSFER_RATE``: transfer rate in kilobytes per millisecond.


The Pool of Connections of the JDBC Data Sources
================================================

The JDBC data sources of Denodo are created with a connection pool. A connection pool is a cache of connections to a database. At run time, when a query sent by an application involves sending a query to this data source, the execution engine requests a connection to the pool of the data source. If there are idle connection in the pool, it returns one. If there are no idle connections, the pool creates one. Then, the execution engine executes the query on this connection and once the query finishes, it  returns the connection back to the pool. This connection is now available for the next query.

Using a connection pool significantly improves the performance of the queries involving a database. The reason is that creating a new connection is costly in terms of time and resources. For each new connection, Denodo has to establish a network connection with the database, the database has to authenticate the client, allocate new memory to the new connection, etc. If the connection is already created, this process is avoided.

Connection pools provide the following benefits:

-  Reduce the number of times a new connection has to be created.

-  Reduce the time of getting a connection to execute a query.  

-  Provide a way of limiting the amount of connections opened to a database.

These are the reasons new JDBC data sources have a connection pool and in general, it should not be disabled.

The exact behavior of the connection pool depends on its settings.


Parameters of the Connection Pool of a JDBC Data Source
-------------------------------------------------------

The connection pool of a JDBC data source is configured with the following parameters. Most of them cannot be changed from the administration tool, only with the statement ``ALTER DATA SOURCE JDBC``. 

-  ``MAXACTIVE`` (default value: 20): maximum number of connections that can be opened at a given time. These are the connections currently used to run queries plus the connections idle in the pool. 

   You can change this parameter graphically, in the dialog in the *Connection Pool Configuration*, field *Maximum number of active connections*. 

   -  If negative (e.g. -1), there is no limit to the number of connections in the pool at one time.
   -  Enter 0 to disable the connection pool of the data source. In this case, the other parameters of the connection pool are ignored.
   
      In this case, for each query, the data source opens a new connection to the database and once the query finishes, it closes the connection. If the query is part of a transaction (the client executed ``BEGIN`` earlier but did not execute ``COMMIT`` or ``ROLLBACK`` yet), the execution engine keeps the connection open until the transaction finishes. That is because as transactions in Denodo are distributed, the transaction needs to be confirmed or rolledback in all the databases involved in the query. See more about transactions in the section :ref:`Transactions in Virtual DataPort`.

   ``MAXACTIVE`` is ignored if ``EXHAUSTEDACTION`` is set to 2 (see the explanation below).

-  ``MAXIDLE`` (default value: -1): maximum number of connections that can sit idle in the pool at any time.

   -  If -1 or lower, there is no limit to the number of connections that may sit idle at one time.
   -  If 0 or higher and the parameters ``TIMEBETWEENEVICTION`` and ``MINEVICTABLETIME`` are greater than 0, the connections that have been idle for more than a certain time will be closed.

-  ``MINIDLE`` (default value: 0): minimum number of connections that are idle in the pool of connections. Useful to guarantee that there are always idle connections in the pool so a query never has to wait for the pool to open a new connection. In order to satisfy ``MINIDLE``, the pool will never create more than ``MAXACTIVE`` connections.

-  ``EXHAUSTEDACTION`` (default value: 1): specifies the behavior of the pool when the pool is empty (all the connections are running queries). The possible values are:

   -  ``0``: the data source returns an error and the query will fail.
   -  ``1``: the pool waits until a connection is available,
      or the maximum waiting time is reached. If the maximum wait time is reached, the query fails.
         
      The default maximum wait time is 30 seconds but it can be changed. If the maximum wait time is ``-1``, the data source will wait indefinitely until a connection is available. To change this property, execute the following command from the VQL Shell and restart the Virtual DataPort server.  
         
      .. code-block:: vql
         
         -- This represents 20 seconds because the value of this property is in milliseconds.
            
         SET 'com.denodo.vdb.misc.datasource.JDBCDataSource.pool.maxWait' = '20000';
            
      After changing the maximum wait time and restarting, the existing JDBC data sources will still have the same maximum wait time but if they are modified, their maximum wait time will change to the new value.
   
   -  ``2``: the data source will create a new connection if there are no idle connections. This makes ``MAXACTIVE`` meaningless.

-  ``INITIALSIZE`` (default value: 4): number of connections with which the pool is initialized.
   
   This value is ignored when the data source is created with the authentication option “Pass-through session
   credentials”. With this option, the
   Server will create one pool of connections for each user account that connects to this database
   and initially, these pools will only have one connection regardless of the value of ``INITIALSIZE``. This is to prevent creating too many
   unnecessary connections.


-  ``VALIDATIONQUERY`` (default value: depends on the adapter): SQL query executed by the connection pool to check
   if a connection is still valid; also known as "ping query". It is only used when at least one of ``TESTONBORROW``, ``TESTONRETURN`` or  ``TESTWHILEIDLE`` are ``true``.
   
   You can change this parameter graphically, in the dialog in the *Connection Pool Configuration*, field *Ping query*. 

   When you create a JDBC data source using the administration tool and select a database adapter other than *Generic*, the data source is automatically created with the appropriate validation query for that database. However, if you select *Generic*, you have to provide a valid ping query. This query is executed often so its footprint has to be as low as
   possible. In addition, the table queried by the pool has to exist and
   the user needs to have privileges to run it.   
   
   Examples of ping queries:

   .. code-block:: sql

      SELECT 1
      SELECT 1 FROM dual
      SELECT 1 FROM view LIMIT 0
   
   The ping query can contain the interpolation variable ``USER_NAME``. At
   runtime, this variable will be replaced with the user name of the
   Virtual DataPort user that runs the query.

   For example, let us say that the ping query of a JDBC data source is
   ``CALL DBMS_SESSION.SET_IDENTIFIER('@{USER_NAME}')``.

   If the user ``scott`` executes a query that involves this data source,
   Virtual DataPort will execute the query
   ``CALL DBMS_SESSION.SET_IDENTIFIER('scott')`` over the connection
   returned by the pool, to check that this connection is still valid and
   not stale.

   If ``TESTONBORROW`` is true, the ping query is executed every time before the actual query is sent
   to the database. Being able to put in this query the username that is currently running the query can be useful
   to execute a statement on the database or for auditing purposes.

-  ``TESTONBORROW`` (default value: true): if true and the parameter ``VALIDATIONQUERY`` is not empty, the pool will execute the ``VALIDATIONQUERY`` on the selected connection before returning it to the execution engine. If the ``VALIDATIONQUERY`` fails, the pool will discard the connection and select another one. If there are no more idle connections, it will create one.

-  ``TESTONRETURN`` (default value: false): if true and the parameter ``VALIDATIONQUERY`` is not empty, the pool will execute the ``VALIDATIONQUERY`` on the connections returned to the pool. If the ``VALIDATIONQUERY`` fails, the pool will discard the connection.

| 

The pool can be configured to periodically check that the connections in the pool are still valid and/or to close the connections that have sat idle in the pool for more than a certain period of time.
To check if a connection is still valid, the pool executes the ``VALIDATIONQUERY``. If it fails, the pool discards the connection. The main reasons for the validation query to fail are:

a. The database has a mechanism that closes connections after a certain period of inactivity.
#. A firewall placed in between the Denodo server and the database automatically closes any connection after a certain period of inactivity.

|

The main goals of enabling this option are: 

1.  Make sure that the connections of the pool are valid at all times. That way, when a connection is needed, the connections of the pool are always valid.
2.  Close connections that have not been used in a while thus freeing resources in the database.

The task of verifying connections and closing the idle ones is performed by the "connection eviction thread" (there is one per pool of connections). Periodically, this thread awakes and perform these checks. 

The parameters that control the connection eviction thread are:

-  ``TIMEBETWEENEVICTION`` (default value: -1): how long in milliseconds the eviction thread should sleep before "runs" of examining idle connections. If negative, the eviction thread is not launched. 

   Eviction runs contend with the execution engine for access to the pool, so if this thread runs too frequently, performance issues may result. 
   
-  ``MINEVICTABLETIME`` (default value: 1800000 - 30 minutes): minimum amount of time in milliseconds that a
   connection may sit idle in the pool before it is eligible for eviction.
   
   When is less than 1, no connections will be evicted from the pool due to
   idle time. In this case, the eviction thread will only discard connections that return an error when executing the validation query.
   
   Even if a connection has been idle for more than ``MINEVICTABLETIME``, it will not be closed if the number of idle connections was less than ``MINIDLE`` after closing that connection.
   
   This parameter has no effect if ``TIMEBETWEENEVICTION`` is less than 1.
      
-  ``TESTWHILEIDLE`` (default value: false): if true and the parameter ``VALIDATIONQUERY`` is not empty, the connections will be validated by the
   connection eviction thread. To validate a connection, the thread runs the validation query on the connection. If it fails, the pool drops the connection from the pool.
   
   This parameter has no effect if ``TIMEBETWEENEVICTION`` is less than 1.
 
-  ``NUMTESTPEREVICTION`` (default value: 3): number of connections examined in each run of the connection eviction thread. The reason for not  examining all the connections of the pool in each run of the thread is because while a connection is being examined, it cannot be used by a query.

   This parameter has no effect if ``TIMEBETWEENEVICTION`` is less than 1.
 
Recommended Settings of the Connection Pool in Environments with a Firewall
---------------------------------------------------------------------------

If there is a firewall between the host where the Denodo server runs and the database, and the firewall is configured to close inactive connections after a certain period of inactivity, you must enable the "connection eviction" thread of that data source. By doing that, the connection pool will periodically execute the validation query, which will maintain the connections alive. Otherwise, the firewall will close these connections. When a firewall closes a connection, it does not usually notify the participants of that connection that the connection is going to be closed. The problem is that if the pool executes the validation query on a connection that has already been dropped by the firewall, the pool still thinks this is a valid connection and it may take several minutes until the query fails with a timeout. During this time, the execution engine will be waiting for the pool to return a connection. For this reason, it is better to configure the eviction thread to run and test the idle connections. 

|

To enable the connection eviction thread, follow these steps:

1. In the administration tool, open the VQL Shell.

2. Execute ``DESC VQL DATASOURCE JDBC <name of the data source>;``.

3. Copy the part that starts with ``VALIDATIONQUERY`` up until ``MINEVICTABLETIME``.

4. Run ``ALTER DATASOURCE JDBC <name of the data source>`` followed by what you copied in the previous step, with these changes:

   -  Set ``TESTWHILEIDLE`` to ``true`` 
   -  Set ``TIMEBETWEENEVICTION`` to ``300000`` (five minutes)

   For example,
   
.. code-block:: vql 
   
   ALTER DATASOURCE JDBC ds_jdbc_oracle_finance
       VALIDATIONQUERY = 'Select 1'
       INITIALSIZE = 4
       MAXIDLE = -1
       MINIDLE = 0
       MAXACTIVE = 20
       EXHAUSTEDACTION = 1
       TESTONBORROW = true
       TESTONRETURN = false
       TESTWHILEIDLE = true
       TIMEBETWEENEVICTION = 300000
       NUMTESTPEREVICTION = 3
       MINEVICTABLETIME = 1800000;
      
With this change, every five minutes (300,000 milliseconds), the pool will examine three connections that are idle from the pool (``NUMTESTPEREVICTION``). For each one of these three connections, the pool will do the following:

-  The pool will close a connection if it has been idle for more than 30 minutes (1,800,000 seconds).
-  If the connection has been idle for less time, it will run the query ``Select 1`` (``VALIDATIONQUERY``) on this connection. If the validation query fails, it will drop this connection.   

|

Another alternative for when the firewall closes inactive connections is to disable the pool of connections. However, this has performance implications because when doing this, instead of reusing the connections, the data source will have to open a new connection for each query instead of reusing them. This will increase the execution time of all the queries.

To disable the connection pool of a data source, follow the same steps as above, set the parameter ``MAXACTIVE`` of the data source to 0.
