*******
Servers
*******

A server in the Solution Manager represents a specific service (included in an installation of the Denodo Platform): Virtual DataPort Server, Scheduler Server, ITPilot Verification Server, ITPilot IEBrowser Pool Server and Data Catalog server.

In the Solution Manager, the Denodo servers are grouped in clusters where all the servers defined in the same cluster hold the same metadata, in order to make a set of services highly available to clients.

The servers defined in the Solution Manager can get their license dynamically
:ref:`using the License Manager or using a local file <sm-licenses-work>`.

In addition, when you promote revisions to a target environment, the changes included in the revisions, will be applied in all the Virtual DataPort and Scheduler servers defined in the environment.

Along this chapter, you will learn to:

* :ref:`Create servers <sm_creating_servers>`.
* :ref:`Assign values to load balancing variables <sm_configuring_server_load_balancing_variables>` for a specific Virtual DataPort server.
* :ref:`Configure the logging level <sm_configuring_server_logging>` for a specific Virtual DataPort server.

.. toctree::
   :hidden:
   
   creating_servers/creating_servers.rst
   configuring_servers/configuring_servers.rst