=====================================
Preparing the Connection to Databases
=====================================

For each the databases from which you plan to obtain data, you need to do this:

1. Check if the Denodo Platform includes the JDBC driver to connect to this database.

   The appendix :ref:`Supported JDBC Data Sources` of the
   Administration Guide lists the databases supported and if its driver is included or not.
   
   If the driver is not included, once you obtain it, copy it to the folder :file:`<DENODO_HOME>/lib-external/jdbc-drivers/{database adapter - version}`, in the host where the Denodo server runs.
   For example, if you are going to connect to Teradata 15, copy the driver to :file:`<DENODO_HOME>/lib-external/jdbc-drivers/{teradata-15}`.

#. Obtain a service account with at least READ privileges over the tables
   and views that you want to query. This user account will be used at
   least, while importing these elements into Virtual DataPort.

#. If this database is going to be a target of a :ref:`Data Movement`, this account also requires the privilege to create and delete tables, and execute ``INSERT``, ``UPDATE`` and
   ``DELETE`` statements on these tables.

Cache Database
==============

Virtual DataPort includes a :ref:`Cache Engine <Cache Module>` that can store a copy of
the data retrieved from the data sources, in a JDBC database. Its main goal is to increase the performance of queries.

We recommend creating a catalog or schema in the database specifically for the Cache Engine of Denodo. The Cache Engine 
creates many tables (one for each view of Denodo where the cache 
is enabled). Therefore, it is useful 
for these tables to be isolated from the rest of the elements
of that database.

The encoding of the new catalog or schema must support this:
-  Encoding: multi-byte characters (e.g. UTF-8). This will allow to cache data that contains multi-byte characters (e.g. Japanese characters). Also, to be able to enable cache for a view whose name or fields has a multi-byte characters.
-  Collation: the collation is binary.
   
Grant the following privileges to the user account that Denodo will use
to connect to this database:

-  Privileges to create and drop tables on this schema.
-  Privileges to execute ``SELECT``, ``INSERT``, ``UPDATE`` and
   ``DELETE`` statements on these tables.

.. note:: Virtual DataPort embeds an `Apache Derby <http://db.apache.org/derby/>`_
   database that can be used to store
   the cache data. However, we strongly advise against using it on a
   production environment. This database is provided just for demoing and
   development purposes.

See more about this in the section :doc:`Cache Module <../../../../vdp/administration/cache_module/cache_module>` of the Administration Guide. It explains how the cache module works and lists the databases that Virtual DataPort can
use to store cached data.