=====================
Elasticsearch Sources
=====================

In order to create `Elasticsearch <https://www.elastic.co/>`_ exporters, a data 
source that gives access to the Elastic indexing server needs to be defined.

Denodo Scheduler supports the following versions of Elasticsearch servers:

- Elasticsearch 2.x
- Elasticsearch 5.6+ / 6.x 

Each version has its own data source configuration in Scheduler. Both share the following parameters:

-  **Host**. Machine name in which the Elastic server will run.
-  **HTTP Port**. Port number of the Elastic HTTP server. This is the port of the RESTful API.
-  **Cluster Name** (optional). The name of your production cluster, which is used
   to discover and auto-join other nodes.
-  **Username** (optional). Username for connecting to the Elastic
   server.
-  **Password** (optional). Password associated with the specified
   user.

In addition, the data source for version 2.x also has the following parameter:

-  **Transport TCP Port**. Port number of the Elastic server. This is the port used by the Java clients to connect to the cluster.
