========================================
7.0 GA: New Features of Virtual DataPort
========================================

This section lists the new features of Denodo Virtual DataPort |version| GA. For the list of features included in the updates of Denodo |version|, check their RELEASE NOTES.




.. csantos@2018/03/10: I don't know what the following means


    .. #32815 Load data in multiple blocks in Impala
    .. #35032 Unable to see queries in SAP Business Explorer Introspection



.. contents::
   :local:
   :depth: 1
   :backlinks: none

Data Sources
============

Salesforce.com Data Source
--------------------------
.. #32233 Add support for a new data source and wrapper type named SALESFORCE
.. #34509 Add support for providing typesize, precision, scale in saleforce wrappers

Denodo now has a :ref:`native data source to Salesforce.com<Salesforce Sources>`. It can perform queries and insert, update and delete records of your Salesforce account.

For previous versions of Denodo, there is a *Denodo Connect* component to connect to Salesforce. Starting with Denodo 7.0, users should use the out-of-the-box data source because the *Denodo Connect* component will not be actively maintained.

The main benefits of the new connector are:

-  You no longer have to enter the connection details to Salesforce for every base view you create.
-  You have a single data source. With the *Denodo Connect* component you may have to create more than one depending on the type of requests you need to send to Salesforce.
-  More operations are pushed down to Salesforce.

JDBC Data Sources
-----------------
.. rubric:: Oracle

The type "XMLTYPE" of Oracle is now mapped to the type "xml". In previous version it is mapped to "text".


.. rubric:: SAP HANA

.. #34799 Enhance the SAP HANA 1 database adapter.

It is now possible to connect to SAP HANA using Kerberos authentication with constrained delegation.

.. rubric:: SQL Server

.. #33726 Distribute the microsoft sql server driver
.. #35035 Update the distributed SQL Server driver

Denodo now includes the Microsoft driver for Microsoft SQL Server. In previous versions, the administrator has to download it from the Microsoft's website.

.. #35118 Add support for SSL to the JTDs driver with Java 8

The jTDS driver included with Denodo is now capable of establishing SSL connections. The jTDS driver of previous versions of Denodo can only establish non-SSL connections.

.. #23272 Import "date" types in SQL Server as "date" instead of "text" types by default when using JTDS

When creating a base view over a data source that uses one of the jTDS adapters, the datetime fields are now created with the appropriate data type. In previous versions, they are created with the type "text".

.. rubric:: Folder for JDBC Drivers Not Included Out of the Box

.. #35142 - We no longer recommend installing new drivers in "<DENODO_HOME>/extensions/thirdparty/lib". Recommended place to put all the drivers
.. #33248 Create a directory in "/lib-external/jdbc-drivers/" for all the adapters we support so the user knows where to put them

Due to licensing restrictions, Denodo does not include the JDBC driver of all the supported databases. There is now a specific directory where you have to copy the JDBC drivers not included with Denodo: :file:`{<DENODO_HOME>}/lib-external/jdbc-drivers/{name of the adapter}`.

.. rubric:: New Databases Supported

.. #35506 Add support for Database catalog based statistics for PrestoDB adapter

The database Presto is now officially supported.

.. rubric:: JDBC Data Sources with Kerberos Authentication

.. #34900 Add the default properties for Kerberos connection to Cloudera and Hortonworks datasources

When creating a JDBC data source with the adapter "Hive for Cloudera" or "Hive for Hortonworks", the administration tool automatically adds the "driver properties" required by the driver to use Kerberos authentication.

.. rubric:: Generic Adapter

.. #33794 Allow to configure a "Generic" data source to not support PrepareStatement

JDBC data sources that use the adapter *Generic* can now be configured to use statements instead of prepared statements, when sending queries to the database. By default, they use prepared statements.

.. rubric:: Subtypes of DECIMAL Fields

.. #32016 Set the Subtype of a field looking the best fit (integer,long,decimal) when introspecting a DECIMAL

When creating a new base view, Denodo now selects the most appropriate type for the numeric fields of the view. For example, if the database reports that the type of a field is DECIMAL with a length of 5 digits and no decimals, the type in the base view will be int (integer).


.. rubric:: Connection Pool

.. #33578 Reduced needed synchronization for creating new JDBC connections

Reduced thread contention when the execution engine requests a connection to the pool of connections of a data source.

DF Data Sources
---------------

.. #35372	Support for using form feed and vertical tab as character delimiters with DF data sources

Added support for using form feed (\\f) as *end of line delimiter*.

Added support for using the vertical tab (\\v) as *column delimiter*.



Custom Data Sources
-------------------

.. #34894 - Add support for providing field subtype information in custom wrappers API

Now the API of custom wrappers provides support to indicate the properties of the fields returned by the wrapper: subtype, length, decimals if it applies, etc.

Denodo 7.0 provides binary compatibility with custom wrappers developed for previous versions of Denodo. That is, they will continue without having to modify them nor recompiling them.

