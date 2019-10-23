===========================
Describing Catalog Elements
===========================


.. toctree::
   :hidden:

   exporting_metadata/exporting_metadata.rst
   importing_metadata/importing_metadata.rst

The ``DESC`` statement allows obtaining a description of the elements in
the Virtual DataPort server. Its syntax is given in :ref:`Syntax of the
statement DESC`.



.. code-block:: bnf
   :caption: Syntax of the statement DESC
   :name: Syntax of the statement DESC

   DESC QUERYPLAN { <query> | <CREATE REMOTE TABLE> }

   DESC SESSION

   DESC VQL ASSOCIATION <name:identifier with database>
       [ ( <DESC parameter> [, <DESC parameter> ]* ) ]

   DESC VQL DATABASE [ <name:identifier> ]
       [ ( <DESC DATABASE parameters> [, <DESC DATABASE parameters> ]* ) ]

   DESC VQL DATASOURCE <data source type> <name:identifier with database>
       [ ( <DESC parameter> [, <DESC parameter> ]* ) ]

   DESC VQL FOLDER <path:literal>
       [ ( <DESC FOLDER parameter> [, <DESC FOLDER parameter> ]* ) ]

   DESC VQL LIST <element DESC statement> [, <element DESC statement>]*
       [ ( <DESC LIST parameter> [, <DESC LIST parameter> ]* ) ]

   DESC VQL LISTENER JMS <name:identifier with database >
       [ ( <DESC parameter> [, <DESC parameter> ]* ) ]

   DESC VQL MAP { I18N | SIMPLE } <name:identifier>
       [ ( <DESC parameter> [, <DESC parameter> ]* ) ]

   DESC VQL PROCEDURE <name:identifier with database >
       [ ( <DESC parameter> [, <DESC parameter> ]* ) ]

   DESC VQL TYPE <name:identifier with database>
       [ ( <DESC parameter> [, <DESC parameter> ]* ) ]

   DESC VQL [ INTERFACE ] VIEW <name:identifier with database>
       [ ( <DESC parameter> [, <DESC parameter> ]* ) ]

   DESC VQL VIEWSTATSUMMARY <view name:identifier with database>

   DESC VQL { REST | SOAP } WEBSERVICE <name:identifier with database>
       [ FOR ( <operation or resource name:literal>
       [, <operation or resource name:literal>]* )]
       [ PRESERVE_OPERATIONS ]
       [ ( <DESC parameter> [, <DESC parameter> ]* ) ]

   DESC VQL WIDGET <name:identifier with database >
       [ ( <DESC parameter> [, <DESC parameter> ]* ) ]

   DESC VQL WRAPPER <wrapper type> <name:identifier with database>
       [ ( <DESC parameter> [, <DESC parameter> ]* ) ]

   DESC VQL WRAPPER ITP [ <name:identifier with database> ]
       [ <ITPilot DESC parameter> ]

   <data source type> ::=
       ARN | CUSTOM | DF | ESSBASE | GS | JDBC | JSON | LDAP | ODBC | OLAP
       | SALESFORCE | SAPBWBAPI | SAPERP | WS | XML }

   <element DESC statement> ::=
       ASSOCIATION <name:identifier with database>
       | DATABASE_CONFIG <name:identifier>
       | DATASOURCE <type:datasource type> <name:identifier with database>
       | FOLDER [<name:database>] <path:literal>
       | INTERFACE VIEW <name:identifier with database>
       | LISTENER JMS <name:identifier with database>
       | PROCEDURE <name:identifier with database>
       | ROLE <name:identifier>
       | USER <name:identifier>
       | VIEW <name:identifier with database> [ WITH STATS ]
       | WIDGET <name:identifier with database>
       | WRAPPER ITP <name:identifier with database>
       | { REST | SOAP } WEBSERVICE <name:identifier with database>
           [ FOR ( <operation or resource name:literal>
           [, <operation or resource name:literal>]* ) ]
           [ PRESERVE_OPERATIONS ]

   <wrapper type> ::=
       <data source type>
     | ITP

   <DESC FOLDER parameter> ::=
       'includeContents' = { 'yes' | 'no' }       // 'no' by default
     | <DESC parameter>

   <DESC DATABASE parameters> ::=
     | 'includeEnvSpecificElements' = { 'yes' | 'no' }    // 'no' by default
     | 'includeNonEnvSpecificElements' = { 'yes' | 'no' } // 'no' by default
     | 'includeVCSConfiguration' = { 'yes' | 'no' }       // 'no' by default
     | <DESC parameter>
     | <ITPilot DESC parameter>

   <DESC parameter> ::=
       'dropElements' = { 'yes' | 'no' }                    // 'yes' by default
     | 'replaceExistingElements' = { 'yes' | 'no' }         // 'no' by default
     | 'includeCreateDatabase' = { 'yes' | 'no' }           // 'no' by default
     | 'includeDependencies' = { 'yes' | 'no' }             // 'yes' by default
     | 'includeDeployments' = { 'yes' | 'no' }              // 'no' by default
     | 'includeJars' = { 'yes' | 'no' }                     // 'no' by default
     | 'includeStatistics' = { 'yes' | 'no' }               // 'no' by default
     | 'includeUserPrivileges' = { 'yes' | 'no' }           // 'no' by default
     | 'includeCreateWebService' = { 'yes' | 'no' }         // 'no' by default
     | 'includeProperties' = { 'yes' | 'no' }               // 'no' by default
     | 'exclude_jdbc_wrapper_properties' = { 'yes' | 'no' } // 'no' by default

   <DESC LIST parameter> ::=
       'includeServerProperties' = { 'yes' | 'no' }       // 'no' by default
     | 'includeWebContainerProperties' = { 'yes' | 'no' } // 'no' by default
     | <DESC parameter>
     | <DESC FOLDER parameter>

   <ITPilot DESC parameter> ::=
       'includeScanners' = { 'yes' | 'no' }               // 'no' by default
     | 'includeCustomComponents' = { 'yes' | 'no' }       // 'no' by default

