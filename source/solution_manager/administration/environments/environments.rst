************
Environments
************

.. toctree::
   :hidden:
   
   creating_environments/creating_environments.rst
   configuring_environments/configuring_environments.rst

The information infrastructure of any organization evolves in the same manner as
the software development does. Any change in the infrastructure has to surpass a
series of stages in a defined lifecycle, until it finally reaches production. A
typical lifecycle considers stages like *development*, *staging* or
*production*. In the Denodo ecosystem, each one of these stages is called an
environment.

In terms of composition, an environment is defined as a set of servers, of the
same or different type, working together for a common purpose. For example, an
environment can be composed by several Virtual DataPort servers, one Scheduler
server and one database server working as data cache. In addition, an
environment is also composed by all the resources and data sources that the
servers depend on.

Inside an environment, servers are organized in one or several clusters, with a
load balancer per cluster. All the requests that enter the cluster are
preprocessed by the load balancer, who decides which is the final server that
will process the queries among the set that conforms the cluster. That is the
way organizations guarantee high availability in their systems. 

Take into account that, for this to work, **all the servers in the same
environment have to share the same metadata**, since they operate on the same
resources and data sources. However, each environment manages its own set of
data sources and resources. Hence, the server's metadata must be different among
environments.

The Solution Manager allows you to promote changes from one environment to the
next one in the lifecycle. Since every environment has different needs, in terms
of consistency or service interruption, the Solution Manager implements several
strategies for deploy changes. Therefore, each environment can configure its own
deployment strategy.

Along this chapter, you will learn to:

* :ref:`Create a new environment <sm-creating-environments>` and define its basic information.
* :ref:`Manage Virtual DataPort properties <Configuring Virtual DataPort Properties>` for a specific environment.
* :ref:`Configure a proper deployment strategy <Configuring Deployments>` according to the environment needs.
* :ref:`Configure the logging level <Configuring Environment Logging Level>` of the Virtual DataPort servers included in the environment.