HTTP Requests of Data Sources
------------------------------------------------------

.. rubric:: Paginated Sources

.. #23607 Streamlining creation of a view containing paginated DF, JSON or XML data returned by a web service

DF, JSON, XML data sources can now be configured to retrieve the data :ref:`in pages <vdp_admin_guide_path_types_pagination>`. The goal is to simplify retrieving data from services that impose a limit on the number of records returned by a single HTTP request.

.. rubric:: User Agent

.. #34742 Make the User-Agent configurable for HTTP requests for XML/JSON/DF wrappers

You can now change the value of the HTTP header ``User-Agent`` that is sent in the HTTP requests sent by DF, JSON, XML and Custom data sources. To change it, execute this statement from the VQL Shell:

.. code-block:: vql

   SET 'com.denodo.vdb.http.userAgent'='<new user agent>';

.. rubric:: Charset

.. #34741 Add support for configuring charset sent in Denodo HTTP Routes (using HTTPClient)

It is now possible to set the charset (UTF-8, ISO-8859-15...) with which DF, JSON, XML data sources and Custom data sources process data.

In previous versions, it is only possible to set the charset in DF data sources.

In previous versions, XML, JSON and Custom data sources detect the charset of the data on a best effort basis.

.. rubric:: OAuth Authentication: More Authentication Grants

.. #32867 Add support for more OAuth 2.0 authorization grants

There is now support for the following OAuth 2.0 authorization grants:

1.  Resource owner password credentials
#.  Client credentials grant
#.  Authorization code grant (already existed in Denodo 6.0)

These authorization grants are defined by the OAuth 2.0 standard (`RFC 6749 <https://tools.ietf.org/html/rfc6749>`_).

This will provide more compatibility with services that require OAuth 2.0 authentication.


.. rubric:: More HTTP Methods

.. Others #32508 Adding support for PUT, PATCH and DELETE in commons-connection
.. #25556	Add support to PUT, PATCH and DELETE http methods for the "Http Client" path type in DF, JSON, XML and Custom data sources

DF, JSON, XML data sources and Custom data sources can now be configured to send an HTTP request of any of these types: GET, POST, PUT, PATCH and DELETE. Previous versions only support GET and POST.


Execution Engine
================

New Data Types
---------------------------

.. #11248 Support for all the ANSI SQL-99 date/time data types

New data types for date and timestamp values: "localdate", "time", "timestamp" and "timestamptz". These types provide several advantages over the existing "date" type. "date" is still supported to keep backward compatibility.

The section :ref:`Data Types for Dates, Timestamps and Intervals` of the VQL Guide explains how queries that use these types have to be built, functions available to manage them, etc.


Massive Parallel Processing
---------------------------

Denodo provides native integration with several Massive Parallel Processing (MPP)
systems to speed up the execution of queries that involve processing billions of rows and cannot be done in "streaming mode".

The section :ref:`Parallel Processing` of the Virtual DataPort Administration Guide provides more information about this.

Query Optimizer
---------------

This section describes the improvements in the optimizer, when doing :ref:`automatic simplifications of the execution tree of a query <Automatic Simplification of Queries>`.

.. rubric:: Push GROUP BY under JOIN

.. #32447 Allow to push a group by under a JOIN even if there are no primary keys defined and without requiring a specific format for the join condition

The optimizer now pushes a GROUP BY under a JOIN if all these conditions are met:

1. It is an equijoin (i.e. the join conditions are like field_1 = field_2)
2. The fields of the primary key of one of the views involved in the JOIN are part of the join condition.


.. rubric:: Push Down GROUP BY that Uses Statistical Functions

.. #34253 Query optimizer should be able to push down a group by when it uses function stdev, stdevp, var, varp

The query optimizer now pushes down a GROUP BY that uses the functions stdev, stdevp, var or varp. This does not occur in previous versions.

.. rubric:: Push Down Joins over Views with Different i18n

.. #35303 Allow delegate joins and unions when the sub-views have different i18n if none of the sub-views uses the old date type

In previous versions, a join of two views whose data is obtained from the same database **is not** pushed down to that database if the underlying base views have different i18n.

In Denodo 7.0, a join of two views whose data is obtained from the same database **is pushed down** to that database, unless the underlying base views have different i18n **and** at least one of the views has one or more fields of type date (deprecated).

.. rubric:: Optimizer Reorders JOIN Operations and then GROUP BY Operations

.. #32779 Sometimes a group by is pushed down under a NJoin partially when it could be fully pushed after a join reordering

In Denodo 6.0, the optimizer pushes down the GROUP BY operations first and then, reorders the joins in order to maximize the delegation of operations (JOINs, UNIONs...) to the database.

In Denodo 7.0, the optimizer does this the opposite way: reorders the joins in order to maximize the delegation of operations to the database, and then tries to push down the GROUP BY operations first.

This change leads to more efficient execution plans because more operations are pushed down to the database.

