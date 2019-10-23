==========================================
7.0 GA: New Features Common to All Modules
==========================================

This section lists the new features related to all the modules of the Denodo Platform |version| GA. For the list of features included in the updates of Denodo |version|, check their RELEASE NOTES.

.. contents::
   :local:
   :backlinks: none

New Tool: Solution Manager
==========================

The :ref:`Solution Manager <Solution Manager Administration Guide>` is a new component of Denodo 7.0. It consists of several subcomponents that work together to help administrators manage large Denodo deployments. Its two main goals are:

1. Manage the licenses of all the Denodo servers of your organization. 
#. Web tool for developers and administrators to promote changes from the development environment to the other environments.

It provides a web interface to administer it.

As the licenses of all the Denodo servers are managed from just one place (the Solution Manager), administrators will now get a single license file that contains all the licenses of the organization.

New Tool: Web Panel
===================

The :ref:`Web Panel <Web Panel Guide>` is a new component of Denodo 7.0. It is a web application that provides a single point of entry to all the Denodo web tools. This will facilitate users to access all the available tools (Data Catalog, Scheduler Administration tool, Diagnostic and Monitoring tool and Solution Manager). It can be installed along with the Solution Manager. 

Control Center
==============
.. Control Center	#28087	Denodo Control Center redesign

The UI of Control Center has been redesigned to make it more user friendly.

| 

.. #Control Center	34730	Performance improvements for the Control Center: it starts up faster. Starting time was an issue in hosts where several network interfaces.

The Control Center starts faster than in previous version. This is particularly noticeable in hosts that have several network interfaces.

New Supported Platforms
=======================

.. #34309 Support to Windows Server 2016

Added support for Windows Server 2016

Data Catalog
============

The "Information Self-Service Tool" has been renamed to "Data Catalog".

|

When Kerberos authentication is enabled and only one Virtual DataPort server has been configured, it tries to auto-login the user without her intervention.

|

The Data Catalog has been revamped to make it more powerful and more user friendly for business users.

|

Now, the Data Catalog "synchronizes" its metadata about databases, views and web services with one or several Virtual DataPort servers and store them locally. This way, the searches in the catalog are faster.

| 

It shows information about web services as well as views, and it allows to execute the REST web services. In previous versions, it shows information about views but not web services.

| 

Users can now modify the descriptions of the catalog elements stored locally (views and its fields, databases, and web services and its parameters). These descriptions are stored in the Data Catalog but are not sent to the Virtual DataPort server. This way, the Data Catalog can show descriptions more appropriate for business users if required (i.e. if the descriptions stored in the Virtual DataPort server are oriented to the developers).

|

The catalog can be synchronized with the Virtual DataPort server at any moment. When synchronizing the catalog, the administrator can choose to overwrite or maintain the descriptions of the elements edited from the Data Catalog.

| 

The Data Catalog now allows creating tags and categories. Users with the appropriate privileges can classify the views and web services in categories, and assign them tags. Users can browse the catalog of views and web services by categories, tags, databases and folders, or relationships between the views.

|

The user can search in the catalog (metadata), but also in the contents of the views (data) if they were previously indexed using jobs of type VDPIndexer. The searches can be executed across all the databases of the server (in previous versions, users can only search on a single database), and can be configured using several search filters.

|

The query wizard has been redesigned to make it more intuitive for business users. It also allows executing REST web services.

|

More personalization options are now available for administrators. For example, they can specify which elements from the catalog the users are allowed to see (only views, only web services, or both), choose whether to allow users to browse by Databases/Folders, and choose to hide uncategorized views and web services from users.


Diagnostic & Monitoring Tool
============================

.. #35396 Add an "auto-login" page to use with Kerberos authentication
.. #35459 Try to "auto-login" when using Kerberos authentication

When Kerberos authentication is enabled, it tries to auto-login the user without her intervention.

.. # 33639 Add support for ping data sources (QUESTION: no sé cómo describir esto. Que lo escriba alguien más)

OData Service
=============

