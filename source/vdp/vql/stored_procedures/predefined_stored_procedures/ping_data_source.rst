==================
PING_DATA_SOURCE
==================

.. rubric:: Description

The stored procedure ``PING_DATA_SOURCE`` checks whether a specified data source is accessible from Virtual DataPort or not.

It tries to open a new connection to the data source within a time window and returns one of the following values:

- ``UP``: The connection was established, so the data source is accessible from Virtual DataPort.

- ``DOWN``: There was a problem trying to connect to the data source.

- ``TIMEOUT``: There was no successful or unsuccessful response within the time window.

As a side effect, it also updates the ping-related attributes of the corresponding :ref:`DataSourceManagementInfo MBean`.

.. rubric:: Syntax

.. code-block:: bnf

   PING_DATA_SOURCE (
         database_name : text
       , data_source_type : <ping_data_source_type>
       , data_source_name : text
       , timeout : long
   )

   <ping_data_source_type> ::=
         'JDBC'
       | 'ODBC'
       | 'LDAP'
       | 'OLAP'
       | 'SAPBWBAPI'
       | 'SAPERP'
       | 'SALESFORCE'

- ``database_name``: name of the database where the data source resides.

- ``data_source_type``: type of the data source. Take into account that only a subset of all the data source types are supported for ping invocation.

- ``data_source_name``: name of the data source.

- ``timeout``: time in milliseconds that the stored procedure will be waiting for an answer from the data source. This parameter is optional. If ``null``, the stored procedure will consider the default timeout, whose value is ``15000``.

The stored procedure returns one row with the following fields:

- ``database_name``: name of the database where the data source resides.

- ``data_source_type``: type of the data source.

- ``data_source_name``: name of the data source.

- ``status``: result of the ping invocation, which can take the value ``UP``, ``DOWN`` or ``TIMEOUT``.

- ``startime``: instant when the ping to the data source actually started.

- ``duration``: if the result of the ping is ``UP`` or ``DOWN``, the time in milliseconds that the data source took to respond; ``null`` otherwise.

- ``down_cause``: if the result of the ping is ``DOWN``, a message explaining the cause of the problem; ``null`` otherwise.

.. rubric:: Privileges Required

Only the users with CONNECT privilege on the database where the data source resides and METADATA privilege on the data source will be able to execute this stored procedure.