For example, let us say we have the views SALES and DATE (data obtained from Redshift) and PRODUCT (data obtained from Vertica).

.. code-block:: sql

   SELECT count(*)
       ,SUM(sales.UNIT_PRICE)
   FROM sales
   JOIN product ON (sales.PRODUCT_ID = product."PROD_ID")
   JOIN date_dim ON (sales.date_id = date_dim.date_id)
   WHERE product."PROD_NAME" LIKE '%a%'
   GROUP BY sales.PRODUCT_ID

In Denodo 6.0, the GROUP BY is partially pushed down under the JOIN operations. Because of this change, the JOIN cannot be reordered.

In Denodo 7.0, the JOIN operations are reordered first. This allows for the JOIN of SALES and DATE to be fully pushed down to the database.

.. rubric:: Remove Restrictions for Pushing down a JOIN under a UNION

.. #33983 Union - Join push down not being applied in specific case

The optimizer is now less restrictive about when it pushes down a JOIN under a UNION:

-  In Denodo 6.0, all the fields of both branches of the UNION have to be associated.
-  In Denodo 7.0, only the fields that are used in the levels above have to be associated.

.. rubric:: Decrease Time to Calculate the Best Execution Plan

.. #34226 Several changes to reduce the time of the query optimization process

The optimizer calculates the best execution plan faster. This enhancement particularly benefits queries with a lot of operations that are pushed down to a database.

Cost-Based Optimizer
--------------------------------------------------

This section describes the improvements in the :ref:`cost-based optimizer`.


.. rubric:: New Alternative for GROUP BY over JOIN over Partitioned UNION

.. #32591 In queries where a group by can be pushed under a JOIN/UNION and the JOIN can be pushed under the UNION, the cost optimizer, should consider not only pushing both group by and JOIN but also push just the group by

In a query with a GROUP BY over a JOIN, and one of the branches of the JOIN is a partitioned UNION, the cost-based optimizer considers three execution plans:

1. Leave the execution plan as is.
#. Push down the GROUP BY over the JOIN, under the UNION.
#. *New in Denodo 7.0*: push down the GROUP BY under the UNION without moving the JOIN.

.. rubric:: Account for Data Movements Executed in Parallel

.. #33940 The cost optimizer should consider the datamovements as executed in parallel instead of sequential

The cost-based optimizer evaluates differently the cost of an execution plan with several data movements *that can be executed in parallel*:

-  In Denodo 6.0, the cost-based optimizer considers that all data movements of a query are executed in series, one after the other.

-  Denodo 7.0 takes into account that the data movements of the query can be executed in parallel. The effect is that the cost calculated by the optimizer is more accurate, which leads to the optimizer taking a better decision.


.. rubric:: Evaluate More Options Involving Data Movements

.. #35189 When a N-Join contains a sub-joins that can be pushed to a source, the data movement optimization should move the delegable sub-join instead of moving each individual JOIN branch

When the cost-based optimizer evaluates the cost of moving the data from one branch of a JOIN to the database of the other branch, it considers three execution plans:

1. Leave the execution plan as is.
#. Moving the entire branch to the other database.
#. *New in Denodo 7.0*: if the branch has a JOIN that be fully pushed down to its source, the optimizer will consider moving the result of the JOIN, instead of moving its branches.

This third option has a high impact if there are WHERE conditions over any of the branches of the JOIN. The main reason is that it reduces a lot the data that has to be moved from one database to the other.

Other Improvements
------------------------------

Performance improvements in:

.. #32188 Execution engine performance improvements in the evaluation of functions and conditions

-  The execution of conditions and functions.

.. #32534 Performance enhancements in merge join execution
.. #34612 Changes in the performance optimizations for merge join to improve response time of fast queries

-  The execution of MERGE JOIN and MINUS.

These changes reduce the amount of memory used to execute these operations and make them faster.

.. rubric:: Performance Improvements Inserting Data into Hive, Impala, Presto and Spark

There are several performance improvements in the process of inserting data into Hive, Impala, Presto and Spark.

.. #33278 Use Parquet format for Impala data movements

-  The data files sent to these database use now the Parquet format.

.. #34304 Movements to HDFS should avoid LOAD DATA INPATH

-  When creating tables in the database, they are created specifying the location of the files that will be uploaded. This prevents from having to execute the statement "LOAD DATA INPATH" in the database, after uploading the data to the data store.

.. rubric:: regexp Function: Better Error Message

.. #34386 More clear message when a regexp expression cannot be evaluated

The :ref:`regexp <REGEXP>` function returns a clearer error message when the input regular expression is invalid.

.. rubric:: Access to Metadata.


.. #35319 Improvements in the usage of metadata catalog connections
.. #34854 Minimized number of times VDP goes to catalog database to find view references information.


The Server now access the metadata less frequently and it does so faster.

.. rubric:: Interpolation Variables are Considered View Parameters

