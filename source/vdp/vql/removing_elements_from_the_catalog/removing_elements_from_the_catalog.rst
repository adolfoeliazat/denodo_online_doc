==================================
Removing Elements from the Catalog
==================================

The statement ``DROP`` removes elements from the
Virtual DataPort server.

.. code-block:: bnf
   :caption: Syntax of the DROP statement
   :name: Syntax of the DROP statement

   DROP DATABASE [ IF EXISTS ] <name:identifier> [ CASCADE ]
   
   DROP DATASOURCE <data source type> [ IF EXISTS ] <name:identifier> [ CASCADE ]

   DROP PROCEDURE [ IF EXISTS ] <name:identifier> [ CASCADE ]
   
   DROP WRAPPER <wrapper type> [ IF EXISTS ] <name:identifier> [ CASCADE ]

   DROP { INTERFACE VIEW | VIEW | TABLE } [ IF EXISTS ] 
       <name:identifier> [ CASCADE ]
   
   DROP ASSOCIATION [ IF EXISTS ] <name:identifier>
   
   DROP CUSTOMCOMPONENT <name:identifier>
   
   DROP ENVIRONMENT <name:identifier>

   DROP FOLDER [ IF EXISTS ] <path of the folder:literal> [ CASCADE ]
   
   DROP JAR [ IF EXISTS ] <name:literal> [ CASCADE ]
   
   DROP LISTENER JMS [ IF EXISTS ] <name:literal>
   
   DROP MAP I18N [ IF EXISTS ] <name:identifier> [ CASCADE ]
   
   DROP MAP SIMPLE [ IF EXISTS ] <name:identifier> [ CASCADE ]

   DROP RESOURCE_MANAGER RULE [ IF EXISTS ] <name:identifier>

   DROP RESOURCE_MANAGER PLAN [ IF EXISTS ] <name:identifier> [ CASCADE ]
   
   DROP SCANNER <name:literal>
   
   DROP TYPE [ IF EXISTS ] <name:identifier> [ CASCADE ]
   
   DROP REST WEBSERVICE [ IF EXISTS ] <name:identifier>
   
   DROP SOAP WEBSERVICE [ IF EXISTS ] <name:identifier>
   
   DROP { USER | ROLE } [ IF EXISTS ] <name:identifier>

   DROP VIEWSTATSUMMARY [ IF EXISTS ] <view name:identifier>
   
   DROP WEBSERVICE [ IF EXISTS ] <name:identifier>
   
   DROP WIDGET [ IF EXISTS ] <name:identifier>
   
   <data source type> ::=
       ARN | CUSTOM | DF | ESSBASE | GS | JDBC | JSON | LDAP | ODBC | OLAP
     | SALESFORCE | SAPBWBAPI | SAPERP | WS | XML
       
   <wrapper type> ::= <data source type> | ITP

The available options for the ``DROP`` statement are:

-  Remove database, a user or a users’ role from the Server
-  Remove a data source or a wrapper indicating its type and name
-  Remove a stored procedure
-  Remove a view (base or derived)
-  Remove an association
-  Remove an ITPilot custom component
-  Remove a folder
-  Remove a jar extension
-  Remove a JMS listener
-  Remove a specific data dictionary map indicating its type (*i18n* or
   *simple)* and its name
-  Remove an ITPilot scanner used by a WWW wrapper
-  Remove a data type
-  Remove a published Web service
-  Remove a published Widget

The ``IF EXISTS`` modifier can be included in all of the above cases. In
this case, the DROP sentence will only be run in the event of the
specified element existing.

The statements for deleting views, types, wrappers and data sources
allow the optional modifier ``CASCADE``. If this modifier is not
indicated, when an attempt is made to delete one of these elements, an
error will occur if another catalog element depends on it (for example,
if a data source is deleted and a wrapper that uses it exists). In this
case, the element will not be deleted. If the modifier ``CASCADE`` is
specified, then the indicated element will be deleted and all the
elements that depended on it will be also deleted. If the user executing
the delete operation has not enough privileges over all the involved
elements, the operation will fail.

Some examples of use of the ``DROP`` statements are shown below. To
eliminate the view ``shopview`` the following statement is executed:

.. code-block:: vql
   
   DROP VIEW shopview;

To remove the WWW (ITPilot) wrapper ``shopview`` simply execute:

.. code-block:: vql
   
   DROP WRAPPER ITP shopview;

And to remove the map type *i18n* ``es_euro`` the following statement is
used:

.. code-block:: vql
   
   DROP MAP I18N es_euro;

   DROP MAP I18N es_euro;
