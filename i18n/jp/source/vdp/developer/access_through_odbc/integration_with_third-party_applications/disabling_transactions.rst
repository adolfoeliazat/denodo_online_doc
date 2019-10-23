======================
Disabling Transactions
======================

Some applications start transactions inadvertently. Starting
transactions when is not needed puts unnecessary strain on the Virtual
DataPort server. By default, to avoid this impact on the performance, the ODBC interface of Denodo ignores the statements that ODBC applications send to start and finish transactions. These are:

-  ``BEGIN``
-  ``COMMIT``
-  ``ROLLBACK``
-  ``SAVEPOINT``
-  ``RELEASE``

You can configure the ODBC interface to process these statements. To do this, follow these steps:

#. Open the Virtual DataPort administration tool.
#. Log in as an administrator user.
#. Open the VQL Shell and execute this statement:

.. code-block:: vql

   SET 'com.denodo.vdb.vdbinterface.server.odbc.ignoreTransactions' = 'false';

The change is applied immediately; restarting the Virtual DataPort
server is not necessary.

After these steps, the ODBC interface will start processing the commands listed above.

Changing this property affects all the clients that connect to the ODBC
interface. It does *not* affect clients that connect to Virtual DataPort
through its other interfaces (JDBC clients, published web services,
etc.)

To revert to the default behavior, execute the following:

.. code-block:: vql

   SET 'com.denodo.vdb.vdbinterface.server.odbc.ignoreTransactions' = 'true';