.. #34282 Fields coming from interpolation variables are managed as view parameters

In Denodo 7.0, interpolation variables are evaluated as view parameters so conditions like "<interpolation variable> = <value>", are considered an assignation of value and not a filtering condition.

For example, let us say that:

-  There is a JDBC base view "customer" created over a SQL query with an interpolation variable "zip_code".
-  There is a base view "sales"
-  You want to obtain all the sales of customers in the ZIP code 94301 *and* the sales for which you do not know the customer that did them.

.. code-block:: sql

   SELECT ...
   sales LEFT OUTER JOIN customer
   WHERE customer.zip_code = 94301

In Denodo 6.0 this query only returns the sales of the customers in the ZIP code 94301. The reason is that ``customer.zip_code = 94301`` assigns a value to the variable "zip_code" but also filters the results of the LEFT OUTER JOIN.

In Denodo 7.0, this query returns the results you want because ``customer.zip_code = 94301`` only assigns a value for the variable "zip_code", it is **not used** to filter the results of the LEFT OUTER JOIN.

In Denodo 6.0 it is possible to achieve the same behavior as in 7.0 by defining a view parameter in "customer" and in its WHERE condition, add a condition "zip_code = <view parameter>".

Cache Engine
============

.. rubric:: Cache Engine Supports More Databases

The cache engine now supports these databases:

.. #30185 Support for Presto database as JDBC data source, cache and data movement
.. #33104 Impala cache support
.. #33956 Hive cache support
.. #33977 Spark cache support

-  Hive 2.0
-  Impala 2.3
-  Presto 0.1
-  Spark 1.5, Spark 1.6 and Spark 2.x

.. rubric:: "Use Bulk Data Load APIs" Enabled by Default in Some Databases

.. #35731	Cache dialog should have bulk load enabled by default for certain datasources

In the cache settings, the option :ref:`Use bulk data load APIs <Bulk Data Load>` is now enabled by default for these data sources: Hive, Impala, Presto, Snowflake and Spark.

.. rubric:: Performance Improvements

.. #33710	Data movement should move only the necessary columns

During a :ref:`data movement <Data Movement>`, the cache engine creates a table in the target database. In previous versions, it creates a table with all the fields of the table in the source database, although later, it only inserts the data of the fields the query needs. In Denodo 7.0, the table of the target database is created just with the necessary fields.

.. rubric:: Oracle: Length of Fields Defined in Characters

.. #35183 Support for caching/moving data to Oracle using character as length semantic.

When the cache database is Oracle, the maximum length of the fields of type CHAR and VARCHAR2 is defined in characters. For example, if the *type size* of a field of a view in Denodo is 100, the type of that field in the table of Oracle for this field will be `CHAR(100 CHAR)`. This feature only applies to fields that have its length defined in their *Source type properties*.

In previous versions, the length is defined in bytes (e.g. `CHAR(100)`). This can cause the cache of a view to not be loaded if one of the text values has several multi-byte characters. When this occurs, the query that loads the cache returns an error.

.. rubric:: Return Error with 'cache_wait_for_load'='true', 'cache_load_on_error'='true'

.. #34622 Load cache queries with 'cache_wait_for_load' clause and with an error at insertion time are returned as erroneous.

The queries that load the cache of a view, with the parameters ``'cache_wait_for_load'='true', 'cache_load_on_error'='true'``, now finish with state ERROR. In previous versions, the query appears to finish successfully even if there is an error.

.. rubric:: Configure Cache Engine to Use a JDBC Data Source

.. #34008 Support for choosing an existent JDBC data source for cache

It is now possible to configure the cache engine to use an existing JDBC data source.


Denodo Stored Procedures
========================

.. rubric:: API

The API of stored procedures now provides methods to execute DDL statements like CREATE VIEW, ALTER DATASOURCE, DROP WEBSERVICE, etc. from a stored procedure, with the method `DatabaseEnvironment.executeVqlCommand(...) <https://community.denodo.com/docs/html/browse/7.0/vdp/javadoc/com/denodo/vdb/engine/storedprocedure/DatabaseEnvironment.html#executeVqlCommand-java.lang.String->`_.

.. rubric:: Timeout

.. #34636 Specify a particular "Stored Procedure Timeout" through a CONTEXT parameter.

When a procedure is invoked with the syntax ``CALL <stored procedure> ( <parameters> )``, it is now possible to indicate the parameter ``queryTimeout`` in the clause ``CONTEXT``. In previous versions, to set a timeout for a specific invocation to a procedure, you need to invoke the procedure with the syntax ``SELECT ... FROM <stored procedure> ( <parameters> )``.

Web Services
============

This section lists the new features in the web services published from Virtual DataPort.

.. rubric:: UserAgent Configurable
.. #34734 Support for custom UserAgent in REST published web service connections and CONNECT statement

