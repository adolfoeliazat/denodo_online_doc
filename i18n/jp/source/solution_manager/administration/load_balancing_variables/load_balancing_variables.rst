************************
Load Balancing Variables
************************

As part of the process of :ref:`configuring the deployment in an environment
<Configuring Deployments>`, you can define a :ref:`set of scripts <Configuring
Deployment Scripts>` that disable and enable a Virtual DataPort server or an
entire cluster in the load balancer. These scripts can receive two kind of
parameters: literal values or load balancing variables.

Load balancing variables are user-defined variables that can be assigned to
Virtual DataPort servers or clusters. Thus, if your script depends on a cluster
load balancing variable, you have to give a value to that variable for each
cluster of the environment. In the same way, if it depends on a server load
balancing variable, you should assign a value to that variable for every Virtual
DataPort server that belongs to the environment. When the Solution Manager
executes the script, it will use the corresponding value of the load balancing
variable, according to which server or cluster it is updating.

Take into account that the scripts that enable or disable clusters in the load
balancer can only receive cluster load balancing variables as parameters.
Nevertheless, those scripts that enable or disable servers in the load balancer
can receive both cluster and server load balancing variables.

There are some load balancing variables that are automatically created for every
server: *name*, *host* and *port*. Their value corresponds to the field
with the same name in the :ref:`server definition dialog <sm_creating_servers>`.
These predefined load balancing variables cannot be deleted or modified.

Let's see an example with a cluster "Cluster 1", with the IP address
192.168.0.1, and a server "Server 1" defined inside the cluster, with the IP 192.168.0.10. 

You can define the load balancing variable *ClusterIP*, which applies to
clusters, and assign it the value 192.168.0.1 for the cluster of the
example.

In the deployment configuration, you can define the script that disables servers
in the load balancer as ``disable_server_from_load_balancer.sh <clusterIP> <host>``,
where ``host`` is the predefined load balancing variable and ``ClusterIP`` is
the cluster load balancing variable that you have created.

During the deployment, the Solution Manager executes the script with the
appropriate arguments, using the values of the load balancing variables
assigned to the current server and its cluster, as follows:

.. code-block:: bash

   $ disable_server_from_load_balancer.sh 192.168.0.1 192.168.0.10

.. note:: Only global administrators and promotion administrators
          can configure load balancing variables. More information is available
          in the :ref:`Authorization` section.

Along this chapter, you will learn to:

* :ref:`Create load balancing variables <sm_creating_load_balancing_variables>`.
* :ref:`Assign values to load balancing variables <sm_assign_values_to_load_balancing_variables>`.

.. toctree::
   :hidden:
   
   creating_load_balancing_variables/creating_load_balancing_variables.rst
   assigning_values_to_load_balancing_variables/assigning_values_to_load_balancing_variables.rst