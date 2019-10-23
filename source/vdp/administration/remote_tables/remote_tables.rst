=============
Remote Tables
=============

.. toctree::
   :hidden:

   managing_remote_tables/managing_remote_tables.rst

With Virtual DataPort, you can store the result of a query on a table of an external database. This feature is called
*remote tables*. When creating a remote table, Denodo automatically does the following:
-  Creates the table with the appropriate schema in an external database.
-  Inserts the result of the query in this table. 
-  Optionally, it creates a base view in Virtual DataPort associated to this new table.

With this feature, the user does not need another tool to execute a query in Denodo and store its
result on a database. The following section (:ref:`Managing Remote Tables`) describes how to create, edit, and drop remote tables.

With this feature, you can use Denodo to create ETL flows (Extract, Transform, Load) easily, quickly and
efficiently. You create this flow in Denodo with a declarative approach, using a SQL query, 

This has multiple benefits over traditional ETL flows:

1. In Denodo, you create the ETL flows from a SQL query. This declarative approach is easier to maintain than traditional ETL flows, which are procedural.
#. Very often, the execution time of an ETL task in Denodo will be faster than in a conventional ETL tool. 
   That is because the execution engine of Denodo optimizes the process to
   obtain the data, and furthermore, it uses parallel bulk data load to store the data in the destination database.
#. In Denodo, you can launch the ETL tasks manually from the administration tool, or scheduling them to run periodically using Denodo Scheduler.

This feature is very useful to move data to the same environment where the applications that consume the data run. Having the data in the same location has the following benefits:

- Minimizes the data access time.
- Parallel data set reads.
- Reusable data set. The data consumption application can execute several times over the same data set without the app
  have to request it again to Virtual DataPort. Additionally, more than one application can access to the same data
  set at the same time.

A common use case of this feature is to move data from an on-premise database to HDFS or Amazon S3 and then, in the same environment, use
Apache Spark to run data processing jobs.