The value of the HTTP header "User-Agent" of requests sent to SOAP and REST web services is passed to the monitoring layer of the Virtual DataPort server. The main benefit is that the applications that monitor the Virtual DataPort server (Denodo Monitor, Solution Manager, any other JMX application) will see that the property "UserAgent" of the query is set to the value of this header.

.. rubric:: OAuth 2.0 Authentication

.. #31720 Support for Oauth authentication in published web services

Added support for :ref:`OAuth 2.0 authentication <vdp_admin_guide_server_configuration_oauth_authentication>` in published web services (SOAP and REST).

.. rubric:: Parameter $expand

.. #30795 Add support for the $expand clause in the REST web services

New parameter in REST web services: :ref:`$expand <Parameters supported by the Denodo RESTful Web service and published REST Web services>`. This is equivalent to the clause EXPAND of the SELECT_NAVIGATIONAL statement.

.. rubric:: Replace/Drop Individual Elements

.. #33411 Add support to replace and drop single operations or resources in Web services.

Added support to replace or drop individual views from a REST web service, using VQL statements. In previous versions, you either do this from the administration tool or replacing the entire web service (``CREATE OR REPLACE WEBSERVICE``).

The same applies to operations of SOAP web services.

.. rubric:: Swagger
.. #20387 Add support to export REST web services to Swagger format

Denodo REST web services now publish an
:ref:`OpenAPI 2 / Swagger <OpenAPI / Swagger>` specification
that describes the available operations of the web service, and their input and output
schemas.

Administration Tool
===================

.. #24583 Close a tab by middle-clicking on the tab

The tabs can now be closed with the wheel button.

.. #35190 Avoid executing DESC USER statement for each existing user at login

The administration tool now executes less statements when establishing a connection with the Denodo server. The main benefit is that it connects slightly faster.

.. #33826 Add checkbox to VQL panels to allow including view statistics in the generated VQL

The panel to display the VQL of views now has an option to show the statistics of the view.



JDBC Driver
===========

.. #34837 Added property to the JDBC driver to report national character types (NCHAR, NVARCHAR, LONGNVARCHAR and NCLOB) as basic character types (CHAR, VARCHAR, LONGVARCHAR and CLOB respectively)

.. rubric:: New Property describeNationalCharTypesAsBasicTypes

The JDBC driver has a new property (:ref:`describeNationalCharTypesAsBasicTypes <Parameters of the JDBC driver and their default value>`) that allows a client application to report the fields of type NCHAR, NVARCHAR, NCLOB and LONGNVARCHAR as CHAR, VARCHAR, CLOB and LONGVARCHAR respectively.

.. rubric:: JDBC API: XML Data Types

.. #33862 Implemented JDBC-Driver methods related with SQLXML objects

The following methods are now supported by the JDBC driver:

.. code-block:: java

    ResultSet.getSQLXML(int columnIndex);
    ResultSet.getSQLXML(String columnLabel);
    PreparedStatement.setSQLXML(int parameterIndex, java.sql.SQLXML xmlObject);

.. rubric:: External Dependencies

.. #632 JDBC driver for VDP contains apache libraries (non basic version)
.. #33676 Removed dependency between Denodo VDP JDBC driver and commons-pool library
.. #35280 Remove unnecessary files in the JDBC driver internal dependencies
.. #33598 Removed dependency between Denodo VDP JDBC driver and log4j library

The JDBC driver no longer causes dependency conflicts with other open source libraries (log4j, commons-collections...)

In previous versions, there can be a conflict when the driver is loaded in an application that already includes these libraries, but an older version of them.

Administration
==============

This section lists the new features that are useful for administrators of Virtual DataPort.

.. rubric:: Obtain Information about Web Services

.. #35147 Add stored procedure GET_CATALOG_METADATA_WS to expose information about web services
.. #33103 Add stored procedure to expose information about web services

There are three new stored procedures that return information about web services:

1. :ref:`GET_CATALOG_METADATA_WS`: returns information about the REST and SOAP web services, its operations and parameters.
#.  :ref:`WEBCONTAINER_ELEMENT_STATUS`: returns information about the REST and SOAP web services and widgets that have been deployed in the web container of Denodo.
#.  WEBCONTAINER_META_INF: returns the current configuration of the web container of Denodo.

.. rubric:: Impose Quota Limits

.. #34920 Allow to define some quota limits in the Resource Manager

Administrators can now create a restriction in a :ref:`plan of the Resource Manager <Defining a Plan>` to limit to a user or role the number of queries per minute, hour, day or month.

.. rubric:: Active Loggers

.. #34917 Create a stored procedure to get log categories with a value specified in log level: GET_ACTIVE_LOGGERS()

There is a new stored procedure ``GET_ACTIVE_LOGGERS()``. It returns information about the log categories that have been set in the Virtual DataPort server. It returns the categories set in the file :file:`{<DENODO_HOME>}/conf/vdp/log4j2.xml` and the ones set by the procedure :ref:`LOGCONTROLLER`.


.. rubric:: Enhancements when Exporting Metadata to VQL Files

