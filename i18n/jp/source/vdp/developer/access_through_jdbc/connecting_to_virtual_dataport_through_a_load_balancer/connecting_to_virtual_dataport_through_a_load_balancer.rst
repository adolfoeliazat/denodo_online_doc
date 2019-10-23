======================================================
Connecting to Virtual DataPort Through a Load Balancer
======================================================
 
Read this section when the JDBC client is connecting to Virtual DataPort
through a load balancer or another intermediate resource that holds a
pool of connections to Virtual DataPort.

The table `Parameters of the JDBC driver useful to set-up a cluster of
Denodo servers`_ lists the parameters of the URL that are useful when
connecting to Virtual DataPort through a load balancer:

.. table:: Parameters of the JDBC driver useful to set-up a cluster of Denodo servers
   :name: Parameters of the JDBC driver useful to set-up a cluster of Denodo servers
    
   +--------------------------------------+--------------------------------------+
   | Parameter of the URL                 | Description                          |
   +======================================+======================================+
   | reuseRegistrySocket                  | Important: set this property to      |
   |                                      | ``false`` when connecting to Virtual |
   |                                      | DataPort through a load balancer.    |
   |                                      |                                      |
   |                                      | If ``false``, the requests will be   |
   |                                      | more evenly distributed across the   |
   |                                      | Virtual DataPort servers of the      |
   |                                      | cluster than if the property is not  |
   |                                      | set or set to ``true``.              |
   |                                      |                                      |
   |                                      | Default value: ``true``              |
   +--------------------------------------+--------------------------------------+
   | pingQuery and pingQueryTimeout       | Important: only use these two        |
   |                                      | properties if the load balancer or   |
   |                                      | the client will execute a ping query |
   |                                      | without support to set a timeout for |
   |                                      | that query.                          |
   |                                      |                                      |
   |                                      | When a client executes the query set |
   |                                      | on the parameter ``pingQuery``, the  |
   |                                      | JDBC driver returns an error if that |
   |                                      | query does not finish in the number  |
   |                                      | of milliseconds set on               |
   |                                      | ``pingQueryTimeout``.                |
   |                                      |                                      |
   |                                      | See below for a more detailed        |
   |                                      | explanation of these properties.     |
   |                                      |                                      |
   |                                      | Default value for both parameters:   |
   |                                      | <empty>                              |
   +--------------------------------------+--------------------------------------+
   
Sample URL for JDBC applications that connect to Virtual DataPort
through a load balancer:

.. code-block:: none

   jdbc:vdb://acme:9999/support?reuseRegistrySocket=false 

Sample URL for JDBC applications with the parameters ``pingQuery`` and
``pingQueryTimeout``:

.. code-block:: none 

   jdbc:vdb://acme:9999/admin?reuseRegistrySocket=false&pingQuery=SELECT 1&pingQueryTimeout=1000

With the URL above, if the query ``SELECT 1`` does not finish in one
second, the driver returns an error.

You need to add the parameters ``pingQuery`` and ``pingQueryTimeout`` to
the connection URL if the load balancer or the client meet these
conditions:

-  It will execute a ping query to check that the Virtual DataPort
   server is alive, or a connection to it is still valid.
-  And it does not support setting a timeout for that ping query.

At runtime, when the JDBC driver receives the query set on the parameter
``pingQuery``, it will wait for a maximum of ``pingQueryTimeout``
milliseconds for the query to finish. If the query does not finish in
that time, the driver will return an error, which will indicate the
client or the load balancer that the connection is no longer valid. A
connection to a Virtual DataPort server can become invalid when it has
timed out or dropped by a firewall.
