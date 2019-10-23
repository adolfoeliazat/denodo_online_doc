============================================================================
Features Removed in Virtual DataPort 7.0
============================================================================

This section lists the features that are no longer available in Denodo
7.0.

.. contents:: 
   :local:
   :backlinks: none


I18n Settings
=============

I18n Settings of the Cache Engine
---------------------------------

The property ``com.denodo.vdb.cache.i18nCode`` has been removed. Even if it is present in the VDBConfiguration.properties file, its value is ignored. Instead, Denodo use the "Default i18n" of the Server (configured in the "Server configuration") to read/write values from/to the cache that need an i18n.

|

Starting with Denodo 7.0, when you change the i18n of the Denodo server, Denodo invalidates the cache of the views that have one or more fields of type timestamptz and/or date (deprecated type). 

We are referring to when you do it from the administration tool or when you execute this statement:

.. code-block:: vql

   SET 'com.denodo.vdb.misc.I18nParametersManager.i18nDefault'='<new i18n map>';


Databases and Cache Do not Have I18N
------------------------------------

.. #34256 Remove custom i18n support for database

The Denodo databases and cache no longer can have a different i18n setting. Now all of them have the same i18n setting.


VQL Syntax
============================================================================

.. #33507 Remove support to specify identifiers surrounded by literal quotes

Using single quotes to surround identifiers is no longer valid. Although deprecated in Denodo 5.0, prior versions still allow it.

For example, this query is no longer valid.

.. code-block:: sql

   SELECT *
   FROM 'admin'.'iinc_sqls'
   WHERE 'iinc_sqls'.'summary' = 'Bandwidth increase';

When necessary, the identifiers have to be surrounded with double quotes. For example:

.. code-block:: sql

   SELECT *
   FROM "admin"."iinc_sqls"
   WHERE "iinc_sqls"."summary" = 'Bandwidth increase';

JDBC Data Source Dialog: Choose Automatically
============================================================================

.. #31168 Remove the check box "Choose automatically" from the JDBC data source dialog

The check box *Choose automatically* of the JDBC and ODBC data sources has been removed.

ODBC Data Sources
=================

.. #33617 Remove SQL Server and Excel from the ODBC adapters selector

The following ODBC adapters have been removed:

-  Microsoft Excel. Instead, create an Excel data source.

-  Microsoft SQL Server. Instead, create a JDBC data source with one of the adapters for SQL Server.

JDBC/ODBC Source Configuration Properties
==========================================

The VDP server will no longer accept ``minevidectabletime``. We will continue supporting ``minevictabletime``, which is
the right way of spelling it.

The source configuration properties ``orderbycollation`` and ``delegateorderbycollation`` are no longer supported.
The server supported them so VQL files from versions prior to 6.0 could be imported without errors but in this version
the support for these properties will be removed.

XMLA Connectors for SAP
============================================================================

The XMLA adapters for SAP (i.e. “SAP BW 3.x (XMLA)” and “SAP BI 7.x
(XMLA)”) have been removed. Use the adapter “SAP BI 7.x (BAPI)” instead.


Comparison of Strings: Case Sensitive
============================================================================

.. #31243 Remove the capability of configuring VDP to perform case insensitive comparisons of strings

Starting with Denodo 5.0, the **comparison of strings** on Virtual
DataPort is case-sensitive. In previous versions it was possible to go back to the previous behavior (case-insensitive comparisons). In Denodo 7.0 it is no longer possible. 

Performing case-insensitive comparison has a significant computational cost when dealing with huge volumes of data.


Published Web Services: “with LDAP” Authentication
============================================================================

.. #31378 In REST and SOAP web services, remove support for the authentication methods "HTTP Basic with LDAP" and "WSS Basic with LDAP"

The following authentication methods of published web services have been removed:

-  For REST web services, “HTTP Basic with LDAP”.
-  For SOAP web services, “HTTP Basic with LDAP” and “WSS Basic with LDAP”.

When migrating to Denodo 7.0, web services with these authentication methods must be edited using the Administration Tool before exporting the VQL,
assigning any of the authentication methods supported in Denodo 7.0. In other case, the VQL generated will not be valid to import in 7.0.

Export Script
=============

.. #31262 Remove the parameters "legacyRepositoryLayout" and "caseSensitive" from the export script

The parameters ``legacyRepositoryLayout`` and ``caseSensitive`` of the export script have been removed.

Denodo JDBC Driver
============================================================================

Basic Version
-------------

The *basic* "flavor" of the JDBC driver has been removed. From now on, there is only one "flavor" of the driver: :file:`{DENODO_HOME}/tools/client-drivers/jdbc/denodo-vdp-jdbcdriver.jar`.

Internal Pool of Connections
----------------------------

The Denodo JDBC driver was capable of creating its own pool of
connections to Virtual DataPort. This feature has been removed. The implication is that the parameters 
``poolEnabled``, ``initSize``, ``maxActive`` and ``maxIdle`` are no longer be valid.

Applications that need to create a pool of connections to Virtual DataPort should use their own library to do it.

Kerberos Constrained Delegation
-------------------------------

.. #31495 Remove the property requestCredDeleg from the jdbcdriver

The property ``requestCredDeleg`` is no longer supported. Instead, use the property
``userGSSCredential`` to pass the impersonated credential to the driver.

Denodo ODBC Driver
==================

The parameter ``com.denodo.vdb.vdbinterface.server.odbc.forceBinaryTypes`` (described in the documentation of Denodo 6.0, section `Increasing the Performance of the Denodo ODBC Driver <https://community.denodo.com/docs/html/browse/6.0/vdp/developer/access_through_odbc/integration_with_third-party_applications/increasing_the_performance_of_the_denodo_odbc_driver>`_) has been removed.

The speed improvement provided by this option was insignificant and after enabling it, ODBC clients only could use the Denodo ODBC driver.

Others
======

.. Installer #33315 Remove the .exe files from Windows installations

The "exe" files of <DENODO_HOME>/bin have been removed (i.e. vqlserver_startup.exe, denodo_platform.exe, etc.). This prevents confusion among newer users that did not know if there is any difference between both (there is not). Use the equivalent .bat scripts (vqlserver_startup.bat, denodo_platform.bat, etc.) The .bat scripts provide the same functionality as the exe.
