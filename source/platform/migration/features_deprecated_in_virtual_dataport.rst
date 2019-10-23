============================================================================
Features Deprecated in Virtual DataPort 7.0
============================================================================

Due to the introduction of new features or changes in existing ones, some features of Denodo that 
are available in previous versions have been deprecated.

Deprecated features are supported in the current version. However, they are not actively maintained and may be removed in the next major version 
of the Denodo Platform. We *strongly* recommend planning to stop using deprecated features.

This section lists the features that are currently deprecated in Denodo Virtual DataPort |version|.
This list is updated whenever a feature is marked as deprecated.

**Last update of this list**: |today|.

.. contents:: 
   :local:

Data Type "Date"
============================================================================

Starting with Denodo 7.0, the data type “date” is deprecated, although is maintained for compatibility reasons. The fields with this data type will behave in the same way as in prior versions 6.0. Instead use the new types: localdate, time, timestamp or timestamptz.
 
VQL Syntax
============================================================================

Contains Operator
-----------------

Starting with Denodo 7.0, the operator `contains <https://community.denodo.com/docs/html/browse/6.0/vdp/vql/appendix/support_for_the_contains_operator_of_each_source_type/support_for_the_contains_operator_of_each_source_type>`_ has been deprecated.

SQLFRAGMENT Clause
------------------

Starting with Denodo 6.0, the ``SQLFRAGMENT`` clause of the ``CREATE WRAPPER JDBC`` statement has
been deprecated.

Syntax ALTER TABLE <base view name> ADD SEARCHMETHOD
-----------------------------------------------------

Starting with Denodo 6.0, the syntax ``ALTER TABLE`` <base view name> ``ADD SEARCHMETHOD`` is
deprecated.

When creating a base view, define the search methods in the
``CREATE TABLE`` statement instead of executing a ``CREATE TABLE`` and
then, adding the search methods with the statement ``ALTER TABLE``.

This reduces the number of statements Virtual DataPort has to process.

For example, instead of running this:

.. code-block:: vql 

   CREATE TABLE internet_inc I18N us_est (
       iinc_id:long,
       summary:text,
       ...
       ...
   );
   
   ALTER TABLE internet_inc
       CACHE OFF
       TIMETOLIVEINCACHE DEFAULT
       ADD SEARCHMETHOD internet_inc(
           I18N us_est
           CONSTRAINTS (
                ADD iinc_id (any) OPT ANY
                ADD summary (any) OPT ANY
                ...
                ...
           )
           OUTPUTLIST (iinc_id, specific_field1, specific_field2, summary, taxid, ttime
           )
           WRAPPER (jdbc internet_inc)
       );


Run the following (i.e. just one command)

.. code-block:: vql

   CREATE TABLE internet_inc I18N us_est (
       iinc_id:long,
       summary:text,
       ...
       ...
       )
       CACHE OFF
       TIMETOLIVEINCACHE DEFAULT
       ADD SEARCHMETHOD internet_inc(
           I18N us_est
           CONSTRAINTS (
                ADD iinc_id (any) OPT ANY
                ADD summary (any) OPT ANY
                ...
                ...
           )
           OUTPUTLIST (iinc_id, specific_field1, specific_field2, summary, taxid, ttime
           )
           WRAPPER (jdbc internet_inc)
       );


VIEWPROPERTIES Parameter of CONTEXT Clause in SELECT Statements
---------------------------------------------------------------

Starting with Denodo 5.5, the parameter ``VIEWPROPERTIES`` of the ``CONTEXT`` clause of ``SELECT`` statements is deprecated. 

The only reason to use it is to specify at runtime the value of the parameter "begin delimiter" of a DF data source. Instead, enable the option *Start delimiter from variable* of the data source.

Function TEXTCONSTANT
---------------------

The function :ref:`TEXTCONSTANT` is deprecated.

Aracne Data Sources
======================

Starting with Denodo 7.0, the Aracne module has been deprecated so the administration tool no longer supports creating Aracne data sources. However, if they have been imported from a previous version, they can be opened.

Google Search
=============

Starting with Denodo 7.0, the Google Search data sources have been deprecated.

