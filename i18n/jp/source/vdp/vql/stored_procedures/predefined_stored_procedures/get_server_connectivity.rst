=======================
GET_SERVER_CONNECTIVITY
=======================

.. rubric:: Description

The stored procedure ``GET_SERVER_CONNECTIVITY`` returns the TCP/IP port numbers that Virtual DataPort listens for incoming connections.

.. rubric:: Syntax

.. code-block:: bnf

   GET_SERVER_CONNECTIVITY ()      

The procedure returns a single row, with these fields:

-  ``server_port``: port for incoming connections from JDBC applications, JMX applications (from the Denodo Monitor and other monitoring tools), and from the administration tool.
-  ``odbc_port``: ODBC port.
-  ``http_port``: HTTP port of the web container of Denodo.
-  ``https_enabled``: ``true`` if HTTPS is enabled on the web container for the communications between the web container and its clients; ``false`` otherwise. Note that even if this is true, the communications through the ``http_port`` are in plain text.
-  ``https_port``: HTTPS port of the web container of Denodo.
-  ``ssl_enabled``: true if SSL/TLS is enabled for the traffic between Virtual DataPort and its clients; false otherwise.

.. rubric:: Privileges Required

No privileges are required to execute this procedure.

.. rubric:: Examples

.. rubric:: Example 1

.. code-block:: sql

   SELECT *
   FROM GET_SERVER_CONNECTIVITY()        
