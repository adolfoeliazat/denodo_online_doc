===============================
Listing Elements in the Catalog
===============================

The ``LIST`` command returns a list of elements names. E.g. a list of
the names of databases, lists of the names of JDBC data sources, etc.

From the VQL Shell of the administration tool, execute ``HELP LIST`` to
obtain the full syntax of these command.

The optional clause ``ON <database name>`` of some of the ``LIST``
commands allows obtaining the list of that type of elements on a
different database from the one you are connected to.

**Elements in the Server**

-  ``LIST DATABASES`` lists the existing databases.
-  ``LIST FUNCTIONS`` returns the name and syntax of all the functions
   available. This list includes the functions available out of the box
   and the custom functions developed by the user and uploaded to the
   Server.

   The output of this command may change in future versions of the Denodo Platform.

-  ``LIST FUNCTIONS CUSTOM`` returns the name and syntax of the custom
   functions developed by a user and uploaded to the Server.

   The output of this command may change in future versions of the Denodo Platform.

-  ``LIST I18NS`` lists the names of the available i18n. To obtain the
   configuration of a specific one, execute ``DESC VQL MAP I18N`` <name
   of the i18n>.
-  ``LIST JARS`` lists the jar extensions (see section :ref:`Defining JAR
   Extensions`). This commands lists the jar files users imported
   executing the command ``CREATE JAR`` or from the “Extension Management”
   dialog of the administration tool. It does not include the jars that
   you copied to the directory <DENODO\_HOME>/extensions/thirdparty/lib
   of the installation.
-  ``LIST MAPS`` lists maps of the type simple or i18n (see section :ref:`Defining a Map`).
-  ``LIST OPERATORS`` lists the operators that are applicable on a given
   data type: ``int``, ``double``, ``float``, ``text``, etc.
-  ``LIST POLICIES`` lists the custom policies.
   The section :doc:`/vdp/developer/custom_policies/custom_policies` of the Developer Guide explains what
   they are, how to develop them and how to import them into Virtual
   DataPort.
-  ``LIST RESOURCE_MANAGER { RULES | PLANS }`` lists information about
   the rules or plans defined in the Resource Manager.
-  ``LIST ROLES`` lists the roles (see section :doc:`/vdp/vql/creating_databases_users_roles_and_access_privileges/managing_users/managing_users`).
-  ``LIST USERS`` lists the user’s roles (see section :ref:`Managing User
   Roles`).

**Elements in the database you are connected to**

-  ``LIST ASSOCIATIONS`` lists the associations (see section :doc:`Associations </vdp/vql/restful_architecture/restful_architecture>`).
-  ``LIST DATASOURCES`` lists the data sources of the type specified
   (see section :ref:`Specifying Paths in Virtual DataPort`).
-  ``LIST LISTENERS JMS`` lists the listeners JMS (see section :ref:`Defining JMS Listeners`)
-  ``LIST PROCEDURES`` lists the Denodo stored procedures.
-  ``LIST TYPES`` shows all data types from the catalog or only those of
   a certain type (array or register).
-  ``LIST VIEWS`` lists the derived views of the database.

   If the clause ``ALL`` is present, it returns the list of base and
   derived views.

   If the clause ``BASE`` is present, it returns the list of base views.
-  ``LIST VIEWSTATSUMMARIES`` lists the views that have statistics that
   can be used by the Cost-based optimization (see section :ref:`Defining the
   Statistics of a View`).
-  ``LIST WEBSERVICES`` lists the published Web services (see section :doc:`/vdp/vql/publication_of_web_services/publication_of_web_services`).
-  ``LIST WIDGETS`` lists the published widgets (see section :ref:`Publication of Widgets`).
-  ``LIST WRAPPERS`` lists wrappers of the specified type (see section :ref:`Generating Wrappers and Data Sources`).

.. code-block:: bnf
   :name: Syntax of the LIST statement
   :caption: Syntax of the LIST statement

   LIST ASSOCIATIONS [ <viewName:identifier> ] [ ON <database:identifier> ]
   LIST DATABASES [ LDAP ]
   LIST DATASOURCES <datasource type> [ ON <database:identifier> ]
   LIST FUNCTIONS
   LIST FUNCTIONS CUSTOM
   LIST I18NS
   LIST JARS
   LIST LISTENERS JMS [ ON <database:identifier> ]
   LIST MAPS I18N
   LIST MAPS SIMPLE [ ON <database:identifier> ]
   LIST OPERATORS [ <type:identifier> ]
   LIST POLICIES
   LIST PROCEDURES
   LIST RESOURCE_MANAGER [ RULES | PLANS ]
   LIST ROLES
   LIST TYPES [ ARRAY | REGISTER ] [ ON <database:identifier> ]
   LIST USERS
   LIST VIEWS [ BASE | ALL ] [ ON <database:identifier> ]
   LIST VIEWSTATSUMMARIES [ ON <database:identifier> ]
   LIST WEBSERVICES [ ON <database:identifier> ]
   LIST WIDGETS [ ON <database:identifier> ]
   LIST WRAPPERS <wrapper type> [ ON <database:identifier> ]

   <datasource type> ::=
       ARN | CUSTOM | DF | ESSBASE | GS | JDBC | JSON | LDAP | ODBC | SALESFORCE | SAPERP
     | SAPBWBAPI | WS | XML

   <wrapper type> ::=
     <datasource type> | ITP

For example, to list the existing databases the following statement is
executed:

.. code-block:: vql

   LIST DATABASES;

To list the maps of the type i18n the following statement is used:

.. code-block:: vql

   LIST MAPS I18N;

To list the JDBC data sources of the support database:

.. code-block:: vql

   LIST DATASOURCES JDBC on support;
