========================================================
Changes in the Privileges System in Virtual DataPort 7.0
========================================================

Denodo 7.0 introduces significant changes in the privileges system to allow granting more fine-grained privileges to users and roles. Administrators can now grant privileges over these elements:

-  Data sources.
-  Web services.
-  Widgets.

In previous versions, only users with the privilege write over a database could create these elements.

|

The Create privilege is now more fine-grained. Instead of granting Create, administrators can grant these to users:

-  Create folder
-  Create data source
-  Create view
-  Create data service (web services, widgets and JMS listener)

Granting Create automatically grants all of these.

This section describes briefly all the privileges available, and the changes to them. 

Detailed Description
==================================

In Denodo |version|, the privilege "Read" has been renamed to "Execute" to clarify its behavior. In VQL statements, the ``READ`` token can still be used to keep backward compatibility with existing VQL scripts.

Find below the privileges that can be granted over each type of element.

Privileges over Databases
--------------------------

The privileges that can be granted to a user/role over a database are:

-  Connect
-  Create (includes all the Create privileges below)

   -  Create data source
   -  Create data service (web services, widgets and JMS listeners)
   -  Create folder
   -  Create view

-  Metadata
-  Execute
-  Write

Privileges over Data Sources
----------------------------

The privileges that can be granted to a user/role over a data source are:

-  Metadata
-  Execute: users with this privilege and "Create view" over the database, can create base views over this data source.
-  Write: required to perform these actions:

   -  Do a "source refresh" of the base views of this data source. The user also needs the write privilege over the base view.
   -  Obtaining the VQL of the data source
   -  Copying the data source
   
   When a user creates a data source, it automatically gets the Write privilege over it.

Privileges over Views
------------------------

The privileges that can be granted to a user/role over a view are:

-  Metadata
-  Execute
-  Write: required to perform these actions:

   -  Obtain the VQL of the view. The user also needs the privilege Execute over the views directly referenced by this view.
   -  Copy the view.
   
   When a user creates a view, it automatically gets the Write privilege over it.
   
-  Insert
-  Delete
-  Update


Privileges over Data Service (Web Services, Widgets and JMS Listeners)
-------------------------------------------------------------------------

The privileges that can be granted to a user/role over a data service (web service, widget and JMS listener) are:

-  Metadata
-  Write: required to deploy a web service, a widget or enable a JMS listener. When a user creates a web service, it automatically gets the Write privilege over it.


Privileges over Stored Procedures
---------------------------------

The privileges that can be granted to a user/role over a stored procedure are:

-  Metadata
-  Execute
-  Write: required to perform these actions:

   -  Obtain the VQL of the procedure
   -  Copy the procedure

CATALOG_PERMISSIONS Procedure
=============================

In Denodo 7.0, the output fields of the stored procedure :ref:`CATALOG_PERMISSIONS` have changed to match the new privileges added in this version.

The following output fields have been renamed:

-  ``dbread``: renamed to ``dbexecute``.
-  ``viewread``: renamed to ``elementexecute``.
-  ``viewwrite``: renamed to ``elementwrite``.
-  ``viewinsert``: renamed to ``elementinsert``.
-  ``viewdelete``: renamed to ``elementdelete``.
-  ``viewupdate``: renamed to ``elementupdate``.

The following output fields have been added:

-  ``elemensubtype``.
-  ``dbcreatedatasource``.
-  ``dbcreatedataservice``.
-  ``dbcreateview``.
-  ``dbcreatefolder``.
-  ``elementmetadata``.

Because some of the output fields have been renamed, you may have to change the queries that invoke this procedure.

Creating Internationalization Maps and Importing Jar Extensions
===============================================================

Starting with Denodo 7.0 only administrators of the Virtual DataPort server can:

-  Create internationalization maps (i18n maps)
-  Import jar extensions

Restoring the Behavior of Prior Versions
========================================

In Denodo 7.0, the owner of the element is not taken into account when checking if the user can perform an action or not; Only the privileges granted to the user are taken into account.
In previous versions, being the owner of a data source or data service was equivalent to having the privilege "Write" over them.

To restore the behavior of previous versions, execute this from the VQL Shell:

.. code-block:: vql

   SET 'com.denodo.vdb.security.requirePrivilegesToOwners' = 'false';
   
Restart the Virtual DataPort server to apply the change in this property.

Note that when a user creates a data source, a view or a data service, it automatically gets the privilege Write over them. However, this privilege can be later revoked.

|

In previous versions, users do not need the privilege Connect over a database to use elements of that database as long as they have the privilege to:

1. Connect to another database
#. Having the right privilege over the elements of the other database.

In Denodo 7.0, users need the privilege Connect over a database to use elements of that database.

To restore the behavior of previous versions, execute this from the VQL Shell:

.. code-block:: vql

   SET 'com.denodo.vdb.security.requireConnectOnCrossDatabaseAccess' = 'false';
