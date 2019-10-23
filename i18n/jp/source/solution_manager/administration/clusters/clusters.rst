********
Clusters
********

A cluster is a group of Denodo servers that belong to the same environment.

To guarantee high availability, the production environments are organized in one
or several clusters behind a load balancer that decides which is the final
server that will process the incoming requests.

Moreover, production environments should provide low latency. Organizations meet
this requirement with several clusters geographically distributed. For example,
they may have one cluster in North America, another one in Europe and a third
one in China.

A typical structure of a cluster includes several Virtual DataPort servers, one
Scheduler server and one database server working as data cache.

All the Virtual DataPort servers in the same environment share the same
metadata.  This means that, before promoting changes from another environment,
you have to define, at environment level, the properties required to execute a
deployment on the Virtual DataPort servers.

On the other hand, the metadata of the Scheduler server is shared at cluster
level, since it references servers or data sources local to the cluster.
Therefore, before promoting changes from another environment, you have to
define, on each cluster, the properties required to execute a deployment on the
Scheduler servers.

Along this chapter, you will learn to:

* :ref:`Create clusters <sm_creating_clusters>`.
* :ref:`Assign values to load balancing variables <sm_configuring_cluster_load_balancing_variables>` for a particular cluster.
* :ref:`Manage Scheduler properties <sm_configuring_cluster_scheduler_properties>` for a specific cluster.
* :ref:`Configure the logging level <sm_configuring_cluster_logging>` of the Virtual DataPort servers included in the cluster.

.. toctree::
   :hidden:
   
   creating_clusters/creating_clusters.rst
   configuring_clusters/configuring_clusters.rst
