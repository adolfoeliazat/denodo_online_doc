======================================
Information and Events on Transactions
======================================

The ``TransactionsManagementInfo`` MBeans provide information about the
transactions executed in a database of the Server.

These MBeans are located in ``com.denodo.vdb.management.mbeans`` >
``TransactionsManagementInfo`` > *database name*.

There is an MBean of this type for each database and they have the
following attributes:


-  ``DatabaseName``. Name of the Virtual DataPort database.


-  ``MaxTransactions``. Maximum number of transactions exported at the same
   time in the MBean. Each transaction appears as an attribute in the form
   ``Transaction<i>``.


-  ``TotalTransactions``. Total number of executed transactions since the
   start of the server.


-  ``ActiveTransactions``. Number of currently active requests.


-  ``Transaction <i>``. Contains information about the transaction #i. Its
   subproperties are:

   -  ``Autostarted``. Indicates whether the transaction has been
      explicitly created by the user. All statements modifying the Virtual
      DataPort catalog must be run within a transaction. Therefore, if the
      user has not created it, Virtual DataPort will create it itself and, in this
      case, the value of this property will be false.
   -  ``DatabaseName``. Name of the Virtual DataPort database on which the
      transaction is run.
   -  ``EndTime``. Moment at which the transaction ended.
   -  ``Identifier``. Transaction ID.
   -  ``SessionID``. Session identifier from which the transaction was
      started.
   -  ``StartTime``. Moment at which the transaction started.
   -  ``State``. This indicates the status with which the transaction
      ended. It can take the following values: ``ROLLBACK`` or ``COMMIT``.
   -  ``UserName``. ID of the user running the transaction.


It is possible to subscribe to the events of the
``TransactionsManagementInfo`` MBean. In that case, every time a
transaction starts or ends on the specified database, a notification
with the following data is received:

-  ``Timestamp``. Moment at which the notification is generated in the
   JMX server.
-  ``Type``. This type of notification takes the string
   ``startTransaction`` or ``endTransaction`` as the type indicator,
   depending on whether the notification indicates the start or the end
   of a transaction.
-  ``UserData``. Compound element. Its subproperties are the same as
   those of the ``Transaction<i>`` property described above.
-  ``SeqNum``. Identifier of the notification.
-  ``Message``. If the notification indicates the start of a
   transaction, its value is “Started the transaction”. If it indicates
   the end of a transaction, its value is “Finished the transaction”.
-  ``Event``. This will be

   .. code-block:: none
   
      javax.management.Notification[source=com.denodo.vdb.management.mbeans:type=TransactionsManagementInfo,databaseName=<dbname>][type=<type>][message=<msg>]
      
      
   , where ``<dbName>`` is the name of the database of the Server,
   ``<type>`` is the type of notification (``startTransaction`` or
   ``endTransaction``) and ``<msg>`` is the notification message
   (``Started the transaction`` or ``Started the transaction``).
-  ``Source``. MBean name.