.. #34587 Request to include default configuration properties during export

-  When an administrator exports the metadata of the whole Server (not only a few databases), the VQL file includes the values of all the properties of the Denodo server. In previous versions, only some properties and the properties added with the command ``SET`` are included in the VQL file.


.. #34376 Add the version of the VDP Server in the VQL file when exporting metadata

-  All the VQL files now include a comment at the top saying which version and update of Virtual DataPort server generates the VQL.


.. #28795 Add export option to the context menu when user selects multiple views.

-  You can now obtain the VQL of several elements with a single statement: :ref:`DESC VQL LIST <Syntax of the statement DESC>`.


.. #24348 encrypt_password escape character

-  The command ``ENCRYPT_PASSWORD`` encrypts a password so you can use it as the password of a data source. This command has now the parameter :ref:`FOR_PROPERTIES_FILE <Example usage of the command ENCRYPT_PASSWORD>`, which generates the password with certain characters escaped, as needed in a properties files.

.. rubric:: Changes in Underlying Sources

.. #34248 source_changes stored procedure needs another input parameter for database

-  There is a new stored procedure: :ref:`GET_SOURCE_CHANGES`. This procedure can search for changes in the underlying source of base views of any database. :ref:`SOURCE_CHANGES` (already available in previous versions) can only search in the current database.


.. #34111 Add support at base view level for retaining source type properties after source refresh

-  When doing a source refresh of a base view from the administration tool, the types, properties and descriptions of the fields are no longer replaced with the information from the source. This allows for users to keep the information they manually set on the base view.


.. rubric:: VIEW_DEPENDENCIES Procedure

.. #33558 VIEW_DEPENDENCIES() procedure does not return information about whether a dependency of type 'storedprocedure' involves a predefined stored procedure

The stored procedure :ref:`VIEW_DEPENDENCIES` now allows to distinguish between stored procedures developed by the user and stored procedure included in Denodo "out of the box". The column ``dependency_type`` returns a different value depending on the type of the dependency.


Privileges
==========

.. rubric:: New Privileges

.. #10365 Make data sources visible to non-admin users and add permissions for data sources
.. #35410 Some column names of the procedure CATALOG_PERMISSIONS should be changed.

The privileges system has been upgraded to provide a more fine-grained control over what actions the users are allowed to do. The privileges you can grant to users or roles over databases are:

-  Administrator of the database
-  Connect
-  Create (includes "Create data source", "Create view", "Create data service", "Create folder")
-  Create data source (new)
-  Create view (new)
-  Create data service (new)
-  Create folder (new)
-  Metadata (new)
-  Execute
-  Write

In previous versions you can only grant the privilege "Create". Now you can control what types of elements users are allowed to create.

You can grant the following privileges over data sources, views, REST and SOAP web services, and widgets:

-  Metadata
-  Execute
-  Write
-  Insert
-  Update
-  Delete

In previous versions, you cannot grant privileges over data sources, nor web services, nor widgets, nor stored procedures. Only administrators, administrators of the database and the owner of these elements can modify them.

Now, the ownership of an element is not taken into account when deciding if a user is allowed to modify/delete an element.

The output parameters of the procedure :ref:`CATALOG_PERMISSIONS` have changed accordingly to these new privileges.

.. rubric:: CONNECT Privilege

.. #33924 Change default value of property added in feature #32318

In Denodo 7.0 you have to grant the CONNECT privilege to a user/role over a database in order to allow this user to access any of the elements of that database. The other privileges over the database or its elements are disregarded if the user/role does not have the privilege CONNECT.

In previous versions, the CONNECT privilege was only checked when establishing the connection.


.. rubric:: FILE Privilege

.. #35463 FILE privilege checking enabled by default

The :ref:`FILE privilege <The FILE Privilege>` is now enabled by default.

.. rubric:: Default Roles

There are new default roles that are created by default and cannot be deleted:

.. #35270 Add new role 'webpaneladmin'
.. #33751 Adding a new role for enabling users to execute Denodo Solution Manager administrative operations
.. #33171 Add a new role that allow to promote specific metadata from a source environment to a target environment

-  data_catalog_admin :sup:`*-1`: Grants the privilege of modifying the settings of the Denodo Data Catalog.
-  data_catalog_exporter :sup:`*-1`: Grants the privilege of exporting the results of a query, from the Denodo Data Catalog.
-  solution_manager_promotion_admin: Grants the role "Promotion Administrator" over the Solution Manager administration tool.
-  solution_manager_admin: Grants administration privileges over the Solution Manager administration tool.
-  web_panel_admin", "Grants administration privileges over the web panel.


:sup:`*-1`: The roles "data_catalog_admin" and "data_catalog_exporter" are equivalent to "selfserviceadmin" and "selfserviceexporter" respectively. The latter ones already exist in previous versions, where the "Data Catalog" is called "Information Self-Service Tool". The new roles have been created for consistency with the new name of this tool.

