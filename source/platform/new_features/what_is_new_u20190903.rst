=========================================
What Is New in Denodo 7.0 Update 20190903
=========================================

This page lists the main enhancements added in Denodo 7.0 update 20190903. For a full list of enhancements and bug fixes, check the RELEASE NOTES of the update, inside the zip file of the update or in the `Support Site <https://support.denodo.com/resources/update/list/>`_.

.. rubric:: Virtual DataPort

-  This update includes several performance improvements that reduce the execution time, CPU usage and memory usage of several operations:

   -  Calculate the lineage and the tree view
   -  Obtain the dependencies of the views
   -  Generate the VQL of the views
   -  Update the configuration of a data source 
   -  Generating the SQL queries to executed on a database.
   -  Optimizing the execution plan of a query.
   -  ...

  In views with several hundreds of columns, these improvements provide a tenfold reduction in the execution time of these operations.

  This enhancement only applies to elements created after 
  you install this update. To benefit from these enhancements 
  for the existing elements, you need to re-import their VQL (e.g. export the VQL of the entire Server and import it again).

-  The query optimizer can now push down the operations GROUP BY and JOIN under an extended UNION (i.e. a UNION of views that have a different schema).

.. rubric::  Virtual DataPort (Data Sources)

-  Added support for new databases: Cassandra, Amazon Athena and Elasticsearch 6.7 (Elasticsearch 6.5 was already supported). The appendix :ref:`Supported JDBC Data Sources` of the Administration Guide lists all the officially supported databases.
-  The data sources DF, JSON and XML now support :ref:`mutual authentication <vdp_admin_guide_path_mutual_authentication>`. That is, using a private key for authentication.
-  Denodo can now gather statistics from the system tables of DB2 for z/OS (:ref:`list of databases <Statistics that are gathered from the system tables of each vendor>` from which Denodo can gather the statistics of tables/views from their system tables).

.. rubric:: Virtual DataPort (Cache Engine)

-  Data sources using pass-through credentials can now be used as cache database. This helps maximize the number of queries that the execution engine can execute in the same data source. Before using this feature, read the appendix :doc:`/vdp/administration/appendix/considerations_when_configuring_data_sources_with_pass-through_credentials/considerations_when_configuring_data_sources_with_pass-through_credentials` of the Virtual DataPort Administration Guide because enabling this option may not be adequate in all scenarios.

.. rubric:: Virtual DataPort (Administration Tool)

-  Performance improvements in the communications between Virtual DataPort and the administration tool. 
   These improvements are noticeable in WAN environments and slow connections.
   To enable this option, open the menu *Tools* > *Admin Tool preferences* (tab *Connection*) and select 
   the check box :ref:`Use WAN optimized communications <Connection Parameters>`. This option is not enabled by default because, 
   when the tool and the server are on the same network, it does not provide any benefit and it could even slow down the connection slightly.

-  Views are opened faster because now, the tool loads some information of the views on demand. E.g. 
   the tool only requests the information about the tree view if the user opens the tree view.

-  Users no longer need to modify the Windows registry to use single sign-on. With previous updates, the user has to modify the registry. To benefit from this feature, you need to update Virtual DataPort *and* the administration tool.

-  "Execute" dialog and VQL Shell: the options "Limit rows" and "Stop query when the limit is reached" have been renamed to "Display rows" and "Retrieve all rows" respectively to make clearer what these options do.
 
.. rubric:: Virtual DataPort (JDBC Driver)

-  The JDBC driver can now be loaded from client applications that support Java 9, 10 or 11. The driver included with 
   previous updates can only be loaded from applications that run with Java 8.

-  Users no longer need to modify the Windows registry to connect to Denodo with single sign-on. With previous updates, you have to modify the registry. To benefit from this feature, you need to do the following:

   1. Update Virtual DataPort and use the latest JDBC driver.
   2. Add the necessary libraries to the classpath of the client application. See more details in the section :ref:`Connecting to Virtual DataPort Using Kerberos Authentication` of the Developer Guide.

.. rubric:: Virtual DataPort (Operations and Management)

-  Now, you can automatically :ref:`store the execution trace <Storing the Execution Trace of Queries>` of the queries that fail or all the queries.

