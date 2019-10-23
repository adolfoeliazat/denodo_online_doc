================================
Transactions in Virtual DataPort
================================

Virtual DataPort allows defining transactions, using the following
clauses:

-  ``BEGIN``. Begins a transaction.
-  ``COMMIT``. Confirms the active transaction.
-  ``ROLLBACK``. Undoes the changes made to the active transaction.

Transactions in Virtual DataPort are distributed by nature. Therefore,
only data sources implementing the Two-Phase-Commit protocol can take
part in them. Most database vendors implement this protocol. In
addition, custom wrappers and stored procedures can also take part in
transactions, provided that the necessary operations to do so are
implemented.

Virtual DataPort imposes certain limits on the duration of the
transactions:

1. A transaction cannot last for more than 30 minutes. The transactions
   that last more time are rollbacked. This limit can be changed by
   executing this command with an administrator account:
   
.. code-block:: vql

   # The command below sets the transaction timeout to an hour (3600 seconds).
   SET 'com.denodo.vdb.engine.session.transactionTimeout' = '3600';

2. A client that starts a transaction cannot be idle for more than 29
   seconds. I.e. once the execution of a statement finishes, the client
   has to execute another statement in less than 30 seconds. Otherwise,
   the transaction is rollbacked.
   
   To change this limit, execute this command with an administrator account:
   
.. code-block:: vql

   # The command below sets the inactivity timeout to a minute (60 seconds).
   SET 'com.denodo.vdb.engine.session.inactiveTransactionTimeout' = '60';

If you do not want a data source to participate in distributed
transactions, set its source configuration property
``supportsDistributedTransactions`` to ``no``.

.. note:: You should use transactions only when you actually need them.
   I.e. you should not surround your queries with ``BEGIN`` and ``COMMIT``
   unless you actually need these queries to be performed inside a
   transaction. The reason is that Virtual DataPort uses a distributed
   transaction manager, which uses a 2-phase commit protocol. This protocol
   introduces some overhead over the queries. Therefore, if you add
   ``BEGIN`` and ``COMMIT`` without needing it, your queries will run
   unnecessarily slower.

-  By default, the ODBC interface of Denodo ignores the requests to start transactions. See more about this in the section :ref:`Disabling Transactions` of the Developer Guide.

-  By default, the JDBC driver of Denodo ignores the invocation to the methods of the JDBC API responsible of managing transactions. This behavior is controlled by the property ``autoCommit``. See more about this property in :ref:`Parameters of the JDBC driver and their default value`.

|

You can connect to the monitoring interface of the server (JMX
interface) to obtain the number of active transactions. See more about
this in the section :ref:`Attributes of the VDBServerManagementInfo MBean` of
the Administration Guide.
