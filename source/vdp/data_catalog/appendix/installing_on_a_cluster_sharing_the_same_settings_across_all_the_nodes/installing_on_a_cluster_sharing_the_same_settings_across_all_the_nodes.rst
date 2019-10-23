=========================================================================================================
Installing the Data Catalog on a Cluster: Sharing the Same Settings Across All the Nodes
=========================================================================================================


This section explains how to configure the Data Catalog
to store its settings on an external database.

By default, the Data Catalog stores the global settings
and certain settings for each user in a local database. For example, the
user “Saved Queries”, the fields selected by the administrator to
display by default on a view search, etc.

If you installed Denodo on a cluster and there is a Data Catalog installed on each node of the cluster, you can
configure all of them to store and retrieve these settings from a common
database. Otherwise, users will use different settings depending on the
node the load balancer redirects them.

.. important:: The Data Catalog makes use of web
   sessions, so when it is installed on a cluster, the load balancer should
   be configured to redirect all the request of the same web session to the
   same node (sticky sessions).

Setting Up the Common Database
==============================

Before configuring the Data Catalog, you should create
and configure the common database in which all the tools of the cluster
will store the common settings. You can use the DBMS of your choice. You
have to:

#. Install the DBMS (if not installed yet).
#. Create the database to be used by the Data Catalog. An existent database
   could be used but it is recommended to create a new one to avoid
   possible conflicts with the table names.
#. Configure a user (and its password) with create, read and write
   privileges on that database.

Configure the Data Catalog to Use the Common Database
======================================================================

Clustering works by having each node of the cluster share the same database. 
To enable this feature you have to :ref:`configure each Data Catalog of the cluster to use 
the common database <Configure the internal database used by the Data Catalog>`.

This process involves stopping the Denodo
web container in each  node (i.e. all the SOAP and REST web services and web
administration tools will be stopped for a few seconds).