-  In this update, :ref:`Virtual DataPort <Setting-Up the Kerberos Authentication in the Virtual DataPort Server>` and the :ref:`administration tool <Configuring the Administration Tool to Use Kerberos Authentication>` - not Scheduler - have changed the way they store the Kerberos debug information in order to facilitate gathering this information. Now, this information is stored in the same file as other logging information ("vdp.log" and "vdp-admin.log"). The benefits are that you do not need to launch the applications differently to obtain this information and each Kerberos message has a timestamp. 

   .. note:: There is an additional step when obtaining the Kerberos debug information: adding a line to the file "log4j2.xml". The sections linked above explain how to do this.

   .. important:: This change affects any log messages that *with previous updates*, Virtual DataPort printed in the standard output. For example, if you add the system property ``-Djavax.net.debug=all`` to Virtual DataPort, the output will now be stored in the log file of the component where you added this property (e.g. for Virtual DataPort, in "vdp.log") and you need to change the "log4j2.xml" file in the same way you do to enable the Kerberos debug mode.
   
.. rubric:: Data Catalog

-  The Data Catalog has new privileges that allow for a more fine-grained control of what users can do:

.. csv-table:: 
   :header: "Tasks", "Roles Required in Virtual DataPort To Perform These Tasks" 
   :escape: '
   
   "Assign categories, tags and custom properties groups to views and web services.", "data_catalog_classifier"
   "Edit views, web services, databases, tags, categories, custom properties groups and custom properties.", "data_catalog_editor"
   "Can do the same as a user with the roles '"data_catalog_editor'" and '"data_catalog_classifier'".", "data_catalog_manager"
   "Configure the personalization options and content search.", "data_catalog_content_admin"
   "Can perform any action.", "data_catalog_admin"

..

   See more details about these roles in the section :doc:`/vdp/data_catalog/administration/administration` of the Data Catalog Guide.

-  Better response times of the dialogs "Element Management", "Tags" and "Content Search Configuration".

-  Performance improvements in the metadata search and in the metadata synchronization with Virtual DataPort. This is noticeable on servers with a lot of elements.

-  The option :ref:`Compute usage statistics <Computing Usage Statistics>` now updates the data about usage before calculating the statistics.
 
-  You can now export and import the metadata of the Data Catalog (categories, tags, settings, etc.) programmatically. You can use a :ref:`script <Import and Export Data Catalog Metadata Using a Script>` or use the :ref:`REST API <data-catalog-rest-api-export-metadata>` of the Data Catalog. The goal is to facilitate the promotion of the metadata across environments.

-  Added support to store the metadata of the Data Catalog in the PostgreSQL edition of Amazon Aurora.


.. rubric:: Enhancements that Apply to Several Components

-  If you enabled SSL/TLS on any of the modules of Denodo, you can now store the password of the keystore encrypted. See how to do it in the 
   sections :doc:`/platform/installation/postinstallation_tasks/enable_ssl_connections_in_the_denodo_platform_servers/enabling_ssl_in_denodo_platform_servers`, :ref:`Enabling HTTPS in the Embedded Apache Tomcat` and :doc:`/solution_manager/installation/postinstallation_tasks/enable_ssl_connections_in_the_solution_manager_servers/enabling_ssl_in_solution_manager_servers`.
   
-  You can now configure all the components to :ref:`store the logs on Amazon S3 <How to Store the Log files of Denodo in Amazon AWS S3>`.

.. rubric:: Solution Manager

-  The REST API of the Solution Manager has new operations that allow you to register and deregister environments, 
   clusters and servers. Before, you had to do it manually from the administration tool.

   With these operations, you can use the auto scaling capabilities of your cloud provider 
   to start and stop Denodo servers when necessary, and register and deregister them on the Solution Manager. 
   The article `Configuring Auto Scaling of Denodo in AWS <https://community.denodo.com/kb/view/document/Configuring%20Autoscaling%20of%20Denodo%20in%20AWS?category=Operation>`_ of the Knowledge Base explains how to do this.
   
   For example, if you have eight Denodo servers on your production environment, you can set a rule to start an additional server when the CPU usage of these servers is higher than 80% for a certain period of time.

-  Deploying revisions that include data sources is easier. When a revision includes properties that are 
   not defined in the target environment (e.g. user/password of a new data source), the user can now choose between using the 
   values of the source environment or entering new values.