.. rubric:: Creating of Jars and Maps

.. #33874 Creation of jars and internationalization maps should be restricted to administrator users.

Only administrators can import and delete jars, and create, modify or delete internationalization maps. The reason for imposing this new restriction is that modifying these elements can potentially affect elements of all the databases.


Monitoring
==========

.. rubric:: Users with the jmxadmin Role

From now on, only administrators or users with the role ``jmxadmin`` can connect to the :ref:`JMX interface <Monitoring with a Java Management Extensions (JMX) Agent>` of Denodo. In previous versions, all users could do it but the actions they could do were limited.

.. rubric:: Query Monitor

.. #35338 JMX Access will restricted for jmxadmin users only. Query monitor will operate through RMI instead

The :ref:`Query Monitor` no longer lists all the queries running, to users with the role ``jmxadmin``. In previous versions, administrators and users with this role can see all the queries that are running. Now, only administrators and users with the role "serveradmin" can do this.

Now this role only affects who can connect to the JMX interface of the Denodo server.

Starting with the update of October 2018 of Denodo 7.0, users with the role ``jmxadmin`` **can** use the Query Monitor.

.. rubric:: Denodo Monitor

.. #26987 Enhancement: Add shutdown script for Denodo Monitor

-  Added a script for shutting down the Denodo Monitor. In previous versions it has to be stopped by killing the process.


.. Denodo Monitor #32690 Add support for ping data sources

-  Added support for pinging data sources to make sure they are still available. This mechanism is not enabled by default. To enable it, modify the properties ``vdp.datasources.ping`` and ``vdp.datasources.ping.timeout`` of the file ``ConfigurationParameter.properties`` in the Denodo Monitor.

Denodo4E Eclipse Plugin
=======================

.. Denodo4E	#34331 Eclipse Oxygen Support

The Denodo plugin for Eclipse now supports Eclipse Oxygen.

Others
======

.. rubric:: Improvement in the Authentication of Users

.. #34973 Add a roles cache for LDAP authorization

Virtual DataPort now caches in memory the roles of the users that connect to Denodo using LDAP authentication or Kerberos authentication. This will reduce the connection time for clients.


.. #34783 Performance improvements when importing VQL with properties

Importing a VQL file with a properties file now consumes less memory.

.. #34293 Support for OAuth2 authentication in CONNECT statements

The CONNECT statement now supports passing an OAuth 2.0 token, in addition to allow user/password and a Kerberos token.


.. -------------------------------------------
.. Redmines that are not going to be documented
.. -------------------------------------------

.. csantos@2018/03/15: not documented because the change is very minor.
    .. #30792 Review the case in the Server explorer tab title
    .. #32982 Remove ARN connector from VDP admin tool wizard
    .. #34574 Trim Server Principal Name in the Kerberos Server configuration
    .. #34888 In the JDBC data source dialog, the check box *Use Oracle Proxy Authentication...* has to be aligned with the other check boxes of the dialog
    .. #34918 Remove the class AdminMultiLineToolTip identified by BlackDuck as a code match with the component "Multi-line ToolTip by Zafir Anjum".
    .. #35672 Increase default width for ServerCondigurationDialog
    .. #33695 Interface view - Definition tab: now, when clicking Restore fields, the Tool restores the field in the same order as they were before, instead of a random order
    .. #33232 Conditions wizard of the VDP admin tool should propose DATE LITERAL values instead of Strings for dates
    .. #30282 Add better descriptions to the default Roles




.. csantos@2018/03/15: Not documented because the driver was always returning false (it was checking if the field was of type "money".)
        .. #33982 Method "VDBJDBCResultSetMetaData.isCurrency(int)" should return false always


.. csantos@2018/03/10: I am not documenting bugs/enhancements over features that were not in Denodo 6.0.
    .. #35428 Unable to assign a datetime literal expression to a view parameter with type date (old date)
    .. #35186 Removed reserved words introduced with enh #32843 from the JDBC wrapper parser
    .. #35426 Disable the restriction about using interval types in view schemas
    .. #35428 Unable to assign a datetime literal expression to a view parameter with type date (old date)
    .. #35426 Disable the restriction about using interval types in view schemas
    .. #35186 Removed reserved words introduced with enh #32843 from the JDBC wrapper parser
    .. #35114 Allow implicit cast from old date type to the new types
    .. #35113 Modify SUBTRACT operation to return the number of days when the operands are of any datetime type
    .. #35311 Improvements in DESC VQL LIST command


