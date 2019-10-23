===============================
Changes in Virtual DataPort 6.0
===============================

Changes introduced in Virtual DataPort 6.0:

.. contents:: 
   :local:

Changes in VQL Syntax
============================================================================

Denodo 6.0 introduces changes to the VQL syntax to avoid ambiguities in
the process of parsing VQL statements.

Because of this, you may have to review the queries sent by third-party
clients to Virtual DataPort.

Starting with Denodo 6.0, consider the following:

-  It is mandatory to surround the values of the right side of a condition
   with a multi-valued operator. These operators are:

   -  ``in``
   -  ``not in``
   -  ``containsand``
   -  ``containsor``
   -  textual similarity (``~``)

      For example:

      .. code-block:: sql

         SELECT * 
         FROM V1 
         WHERE field_1 IN (1, 2)

    
      In earlier versions, the parentheses are optional.

-  It is mandatory to separate with the clause ``AND`` the values of the
   operator ``between``. E.g.
   ``field_1 BETWEEN value_1 AND value_2``.


   In earlier versions, you can execute
   ``field_1 between value1, value2``.


-  It is mandatory to surround literals with single quotes as indicated by
   the SQL standard. In earlier versions, you could use either single or
   double quotes for this, but not anymore.

   In Denodo 6.0, the double quotes are used to surround identifiers with
   special characters.


-  Surround “Unicode” identifiers with double quotes. In Denodo 5.5, you
   could only do it with French quotes. In Denodo 6.0 you can use French
   quotes as well, but they may be deprecated in the future.

   For example, the following two statements are equivalent:
   
   .. code-block:: sql

      CREATE VIEW `view with spaces in the name` ...

..

   .. code-block:: sql

      CREATE VIEW "view with spaces in the name" ...


-  To provide the value of an interpolation variable, do it with the “=”
   operator. E.g. the following query is no longer valid.
   
   .. code-block:: sql

      SELECT * 
      FROM view1 
      WHERE input_status > 'value'
      
      
   In previous versions, you could use any operator and the result was the 
   same as using ``=``. However, it was semantically incorrect.