Deprecated Functions
====================

The function ``CREATETYPEFROMXML`` is deprecated and may be removed in future versions of the Denodo Platform. Instead, create an XML data source with a route of type *from variable* and pass the XML document to this data source.

Denodo Stored Procedures
========================

The following stored procedures included out-of-the box in Virtual DataPort are deprecated: 

-  :ref:`CATALOG_ELEMENTS` replaced with :ref:`GET_ELEMENTS`.
-  :ref:`CATALOG_VIEWS` replaced with :ref:`GET_VIEWS`.
-  :ref:`CATALOG_PKS` replaced with :ref:`GET_PRIMARY_KEYS`.
-  :ref:`CATALOG_FKS` replaced with :ref:`GET_FOREIGN_KEYS`.
-  :ref:`SOURCE_CHANGES` replaced with :ref:`GET_SOURCE_CHANGES`.

These procedures have been replaced with new ones because the old ones only return information about the database you are currently connected to. The new ones ("GET\_...") can obtain the same information from any database.

Denodo Stored Procedures API: getNumOfAffectedRows Method
============================================================================

Starting with Denodo 6.0, the method ``getNumOfAffectedRows()`` of the stored procedures API has
been deprecated. Do not override this method in new stored procedures.

Although it is deprecated, the method will probably not be removed from
future versions of the API in order to maintain binary compatibility
with old stored procedures.

Denodo Custom Wrappers API: Deprecated Methods
============================================================================

Starting with Denodo 6.0, the following methods of API for Denodo custom wrappers have been deprecated:

-  com.denodo.vdb.engine.customwrapper.CustomWrapperConfiguration.isDelegateCompoundFieldProjections()
-  com.denodo.vdb.engine.customwrapper.CustomWrapperConfiguration.setDelegateCompoundFieldProjections(boolean)
-  com.denodo.vdb.engine.customwrapper.expression.CustomWrapperFieldExpression.getSubFields()
-  com.denodo.vdb.engine.customwrapper.expression.CustomWrapperFieldExpression.hasSubFields()

Denodo JDBC Driver
==================

Starting with Denodo 7.0, the parameter ``wanOptimizedCalls`` of the JDBC driver has been deprecated.

This parameter is deprecated because in Denodo 7.0 its default value is true and there is no reason to set it to false. In previous versions, the default value of this parameter is false. If true the driver reduces the number of remote calls it sends to the Server. In addition, when the application closes a result set, the driver only sends a cancel request to the Server if there are still pending results. If false, the driver always sends the cancel request.

Denodo Web Services
===================

The Denodo Web Services created with Virtual DataPort 4.7 or earlier versions are deprecated. These web services provide SOAP and
REST capabilities. They still work, its VQL statements can be loaded into Virtual DataPort |version| and can be edited from the administration tool. However, no new web services of this type can be created.

Starting with Denodo 5.0, SOAP web services and REST
web services are different elements. The reason is that SOAP web
services are operation-oriented and REST web services are
resource-oriented.

Widgets
=======

Starting with Denodo 7.0, the feature of publishing widgets is deprecated. This includes Web Parts for Microsoft SharePoint, Portlets v1.0 (JSR-168) and Portlets 2.0 (JSR-268).

Script Export
=============

Starting with Denodo 5.5, the scripts <DENODO_HOME>/bin/export and <DENODO_HOME>/bin/import have several parameters that are deprecated:

1. ``includeEnvSpecificElements``
#. ``includeNonEnvSpecificElements``

Instead, use the parameter ``includeProperties``.

Data Catalog: Roles "selfserviceadmin", "selfserviceexporter"
=============================================================

In the next major version of Denodo, the roles "selfserviceadmin" and "selfserviceexporter" will be removed. These roles exist 
in Denodo 7.0 to keep backward compatibility with Denodo 6.0 
but you should not grant them to users anymore. Instead, grant the new roles 
"data_catalog_admin" and "data_catalog_exporter", 
which are equivalent to "selfserviceadmin" and "selfserviceexporter" respectively.

In Denodo 7.0, the "Information Self-Service Tool" has been rebranded 
to "Data Catalog" and so have been these two roles.