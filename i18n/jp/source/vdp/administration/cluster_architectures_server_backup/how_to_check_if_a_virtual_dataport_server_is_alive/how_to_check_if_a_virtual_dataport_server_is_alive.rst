==================================================
How to Check If a Virtual DataPort Server Is Alive
==================================================

When you connect to Denodo through a load balancer or an application
that creates a pool of connections to Virtual DataPort, the load
balancer or the application need a way to check that a connection is
still valid. The following sections explain different alternatives to
check the health of a connection.

Connecting from a JDBC Client Through a Load Balancer
=====================================================

Follow these steps when connecting from a JDBC client to Virtual
DataPort through a load balancer or another intermediate resource that
holds a pool of connections to Virtual DataPort:

#. Add the parameter ``reuseRegistrySocket=false`` to the URL of the
   JDBC client.
#. If the load balancer or the client will execute a ping query without
   support to set a timeout for that query, add the parameters
   ``pingQuery`` and ``pingQueryTimeout`` to the URL of the JDBC client.

The section :ref:`Connecting to Virtual DataPort Through a Load Balancer` of
the Developer Guide explains how to use these parameters.

Using the Ping Script
=====================

The script ``ping`` checks that a Virtual DataPort server is “alive”. It
is located in the directory :file:`{<DENODO_HOME>}/bin`.

Its syntax is the following:

.. code-block:: bnf 
   :caption: Syntax of the ping script
   :name: Syntax of the ping script
   
   ping [ -t timeout ] [ -q <query> -l <login> [ -p <password> | -cf <configuration file> ] ] [ -v ]
        <host>[:<port>][/<database name>  


.. table:: Parameters of the ping script

   +------------+-------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter  | Description                                                                                                                               |
   +============+===========================================================================================================================================+
   | -t         | Optional. Time to wait for a response, in milliseconds. If after this period the script does not receive a response, it returns an error. |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------+
   | -q <query> | Optional. If present, the script, besides checking if the Virtual DataPort server is up, it executes a query.                             |
   |            | If you indicate this parameter, you also have to pass the login and password of the user.                                                 |
   |            | In many operating systems, you have to surround the query with double quotes so the query is considered a single parameter.               |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------+
   | -l         | Mandatory when using ``-q``. Login.                                                                                                       |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------+
   | -p         | Password. Incompatible with ``-cf``.  You can provide the password encrypted by prefixing it with                                         |
   |            | ``encrypted:`` (use the script ``encrypt_password`` to encrypt the password). E.g. ``-p "encrypted:UjOsIu8972jviqGpcLP3Mg=="``            |
   |            |                                                                                                                                           |
   |            | If you provide the login (``-l``) but not the password (``-p``) nor the                                                                   |
   |            | configuration file (``-cf``), the script will prompt for your password.                                                                   |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------+
   | -cf <file> | Incompatible with ``-p``. File with one line that contains this:                                                                          |
   |            |                                                                                                                                           |
   |            | ``password=<password>``                                                                                                                   |
   |            |                                                                                                                                           |
   |            | or this:                                                                                                                                  |
   |            |                                                                                                                                           |
   |            | ``password=encrypted:<encrypted_password>`` (Use the script ``encrypt_password`` to encrypt the password)                                 |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------+
   | -v         | Optional. If present, the script displays the status the time taken to receive a response from the Server. Otherwise, it only displays    |
   |            | error messages.                                                                                                                           |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------+
   | <host>     | Host of Virtual DataPort.                                                                                                                 |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------+
   | <port>     | Optional. If not present, the script sends the request to the default Virtual DataPort port: 9999.                                        |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------+
   | <database> | Mandatory when using ``-q``. The database that the query will be executed.                                                                |
   +------------+-------------------------------------------------------------------------------------------------------------------------------------------+

The ping script returns 0 if the status check is successful.
Otherwise, it returns 1.

An example of running the ping command is shown below:

.. rubric:: Example 1

.. code-block:: bash

   ping -t 30000 -v //localhost:9999

Sends a ping request to ``localhost`` and port ``9999`` in verbose mode
with a timeout of 30 seconds.

.. rubric:: Example 2

.. code-block:: bash

   ping localhost

Sends a ping request to ``localhost``. As the port is not set, the
script sends the request to the default port: ``9999``.

.. rubric:: Example 3

.. code-block:: bash

   ping -t 30000 -q "SELECT 1" -l admin -p "encrypted:UjOsIu8972jviqGpcLP3Mg==" //localhost:5999/admin

Sends a ping request to ``localhost`` and port ``5999`` with a timeout
of 30 seconds. As we indicate the parameter, ``-q``, besides checking if
the Server is alive, the script will execute a query to the database
``admin``.

Note that the query (``-q`` parameter) is surrounded by double quotes so
the query is considered a single parameter.

The password was encrypted executing this command:

.. code-block:: bash

   encrypt_password.bat "my password"

Alternatives to the Ping Script
===============================

In an environment where the load balancer cannot run a ping query, nor
use the ping script, you can develop a script that sends an HTTP request
to Virtual DataPort through the RESTful Web service.

It is important for the queried view to be very lightweight. We suggest
creating this view with this statement:

.. code-block:: sql

   CREATE VIEW ping_query_view AS 
   SELECT 1 
   FROM Dual();

When this view is queried, it only invokes the internal stored procedure
``Dual`` and does not involve any data source.

Then, develop a script that access the URL
``http://localhost:9090/denodo-restfulws/admin/views/ping_query_view``

If this URL returns the HTTP code 200, it means that the Virtual
DataPort server is alive.
