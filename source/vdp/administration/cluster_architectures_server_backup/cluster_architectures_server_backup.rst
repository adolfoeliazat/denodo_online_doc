=====================================
Cluster Architectures / Server Backup
=====================================

.. toctree::
   :hidden:
   
   denodo_tools/denodo_tools.rst
   how_to_check_if_a_virtual_dataport_server_is_alive/how_to_check_if_a_virtual_dataport_server_is_alive.rst
   using_the_import_export_scripts_for_backup_and_or_replication/using_the_import_export_scripts_for_backup_and_or_replication.rst
   configuring_several_instances_of_a_virtual_dataport_server/configuring_several_instances_of_a_virtual_dataport_server.rst

Virtual DataPort can operate within a cluster architecture to provide
high availability and load balancing.

Typically, clustering architectures for Virtual DataPort operate as
follows:

#. An external load-balancing system distributes the requests received
   among a pool of Virtual DataPort servers.
#. The load-balancing system detects when a server is not responding
   (using the so-called “server health check”). In this case, the load
   is distributed among the remaining servers.
#. The clustering system detects when the server is available again
   (using the “health check”) and adds it to the pool.

Virtual DataPort provides some features to make cluster architecture
configuration easier:

-  It provides a “Ping” script to check the status of Virtual DataPort
   servers. Some products provide their own
   default implementations for “health-checking” an application. However, the
   Ping script detects failure situations that would not be detected by
   other solutions.
-  It provides a utility to automatically replicate the metadata and the
   configuration of a Virtual DataPort server into a pool of other
   Virtual DataPort servers. This utility can also be used to schedule
   backup copies of the metadata of a Server.
-  “Safe shut down mode”: when entering this mode, the Server does not
   accept new connections and it will not shut down until all the current
   queries finish. This allows you to remove a Server from the cluster
   without affecting external clients.

The following sections describe how to:

#. Install the Denodo Tools: a set of tools that help building a cluster
   with several Virtual DataPort servers (see section :ref:`Denodo Tools`).
#. Use the Ping script (see section :ref:`How to Check if a Virtual DataPort
   Server is Alive`)
#. Replicate the metadata of a Virtual DataPort server, to a group of
   servers (see section :ref:`Using the Import/Export Scripts for Backup
   and/or Replication`).