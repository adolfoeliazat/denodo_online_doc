==========================================================================
Backward Compatibility Between the Virtual DataPort Server and Its Clients
==========================================================================

This section explains the backward compatibility policies between the Virtual DataPort server, and its administration tool and other clients.

.. note:: The following policies and exceptions apply to the administration tool, the JDBC driver and the ODBC driver, even though only the administration tool is mentioned.

-  The administration tool and the Virtual DataPort server have to be of the same major version. E.g. you cannot connect to a Virtual DataPort server 7.0 from an administration tool of version 6.0, nor vice versa.

-  A Virtual DataPort server is compatible with an administration tool if:

   1. They both have the same update installed.
   2. Or if the administration tool has an update that is older than the update installed on the server.

-  **Do not connect** to a Virtual DataPort server using an administration tool that has an update newer than the update of the server. This is not supported. It may lead to unexpected errors.

.. rubric:: Important Exceptions to These Policies

1. **Do not use** an administration tool without updates (GA version) to connect to a Virtual DataPort server 7.0 with the Sep 2018 update (20180926) or a more recent update.

#. To connect to a Virtual DataPort server 7.0 GA (without updates), **only use** an administration tool without updates.

#. To connect to a Virtual DataPort server 7.0 with the Jun 2018 update (20180611), **only use** an administration tool with the same update.

In these scenarios, the connection to the server will succeed but the queries that return datetime values may fail unexpectedly.