.. csantos@2018/03/15: Not documented because users will not notice or it does not affect them in anyway
    .. #32193 Improvements in delegation to JDBC sources for literals/functions
    .. #35675 Add information for identifying problems with decimal in data movement
    .. #33734 Removed not implemented classes from the JDBC-Driver.
    .. #34674 Prevent execution cache information to remain stored at the session after an error accessing the cache
    .. 33504	Upgrade Jersey 1.x library to the latest version, from 1.15 to 1.19 (so REST WS can be deployed in external webcontainers that run Java 8)
    .. #33597 Improve metadata database shutdown so db.lck file is being deleted after server normal shutdown
    .. #35533	Removed unnecessary info about FILE privilege when describing (DESC) local admin users
    .. #35512	Warning Messages in Denodo Express Installation
    .. #35325 Remove com.denodo.vdb.vdbinterface.server.VDBManagerImpl.securityManagerEnabled property from configuration file
    .. #34169 Remove SourceIntrospectionServiceFactory,  SourceIntrospectionServiceUtilFactory, AdminSourceIntrospectionServiceUtil and the related properties
    .. #33820	Modified HashMap to LinkedHashMap used for storing JDBC driver properties in a JDBC data source
    .. #33583 Adapt VDP VCS plugins and classes to use the new denodo-commons-vcs library
    .. #32587 Improve the readability of the name of the export properties
    .. #34675 Refactor ViewStatSummaryVO to not use java 8 clases, so denodo-vdp-client can be compiled with java 7
    .. #34169 Remove SourceIntrospectionServiceFactory,  SourceIntrospectionServiceUtilFactory, AdminSourceIntrospectionServiceUtil and the related properties
    .. #33663 Removed old methods from IQueryExecutor related with query executions.
    .. #33600 Avoid possible ClassCastException in order by fields stored in executing Context
    .. #33481 Define the system property "javax.xml.parsers.DocumentBuilderFactory" to ensure VDP creates the appropriate "DocumentBuilderFactory"
    .. #33506 I18n and simple maps are not being stored in the metadata cache the first time they are loaded
    .. #33087 Removed legacy XML serialization for VDP catalog
    .. #32393 Add the property com.denodo.security.ssl.trustStorePassword commented to the VDBConfiguration.template so users know they can change this
    .. #32057 Modified web service operations in ModifiedElementsVO should be inserted in a stable order
    .. #32195 New stored procedure to help in the process of certificating new sources.
    .. #31241 JDBC and ODBC parsers: remove the support for the source configuration properties ORDERBYCOLLATION and DELEGATEORDERBYCOLLATION
    .. #31240 JDBC and ODBC parsers: remove the support for MINEVIDECTABLETIME because it has a typo. Denodo server continues supporting MINEVICTABLETIME
    .. #33067 Remove the non-optimized working mode from the API.
    .. #33113 Improvements processing requests management information
    .. #33879 Use GSSAPI instead SSPI for Kerberos authentication over ODBC


.. csantos@2018/03/15: Currently, we do not document how/which functions are delegated to sources
    .. #35206 Improve delegation of datetime functions using the new datetime types to Hive
    .. #35221 Improve delegation of datetime functions using the new datetime types to Spark


.. csantos@2018/03/15: I do not document because it is complex to explain and it is an uncommon problem.
    #35198 Condition over a generic adapter are not being delegated on derived views



.. csantos@2018/03/09: no lo voy a comentar porque es algo que los usuarios no pueden notar y me parece que queda mal comentar que antes lo hacíamos así.

    .. #33505 Improve performance for cache insertion by computing the SQL for insertion only once

.. csantos@2018/03/19: this is actually a bug
    #35317 Allow CASE statements in ORDER BY clauses for analytical functions for all the fields
    .. #32275 Changing algorithm for ciphering data source passwords to AES128



.. csantos@2018/03/09: currently we do not document log categories
    .. #34106 Added log information printing rows which are being inserted in cache
    .. #36044 - VDP Server-Cache Engine - Modified log category for showing that a source does not support foreign keys introspection


.. csantos@2018/03/09: currently we do not document what or how functions are delegated to databases
    .. #35135 Add support to delegate GETQUARTER() and GETWEEK() functions to DB2
    .. #34849 Support for analytic LISTAGG function in db2, snowflake and amazon redshift
    .. #34800 Request to delegate FIRSTDAYOFMONTH
    .. #35389 Allow delegate addday function to postgresql when the parameter amount is not a literal
    .. #35260 VDP datasources escape char (the identifier is now ` and not "`)
    .. #35091 'Comment' is not recognized as a reserved keyword in the Sybase version 16.
    .. #35616 Improve delegation of datetime functions using the new datetime types to Greenplum
    .. #35547 Improve delegation of datetime functions using the new datetime types to Netezza
    .. #35331 Improve delegation of datetime functions using the new datetime types to SnowFlake
    .. #34800 Request to delegate FIRSTDAYOFMONTH
    .. #34296 Delegate 'getdaysbetween' to redshift


.. csantos@2018/03/15: I do not think document this is worth.
    .. #33605 Do not copy the JDBC drivers for Oracle and PostgreSQL to extensions/thirdparty/lib


.. csantos@2018/03/15: Do not document. This is only useful for proddev.
    .. #35606 Provide additional logging information for SSL connections
