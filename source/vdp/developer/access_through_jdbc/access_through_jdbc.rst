===================
Access Through JDBC
===================

.. toctree::
   :hidden:

   parameters_of_the_jdbc_connection_url/parameters_of_the_jdbc_connection_url
   connecting_to_virtual_dataport_through_a_load_balancer/connecting_to_virtual_dataport_through_a_load_balancer
   connecting_to_virtual_dataport_with_ssl/connecting_to_virtual_dataport_with_ssl
   connecting_to_virtual_dataport_using_kerberos_authentication/connecting_to_virtual_dataport_using_kerberos_authentication
   details_of_the_jdbc_interface/details_of_the_jdbc_interface 

JDBC (Java Database Connectivity) is a Java API that allows
executing statements on a relational database regardless of the Database
Management System used.

Denodo provides a JDBC type 4 driver that implements the main
characteristics of the JDBC 4.1 API 
(`Java Database Connectivity <https://www.oracle.com/technetwork/java/javase/jdbc/index.html>`_). 

You can get the Denodo JDBC driver from:

-  The Denodo Community (https://community.denodo.com/drivers/jdbc/). Download the version of the update installed in the Denodo server. To find out the version of the Denodo server you are connecting to, log into that server with the administration tool, click the menu *File* > *About...* and look for *VDP server* (below *installed updates*).
-  Or from the installation (:file:`{<DENODO_HOME>}/tools/client-drivers/jdbc/vdp-jdbcdriver-core/denodo-vdp-jdbcdriver.jar`).

The JDBC driver is compatible with Java 8, 9, 10 and 11. The JDBC driver included in the Denodo 7.0 update 20190312 or earlier requires Java 8. Note that you cannot use a JDBC driver of an update to connect to a Denodo server with an older update. That is, you cannot use the JDBC driver of 7.0 update 20190903 to connect to a Denodo server with the update 20190312.

|

The Virtual DataPort server is backward compatible with the JDBC driver and other clients, within the same major version. That is, the Denodo JDBC driver of an update can be used to connect to the Virtual DataPort server with the same update or a newer update. For example:

-  To connect to a Denodo server version 7.0 update 20181011, you can use the JDBC driver included in the same update or the driver included in 7.0 20180926, 7.0 20180611 or 7.0 GA.
-  Do not use the JDBC driver included in 7.0 20181011 to connect to a Denodo server version 7.0 update 20180926 because the driver is more recent than the server. This is an unsupported configuration; some operations may fail.
-  The JDBC driver of Denodo 7.0 GA or any 7.0 update will return an error when trying to connect to Denodo 6.0 or earlier.

|

The class that implements the driver is
``com.denodo.vdp.jdbc.Driver``.

The syntax of the database URL is

.. code-block:: none
   :caption: Syntax of the JDBC connection URL
 
   jdbc:vdb://<hostName>:<port>/<databaseName>[?<paramName>=<paramValue> [&<paramName>=<paramValue>]* ]

The name of the database is mandatory.

For example:

.. code-block:: none
   :caption: JDBC connection URL sample

   jdbc:vdb://localhost:9999/admin?queryTimeout=100000&chunkTimeout=1000&userAgent=myApplication&autoCommit=true
   
.. important:: The connection URL only references one port (by default, 9999). However, the driver *also* opens a connection to 
   the *Auxiliary port* of the Denodo server (by default, 9997). Therefore, if there is a firewall between the client and the Denodo server, you need to allow connections to both ports. You can get the value of this port using the administration tool, in the menu *Administration* > *Server Configuration*, in the tab *Server connectivity*.

If the name of the database contains non-ASCII characters, they
have to be URL-encoded. For example, if the name of the database is
“テスト”, the connection URL to the database will be this:

.. テスト means "Test" in Japanese 

.. code-block:: none
   :caption: JDBC connection URL sample with non-ASCII characters
   
   jdbc:vdb://localhost:9999/%E3%83%86%E3%82%B9%E3%83%88?queryTimeout=900000&chunkTimeout=1000&userAgent=myApplication&autoCommit=true

The path :file:`{<DENODO_HOME>}/samples/vdp/vdp-clients` contains examples
of client programs accessing Virtual DataPort through JDBC (the ``README`` file
of this path explains how to generate and publish the views accessed by
the clients in the example).

|


These are some of the features of the JDBC specification supported by the
Virtual DataPort JDBC driver:

-  The data types supported are defined in the VQL Guide
   (includes support for all basic types and for fields of the type
   array and register).
-  Execution of statements to query, insert, update and delete
   data. In addition, to create new elements such as data sources,
   views, etc.
-  Support for metadata description statements and listing of
   server catalog elements.
-  Support for ``PreparedStatements``.
-  Support for canceling the current statement execution by using
   the ``cancel()`` method of the ``java.sql.Statement`` class.
   When a query is cancelled, the Virtual DataPort Server will cancel
   all current accesses to data sources and cache. After invoking the
   ``cancel`` method, it is still possible for the server to return some
   results, if these were retrieved before the source access canceling
   were effective. Therefore, the query cancellation does not imply
   closing the ``ResultSet`` that is being used.
-  Invocation of stored procedures using the ``CALL`` statement.
-  Support for submitting batches of commands.
-  The ``ResultSet`` objects returned by Virtual DataPort are not
   updatable (i.e. ``CONCUR_READ_ONLY``) and have a cursor that moves
   forward only (i.e. ``TYPE_FORWARD_ONLY``). In addition, the
   ``ResultSet`` objects are closed when the current transaction is
   committed (i.e. ``CLOSE_CURSORS_AT_COMMIT``).