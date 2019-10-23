=====================================
Parameters of the JDBC Connection URL
=====================================

The table `Parameters of the JDBC driver and their default value`_ lists
the optional configuration parameters of the URL and their default
value:

.. table:: Parameters of the JDBC driver and their default value
   :name: Parameters of the JDBC driver and their default value 

   +--------------------------------------+--------------------------------------+
   | Parameter of the URL                 | Description                          |
   +======================================+======================================+
   | autoCommit                           | If ``true``, the invocations to the  |
   |                                      | methods of the JDBC API responsible  |
   |                                      | of managing transactions are         |
   |                                      | ignored. I.e. the driver ignores the |
   |                                      | invocations to the methods           |
   |                                      | ``setAutoCommit(...)``, ``commit()`` |
   |                                      | and ``rollback()``.                  |
   |                                      |                                      |
   |                                      | This is useful to make sure that an  |
   |                                      | application does not start           |
   |                                      | transactions inadvertently.          |
   |                                      |                                      |
   |                                      | Even with this parameter set to      |
   |                                      | true, an application can start and   |
   |                                      | finish transactions by executing the |
   |                                      | statements ``BEGIN``, ``COMMIT`` and |
   |                                      | ``ROLLBACK``.                        |
   |                                      |                                      |
   |                                      | If the client invokes                |
   |                                      | ``setAutoCommit(false)``, after      |
   |                                      | executing a ``COMMIT``, the driver   |
   |                                      | will not start a new transaction     |
   |                                      | until it has to execute another      |
   |                                      | statement.                           |
   |                                      |                                      |
   |                                      | If the client invokes                |
   |                                      | ``setAutoCommit(false)``, take into  |
   |                                      | account the limits on the duration   |
   |                                      | of a transaction:                    |
   |                                      |                                      |
   |                                      | -  By default, transactions cannot   |
   |                                      |    last for more than 30 minutes.    |
   |                                      | -  Once the execution of a statement |
   |                                      |    finishes, the client has to       |
   |                                      |    execute another statement in less |
   |                                      |    than 30 seconds.                  |
   |                                      |                                      |
   |                                      | See more about the limits on the     |
   |                                      | duration of transaction in the       |
   |                                      | section :ref:`Transactions in        |
   |                                      | Virtual DataPort` of the VQL Guide.  |
   |                                      |                                      |
   |                                      | Default value: ``true``.             |
   +--------------------------------------+--------------------------------------+
   | chunkSize                            | The results of a query can be        |
   |                                      | divided into blocks (chunks), so the |
   |                                      | Server does not have to wait for the |
   |                                      | query to finish, in order to begin   |
   |                                      | sending part of the results to the   |
   |                                      | client.                              |
   |                                      |                                      |
   |                                      | This parameter establishes the       |
   |                                      | maximum number of results that a     |
   |                                      | block can contain. When the          |
   |                                      | Server obtains enough results to     |
   |                                      | complete a block, it sends this      |
   |                                      | block to the driver and continues    |
   |                                      | processing the next results.         |
   |                                      |                                      |
   |                                      | In an application that uses this     |
   |                                      | driver, you can either add this      |
   |                                      | parameter to the connection URL      |
   |                                      | and/or before executing the query,   |
   |                                      | invoke the method ``setFetchSize``   |
   |                                      | of the class ``Statement``. The      |
   |                                      | value set with the ``setFetchSize``  |
   |                                      | method overrides the value set in    |
   |                                      | the URL.                             |
   |                                      |                                      |
   |                                      | Default value: ``1000``              |
   +--------------------------------------+--------------------------------------+
   | chunkTimeout                         | This parameter establishes the       |
   |                                      | maximum time (in milliseconds) the   |
   |                                      | Server waits before returning a new  |
   |                                      | block to the driver. When this time  |
   |                                      | is exceeded, the Server sends the    |
   |                                      | current block to the driver, even if |
   |                                      | it does not contain the number of    |
   |                                      | results specified in the             |
   |                                      | ``chunkSize`` parameter.             |
   |                                      |                                      |
   |                                      | **Note**: if ``chunkSize`` and       |
   |                                      | ``chunkTimeout`` are ``0``, the      |
   |                                      | Server returns all the results in a  |
   |                                      | single block. If both values are     |
   |                                      | different than ``0``, the Server     |
   |                                      | returns a chunk whenever one of      |
   |                                      | these conditions happen first:       |
   |                                      |                                      |
   |                                      | -  The chunk is filled               |
   |                                      |    (``chunkSize``)                   |
   |                                      | -  Or, after a certain time of not   |
   |                                      |    sending any chunk to the client   |
   |                                      |    (``chunkTimeout``)                |
   |                                      |                                      |
   |                                      | Default value: ``90000``             |
   |                                      | milliseconds (90 seconds)            |
   +--------------------------------------+--------------------------------------+
   | describeNationalCharTypesAsBasicTypes| If ``true``, the driver reports the  |
   |                                      | field types NCHAR, NVARCHAR, NCLOB   |
   |                                      | and LONGNVARCHAR as CHAR, VARCHAR,   |
   |                                      | CLOB and LONGVARCHAR respectively.   |
   |                                      |                                      |
   |                                      | Default value: ``false``             |
   +--------------------------------------+--------------------------------------+
   |  i18n                                | This parameter establishes the       |
   |                                      | internationalization (i18n)          |
   |                                      | configuration of the connection with |
   |                                      | the Server.                          |
   |                                      |                                      |
   |                                      | If not present, the driver assumes   |
   |                                      | the i18n of the database that you    |
   |                                      | are connecting to.                   |
   |                                      |                                      |
   |                                      | The parameter ``i18n`` in the        |
   |                                      | ``CONTEXT`` clause of the queries    |
   |                                      | overrides the value of this          |
   |                                      | parameter.                           |
   |                                      |                                      |
   |                                      | Default value: I18N of the database  |
   |                                      | that you are connecting to           |
   +--------------------------------------+--------------------------------------+
   | identifiersUppercase                 | If ``true``, when executing          |
   |                                      | ``SELECT`` queries, the names of the |
   |                                      | fields are returned in uppercase.    |
   |                                      |                                      |
   |                                      | The default value is ``false``.      |
   |                                      |                                      |
   |                                      | Default value: ``false``             |
   +--------------------------------------+--------------------------------------+
   | password                             | Password used to authenticate        |
   |                                      | against Virtual DataPort. When using |
   |                                      | this parameter, also provide the     |
   |                                      | value of the parameter ``user``.     |
   |                                      |                                      |
   |                                      | Default value: N/A                   |
   +--------------------------------------+--------------------------------------+
   | publishViewsAsTables                 | If ``false``, the metadata published |
   |                                      | by the JDBC driver describes base    |
   |                                      | views as TABLE elements and the      |
   |                                      | derived and interface views as VIEW  |
   |                                      | elements.                            |
   |                                      |                                      |
   |                                      | If ``true``, the metadata describes  |
   |                                      | all the views as TABLE elements.     |
   |                                      |                                      |
   |                                      | Some third-party tools require the   |
   |                                      | JDBC metadata to publish all the     |
   |                                      | views as tables in order to          |
   |                                      | recognize the associations created   |
   |                                      | between views. For these             |
   |                                      | applications, add this parameter to  |
   |                                      | the URL with the value ``true``.     |
   |                                      |                                      |
   |                                      | Default value: ``false``             |
   +--------------------------------------+--------------------------------------+
   | queryTimeout                         | Maximum time (in milliseconds) the   |
   |                                      | driver will wait for a query to      |
   |                                      | finish. After this period, it will   |
   |                                      | throw an Exception.                  |
   |                                      |                                      |
   |                                      | This parameter is optional. If it is |
   |                                      | not set, the query timeout has the   |
   |                                      | default value (900000 milliseconds). |
   |                                      | If 0, the driver will wait           |
   |                                      | indefinitely until the query         |
   |                                      | finishes.                            |
   |                                      |                                      |
   |                                      | This parameter sets the default      |
   |                                      | timeout for all the queries. In      |
   |                                      | addition, you can change the timeout |
   |                                      | for a single query by adding the     |
   |                                      | parameter 'QUERYTIMEOUT' = '<value>' |
   |                                      | to the CONTEXT clause of the query.  |
   |                                      | See more about this in the section   |
   |                                      | :ref:`CONTEXT Clause` of the         |
   |                                      | VQL Guide.                           |
   |                                      |                                      |
   |                                      | Default value: ``900000``            |
   |                                      | milliseconds (15 minutes)            |
   +--------------------------------------+--------------------------------------+
   | reuseRegistrySocket,                 | Parameters needed when connecting to |
   | pingQuery and                        | Virtual DataPort through a load      |
   | pingQueryTimeout                     | balancer. The section                |
   |                                      | :ref:`Connecting to Virtual DataPort |
   |                                      | Through a Load Balancer` explains    |
   |                                      | how to use them.                     |                                     
   |                                      |                                      |
   |                                      | Default value: None                  |
   +--------------------------------------+--------------------------------------+
   |  ssl                                 | By default, the driver tries to      |
   |                                      | establish a non-SSL connection with  |
   |                                      | the Server. If SSL is enabled on the |
   |                                      | Server, the connection fails and     |
   |                                      | immediately, the driver tries to     |
   |                                      | establish an SSL connection.         |
   |                                      |                                      |
   |                                      | If ``true``, the driver only         |
   |                                      | establishes SSL connections. If SSL  |
   |                                      | is not enabled on the Server, the    |
   |                                      | connection will fail.                |
   |                                      |                                      |
   |                                      | If ``false``, the driver only        |
   |                                      | establishes non-SSL connections, it  |
   |                                      | does not try to establish SSL        |
   |                                      | connections. If SSL is enabled on    |
   |                                      | the Server, the connection will fail.|                                       
   |                                      |                                      |
   |                                      | Default value: None                  |
   +--------------------------------------+--------------------------------------+
   | user                                 | User name used to authenticate       |
   |                                      | against Virtual DataPort. When using |
   |                                      | this parameter, also provide the     |
   |                                      | value of the parameter ``password``. |
   |                                      |                                      |
   |                                      | Default value: N/A                   |
   +--------------------------------------+--------------------------------------+
   | userAgent                            | Sets the user agent of the           |
   |                                      | connection. The section “Setting the |
   |                                      | User Agent of an Application” of the |
   |                                      | Administration Guide explains why we |
   |                                      | recommend setting this parameter.    |
   |                                      |                                      |
   |                                      | Default value: <empty>               |
   +--------------------------------------+--------------------------------------+
   | userGSSCredential                    | Java object of the class             |
   |                                      | ``org.ietf.jgss.GSSCredential``.     |
   |                                      |                                      |
   |                                      | Use this driver property to pass     | 
   |                                      | a Kerberos                           |
   |                                      | credential, which the driver will    |
   |                                      | use to connect to Denodo.            |
   |                                      |                                      |
   |                                      | ``userGSSCredential`` has to be a    |
   |                                      | driver property,                     |
   |                                      | it cannot be passed as a parameter   |
   |                                      | of the URL.                          |
   |                                      |                                      |
   |                                      | See more about this property in the  |
   |                                      | section :ref:`Connecting to Virtual  |
   |                                      | DataPort Using Kerberos              |
   |                                      | Authentication`.                     |
   |                                      |                                      |
   |                                      | Default value: none.                 |
   +--------------------------------------+--------------------------------------+
   | wanOptimized                         | If ``true``, the driver enables      |
   |                                      | several features that reduce the     |
   |                                      | latency in the communications between|
   |                                      | the client application and Virtual   |
   |                                      | DataPort server.                     |
   |                                      |                                      |
   |                                      | Default value: ``false``             |
   +--------------------------------------+--------------------------------------+

.. note:: You can indicate all these options either as a parameter of the connection URL or as a driver property,
   except ``userGSSCredential`` that has to be passed as a driver property, not a URL parameter.

.. rubric:: Autocommit Property

By default, the connections opened by the Denodo JDBC driver have the
property “autocommit” set to ``true``. This is the recommended value and
its effect is that the queries are not performed inside a transaction.

You should not change this property to ``false`` unless you need the
statements to be executed inside the same transaction. The reason is
that Virtual DataPort uses a distributed transaction manager, which uses
a 2-phase commit protocol. This protocol introduces some overhead over
the queries. Therefore, if you set this property to ``false`` without
needing it, your queries will run unnecessarily slower.