-  In literals, you can no longer use the backslash (``\``) to escape
   the simple quote (``'``). To escape a single quote, you have prefix
   it with another single quote.
   
   For example, in Denodo 5.5, the following query returns he\\'llo and in Denodo 6.0, returns he\\.
   
   .. code-block:: vql 
   
      SELECT 'he\'llo' AS f1

Changes in VQL Statements
============================================================================



CREATE DATASOURCE JDBC
-----------------------------------------------------------------------------------------------------

In Denodo 6.0, the following properties of the “Source Configuration” of
JDBC and ODBC data sources have been removed:

-  “ORDER BY collation” (``ORDERBYCOLLATION`` parameter of the
   ``SOURCECONFIGURATION`` clause in the ``CREATE DATASOURCE`` command).
-  “Delegate ORDER BY Collation” (``DELEGATEORDERBYCOLLATION`` parameter
   of the ``SOURCECONFIGURATION`` clause in the ``CREATE DATASOURCE``
   command).

When you import a data source that has these properties set, their value
is ignored.

Removing these properties does not cause a loss of functionality. They
were removed because having these properties configured in a data source
could cause merge joins to produce invalid results. The reason is that
the database could return the results sorted with a non-binary
collation, while the merge join expects the result sets sorted with a
binary collation. Instead, the properties “Supports binary ORDER BY
collation”, “Delegate ORDER BY collation modifier” and “Delegate binary
ORDER BY collation” control correctly how Virtual DataPort pushes down
the ORDER BY clause to databases.

The section :ref:`ORDER BY Properties of the Source Configuration` of the
Virtual DataPort Administration Guide explains the meaning of these new
properties. However, it is very rare that you need to modify their
default value.


LIST FUNCTIONS CUSTOM
-----------------------------------------------------------------------------------------------------

Starting with Denodo 6.0, ``LIST FUNCTIONS CUSTOM`` only returns the
list of functions contained in jar files imported by the users. In
previous versions, it listed more functions.


Dependencies to Develop Custom Elements (Functions, Wrappers…)
============================================================================

Some of the jars required to develop custom elements (custom functions,
custom wrappers, stored procedures and custom input filters) now have
different name.

However, the names of the Java packages and classes has *not* changed.
Therefore, the custom elements developed for previous version will work
correctly in Denodo 6.0.

The new names are documented in the Virtual DataPort Developer Guide.

Importing Roles from an LDAP Server
============================================================================

In Denodo 6.0, in the “Import roles from LDAP” dialog of “Roles
Management”, the value you have to enter in “Role base” is different
from the previous versions.

To import the roles, Virtual DataPort queries the LDAP server and this
LDAP query is performed under a specific level.

In Denodo 5.5 and earlier versions, this level is the concatenation of
two values:

#. The base search of the URI of the LDAP data source.
#. And, the value of the “Role base” field of the settings of the
   database.

In Denodo 6.0, this level is just the value of the “Role base” field.

For example, if the URL of the LDAP data source used by the database is "\ldap://acme.denodo.com:389/DC=Denodo" and “Role base” on the database is ``DC=contoso,DC=com``:

-  In Denodo 5.5 and earlier, the search is performed under the level
   ``DC=denodo,DC=contoso,DC=com``.
-  In Denodo 6.0 and 7.0, the search is performed under the level
   ``DC=contoso,DC=com``.


JDBC Data Sources
============================================================================



Access to Teradata
-----------------------------------------------------------------------------------------------------

Starting with Denodo 6.0, the property “Supports binary ORDER BY
collation” of the JDBC data sources is “yes” by default, when the
database adapter is Teradata. In previous versions, the default value
was “no”.

Setting this property to “yes” allows delegating merge joins to
Teradata, which is the fastest join method. However, is very important
that in Teradata, the default collation is set to binary. Otherwise, the
results of the merge joins executed by Virtual DataPort (not delegated
to the database) may be incorrect.

If you cannot set the default collation to binary, set this property to
“no” in all the data sources that connect to a Teradata database.


Delegate SQL Sentence as Subquery
-----------------------------------------------------------------------------------------------------

Starting with Denodo 6.0, the base views created from query using the
administration tool have the option “Delegate SQL Sentence as subquery”
set to “yes”.

When this option is enabled, at runtime, Virtual DataPort delegates the
SQL query of the base view as a subquery in the ``FROM`` clause. This
increases the number of operations that are delegated to the database.
However, some queries may fail if the syntax of the delegated query is
not accepted by the database.

The section :doc:`View Configuration Properties </vdp/administration/creating_derived_views/advanced_configuration_of_views/view_configuration_properties>` of the Virtual DataPort
Administration Guide explains in more detail how this property works and
its limitations.




JDBC Driver
============================================================================



Location of the JDBC Driver
-----------------------------------------------------------------------------------------------------

Starting with Denodo 6.0, the Denodo JDBC driver is located at the
directory :file:`{<DENODO_HOME>}/tools/client-drivers/jdbc`.


Metadata Returned by the JDBC driver
-----------------------------------------------------------------------------------------------------

Starting with Denodo 6.0, the method
``DatabaseMetadata.getMaxColumnNameLength()`` of the Denodo JDBC driver
returns the maximum number of characters Virtual DataPort allows for a
column name.

In earlier versions, this method always returns ``0``, which means that
the limit is unknown. Some JDBC clients such as “JasperReports” do not
work well with this value.


JDBC Driver Compiled with Java 7
-----------------------------------------------------------------------------------------------------

Starting with Denodo 6.0, the Denodo JDBC driver is compiled with Java
7.

The applications that use this driver and that run with earlier versions
of Java will have to update their version of Java. Otherwise, the
applications will not be able to load this driver.

.. note:: The JDBC driver of earlier versions of Denodo cannot connect to Denodo 6.0


JMX
============================================================================

Starting with Denodo 6.0, the operation ``GetActiveRequestList()`` of
the JMX MBean ``VDBServerManagementInfo`` returns the complete list of
requests sent to the server. In earlier versions, this operation only
returns information about ``SELECT``, ``CALL``, ``INSERT``, ``UPDATE``
and ``DELETE`` requests.

With this change, you also can monitor Data Definition Language and
``DESC`` statements.


Memory Usage Options
============================================================================

Starting with Denodo 6.0, by default, Virtual DataPort limits the total
amount of memory used by a query.

This feature existed in earlier versions of Denodo, but it was disabled
by default.

The section :doc:`Limit the Maximum Amount of Memory of a Query <../../../../vdp/administration/memory_management/limit_the_maximum_amount_of_memory_of_a_query/limit_the_maximum_amount_of_memory_of_a_query>` 
describes of the Virtual DataPort Administration Guide this option.


ODBC Driver
============================================================================



New Denodo ODBC Driver
-----------------------------------------------------------------------------------------------------

Starting with version 6.0, the Denodo Platform provides its own ODBC
driver, which is based on the PostgreSQL ODBC driver. Whenever is
possible, stop using the PostgreSQL one and begin using the Denodo ODBC
driver. It is located at :file:`{<DENODO_HOME>}/tools/client-drivers/odbc`.

The steps to configure it on Linux are slightly different. They are
documented in the Virtual DataPort Developer Guide.

The Denodo ODBC driver performs better than the PostgreSQL ODBC driver
because it has several changes tailored specifically to the Denodo
Platform.


Date Values with Millisecond Precision
-----------------------------------------------------------------------------------------------------

Starting with Denodo 6.0, the ODBC interface returns date fields with
millisecond precision. In previous versions, this interface returns the
dates with second precision.

This change only applies to the ODBC interface. The JDBC interface
returns dates with millisecond precision in this version *and* the
previous ones.




REST Web Services No Longer Return the HTTP Code 204 by Default
============================================================================

Starting with Denodo 6.0, the REST Web services published by Denodo no
longer return the HTTP code 204 (No content) by default.

In previous versions, the RESTful web service and the published REST web
services return the HTTP code 204 in the following scenarios:

#. The request has the parameter ``$filter`` and does not return any
   rows.
#. Or the request queries a view that does not return any rows.

This applies only to the JSON and XML representations; not to the HTML
one.

If you want the REST Web services to go back to the behavior of previous
versions (returning 204), execute the following command from the VQL
Shell of the Administration Tool:

.. code-block:: sql

   SET 'com.denodo.wsgenerator.restws.noResultsReturnHTTPCode' = '204';

After executing this statement, all the REST Web services that you
deploy or redeploy will:

-  Return the HTTP code 204 instead of 200 when the result does not
   return any rows.
-  They will return an empty response, instead of an empty XML or JSON
   document (depending on the representation requested).

You do not need to restart Virtual DataPort to apply this change, but
you have to redeploy the REST web services.

REST services continue returning the HTTP code 204 when updating (HTTP
PUT request) or deleting rows (HTTP DELETE request).

The behavior of the RESTful Web service
(http://localhost:9090/denodo-restfulws) has changed in the same way. To
configure it to return the HTTP code 204 instead of 200 in the scenarios
listed above, follow these steps:

#. Edit the file ``other_settings.xml`` of the directory
   :file:`{<DENODO_HOME>}/resources/apache-tomcat/webapps/denodo-restfulws/WEB-INF/`
#. Set the value of the property ``noResultsReturnHTTPCode`` to ``200``.
#. Restart the Virtual DataPort server to apply the changes.


Stored Procedures
============================================================================

The return parameter ``old type`` of the ``SOURCE_CHANGES`` built-in
stored procedure has been renamed to ``old_type``.


User Privileges
============================================================================

Starting with Denodo 6.0, users can query views that belong to databases
other than the one they are connected. Therefore, even if a user is not
allowed to connect to a database, she can query the views of that
database over which she has read access, when she is connected to
another database. However, as it cannot connect to that database, it
will not be able to create, edit or drop its views.

In previous versions, the users can only query views that belong to
databases over which they have the privilege “Connect”.

Version Control System Integration (VCS)
============================================================================

The process of importing checking out a database from a Version Control
System (VCS) has changed in this version.

In Denodo 5.5, when a statement obtained from the VCS fails, the process
is stopped and nothing is imported.

In Denodo 6.0, when a statement obtained from the VCS fails, the Server
continues executing the next one. This is necessary so is possible to
partially update a database that has elements that depend on elements
from another database that does not exist yet.

Windows Services
============================================================================

Starting with Denodo 6.0, when you change the user that starts the
Denodo Windows services, you do not have to modify the value of the
parameter “jna\_tmpdir”. In Denodo 6.0, this property points to a folder
in the Denodo installation.

In previous versions, this parameter points to the TEMP directory of the
user that installed the Denodo Platform. Therefore, when you change the
user that runs the Denodo Windows services, in previous versions you
have to change the value of the “jna\_tmpdir”. But you do not have to do
this in Denodo 6.0.

Features Deprecated in Denodo 6.0
============================================================================

This section lists the features that are marked as deprecated in Denodo
6.0. Deprecated features are not actively maintained and may be removed
in future versions of the Denodo Platform. We recommend not using them
anymore.



VQL Syntax
-----------------------------------------------------------------------------------------------------

The syntax ``ALTER TABLE`` <base view name> ``ADD SEARCHMETHOD`` is
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

Internal Pool of Connections of the JDBC Driver
-----------------------------------------------------------------------------------------------------

The Denodo JDBC driver is capable of creating its own pool of
connections to Virtual DataPort. This feature is deprecated.

This pool of connections is configured with the parameters
``poolEnabled``, ``initSize``, ``maxActive`` and ``maxIdle``, which will
no longer be valid when this feature is removed.

Published Web Services: “with LDAP” Authentication
-----------------------------------------------------------------------------------------------------

These following authentication methods of published web services have
been deprecated:

-  For REST web services, “HTTP Basic with LDAP” and “WSS Basic with
   LDAP”.
-  For SOAP web services, “HTTP Basic with LDAP”.

SQLFRAGMENT clause
-----------------------------------------------------------------------------------------------------

The ``SQLFRAGMENT`` clause of the ``CREATE WRAPPER JDBC`` statement has
been deprecated.

Stored Procedures API: getNumOfAffectedRows Method
-----------------------------------------------------------------------------------------------------

The method ``getNumOfAffectedRows()`` of the stored procedures API has
been deprecated. Do not override this method in new stored procedures.

Although it is deprecated, the method will probably not be removed from
future versions of the API in order to maintain binary compatibility
with old stored procedures.

Custom Wrappers API: deprecated methods
--------------------------------------------------

The following methods of the custom wrappers API have been deprecated:

-  com.denodo.vdb.engine.customwrapper.CustomWrapperConfiguration.isDelegateCompoundFieldProjections()
-  com.denodo.vdb.engine.customwrapper.CustomWrapperConfiguration.setDelegateCompoundFieldProjections(boolean)
-  com.denodo.vdb.engine.customwrapper.expression.CustomWrapperFieldExpression.getSubFields()
-  com.denodo.vdb.engine.customwrapper.expression.CustomWrapperFieldExpression.hasSubFields()


XMLA Connectors for SAP
-----------------------------------------------------------------------------------------------------

The XMLA adapters for SAP (i.e. “SAP BW 3.x (XMLA)” and “SAP BI 7.x
(XMLA)”) have been deprecated. We suggest you begin using the adapter
“SAP BI 7.x (BAPI)”.

JDBC Data Source Dialog: Choose Automatically
-----------------------------------------------------------------------------------------------------

The check box *Choose automatically* will be removed in future major versions of the Denodo Platform.


Features Removed in Denodo 6.0
============================================================================

This section lists the features that are no longer available in Denodo
6.0.

Enumerated and Money Types
-----------------------------------------------------------------------------------------------------

The types ``enumerated`` and ``money`` have been removed.

If you used the type ``money``, use ``decimal`` instead.

Generic SOAP Web Service Interface
-----------------------------------------------------------------------------------------------------

The generic SOAP Web Service interface has been removed. In earlier
versions, this interface is deployed at
\http://localhost:9090/denodo-vdp-wsclient/services.

This change does *not* affect SOAP Web services that publish one or more
views. They are still fully supported.

As an alternative to clients that cannot use the JDBC driver, Denodo
provides the RESTful Web service
(http://localhost:9090/denodo-restfulws) and the ODBC interface.

Server Administration Mode
-----------------------------------------------------------------------------------------------------

In the administration tool of Virtual DataPort, the “server
administration mode” has been removed. Starting with Denodo 6.0,
whenever you connect to a Virtual DataPort server, the URL always has to
contain the name of a database.