The :ref:`Denodo OData 4.0 Service <Denodo OData 4.0 Service>` is now included with the Denodo Platform. In previous versions this is a *Denodo Connect* component that has to be downloaded from the Denodo support site.

Others
======

The Denodo Platform now ships with the Java Runtime Environment (JRE) version 8. The Denodo servers and its tools cannot run with earlier Java versions. 

.. Installer #33601 Add the necessary odbc classes to use the jdbc-odbc bridge with java 8 and later. This would allow to still support Access files.
.. # 33621 Add the necessary odbc classes to use the jdbc-odbc bridge with java 8 and later

Note that Microsoft Access files still are supported as a data source because the component JDBC-ODBC Bridge has been included in Denodo.

|

.. #31549 Force jars in lib/contrib (and others) to be loaded into the classpath in a specific order

The order in which the jar files of a directory are loaded into the classpath is now deterministic (alphabetical order in ascending order). In previous versions the order is random which can cause intermittent errors when two conflicting jars are placed in the same directory. With this change, the error will either not occur or always occur, which will make it easier to fix.

| 

.. #Installer #33092 #Migrate to log4j2. All the components use now the log4j version 2 library. In the Denodo 6.0, only the Denodo server used it while the previous ones used log4j 1.x
.. #36041 Maintain compatibility with the Platform extensions and applications that are installed on the internal Tomcat using log4j 1.x


All modules of the Denodo Platform use now Apache Log4j 2 that provides significant improvements over its predecessor, Log4j 1.x. In previous versions, only the Virtual DataPort server uses Log4j 2 while the other modules still use Log4j 1.x. All the extensions developed with previous versions of the Denodo Platform that used Log4j 1.x will continue working without any modification because the Denodo Platform contains a bridge between the two versions of Log4j.

|

.. #Installer	32726	Update the distributed version of YAJSW to its latest stable release. Allows to (QUESTION: something about creating a something of more than 48 cores)

It is now possible to `limit the number of cores  <https://community.denodo.com/kb/view/document/How%20to%20limit%20the%20number%20of%20processors%20used%20by%20Denodo?category=Operation>`_ used by a Denodo component that is launched as a Windows service. In previous version, the maximum limit that it can be set is 31 cores. In Denodo 7.0 this limit can be any value.

| 

.. #34331 - Eclipse Oxygen Support

The Denodo4e Eclipse plugin provides support for Eclipse Oxygen (version 4.7)

Kerberos Authentication
-----------------------

.. #34200 Include the Kerberos tools kinit, ktab and klist in the JRE of the Denodo Platform

The Java Runtime Environment (JRE) included with the Denodo Platform for Windows now contains the following tools for Kerberos:

-  ``kinit``: used to obtain and cache Kerberos ticket-granting tickets.
-  ``ktab``: lists, adds, updates and deletes principal names and key pairs from keytab files.
-  ``klist``: displays the entries in the local credentials cache and keytab files.

These tools are very useful to debug problems with Kerberos authentication. 

The JRE included in previous versions of Denodo does not contain these tools. Users had to download a separate JRE to use them.

|

.. DOC#34278 - New option to activate the JCE: 
.. From: https://www.oracle.com/technetwork/java/javase/8u161-relnotes-4021379.html 
    .. "security-libs/javax.crypto
     Unlimited cryptography enabled by default

    .. The JDK uses the Java Cryptography Extension (JCE) Jurisdiction Policy files to configure cryptographic algorithm restrictions. Previously, the Policy files in the JDK placed limits on various algorithms. This release ships with both the limited and unlimited jurisdiction policy files, with unlimited being the default. The behavior can be controlled via the new 'crypto.policy' Security property found in the /lib/java.security file. Please refer to that file for more information on this property."

It is no longer necessary to install the *Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files*.

In previous versions, it is necessary to download and install these policy files when enabling Kerberos authentication *and* if the service account for the Denodo servers in the Active Directory uses AES-256 encryption. In Denodo 7.0, these policy files are already included and unlimited strength is enabled by default.