..

   <query> ::= (see :ref:`Syntax of the SELECT statement`)

   <CREATE REMOTE TABLE> ::= (see :ref:`Syntax of the CREATE REMOTE TABLE statement`)


``DESC QUERYPLAN`` provides a look ahead at the execution plan that
will be used to execute a query. Although it will be possible to access
detailed trace information, including the execution plan used, after
executing the query (see section :ref:`TRACE Clause`), the ``QUERYPLAN``
provides this information without having to run the query.

``DESC SESSION`` returns the name of the database that the user is
connected to, along with her login name.

The statements of the following group return the VQL of the elements of
the catalog. If the element is a wrapper, a view, a Web service, a
widget or an association, the statement also returns the statements
needed to create the elements it depends on.

For example, ``DESC VQL VIEW V`` will return the statement required to
create the view ``V`` and the statements needed to create the data
types, wrappers, data sources, stored procedures and other views
required to define the view ``V``. If you are executing
``DESC VQL VIEW`` but you do not need the VQL statements of the elements
that this view depends on, add the parameter ``includeDependencies``.

E.g. ``DESC VQL VIEW V ('includeDependencies' = 'no')`` returns the
sentence ``CREATE VIEW V ...``, but not the sentences to creates the
views that ``V`` depends on and its data sources.

The statement ``DESC VQL FOLDER`` returns the VQL statements to recreate
the folder. If you add the option
``'includeContents' = 'yes'``, the output also includes the statements
that recreate the elements inside the folder, including other folders. If you also add the option
``'includeDependencies' = 'yes'``, the output also includes the statements
to recreate the dependencies of the elements inside the folder.

For example, if there is a folder ``F1`` with a view ``V`` in it, the
command ``DESC VQL FOLDER F1 ('includeDependencies' = 'yes', 'includeContents' = 'yes'``) returns the
statement to create the folder, the statements to create the view ``V``
and all the statements to create the elements that ``V`` depends on,
even if they are in different folders.

``DESC VQL LIST A, B`` will return the statements required to create the elements ``A`` and ``B``. For example, ``DESC VQL LIST VIEW V, DATASOURCE JDBC D`` returns the sentences to create the view ``V``, the data source ``D`` and their dependencies.

To the ``DESC VQL WEBSERVICE`` you can add the token ``FOR`` followed by
the name of one or more operations or resources. If you do this, the
result will be an :ref:`ALTER REST WEBSERVICE <Syntax of the ALTER REST WEBSERVICE statement>` statement or an :ref:`ALTER SOAP WEBSERVICE <Syntax of the ALTER SOAP WEBSERVICE statement>` statement that will allow you to
add these operations or resources to an existing web service. If you want to
generate a ``CREATE OR REPLACE`` statement that will keep the existent operations
or resources if the web service already exists, you can simply add
``PRESERVE_OPERATIONS`` token to the ``DESC VQL WEBSERVICE`` statement.

|

With the parameter ``includeProperties = yes``, the values of the 
parameters that depend on the environment
are variables instead of the actual values.
For example, with this property, in the ``CREATE DATASOURCE JDBC`` statements, the value of
``DATABASEURI`` will be something like
``${databases.common_database.datasources.jdbc.oracle_product.DATABASEURI}``
instead of the URI of the database.

When you add ``includeProperties = yes``, add ``'exclude_jdbc_wrapper_properties' = 'yes'`` if you want the parameters ``CATALOGNAME`` and ``SCHEMANAME`` of the ``CREATE WRAPPER JDBC`` statements to contain the actual value instead of a variable.

| 

When you export a database with the option ``'includeCreateDatabase'='yes'``, the output contains the statements CREATE DATABASE and ALTER DATABASE so the database is created with the same configuration, including the VCS settings of the database. If you do not want to export the VCS settings, add ``'includeVCSConfiguration'='no'``.

| 

By default, the statistics of views are only included in the result when you export the VQL of the entire server, nor when you export a database nor when you export a view. To include the statistics of the views in the result, add the option ``'includeStatistics'='yes'``.
