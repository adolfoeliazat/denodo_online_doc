=================================================
Creating and Modifying Virtual DataPort Databases
=================================================

The commands to manage a Virtual DataPort database are:

-  ``CREATE DATABASE``: creates a new database. See the syntax below. Only administrators can run this.
-  ``ALTER DATABASE``: modifies an existing database. See the syntax below. Only administrators can run this.
-  ``DESC VQL DATABASE``: obtains the VQL statements of the elements of the database.
-  ``LIST DATABASES``: returns the names of all the databases. For non-administrators, this command only returns the databases over which the user has the CONNECT privilege.
-  ``DROP DATABASE``: deletes a database. When a database is deleted all its components are deleted as well (data sources, views, base views...). Only administrators can run this. 

.. code-block:: bnf
   :caption: Syntax of the CREATE DATABASE statement
   :name: Syntax of the CREATE DATABASE statement

   CREATE [ OR REPLACE ] DATABASE <name:identifier> [ <description:literal> ] 
       [ VCS
         {   OFF
           | ON
             {   (
                   [ ENVIRONMENT = <identifier> ] 
                   [ REMOTEDB = <identifier> ]
                   [ PROPERTIES ( <literal> = <literal> [, <literal> = <literal> ]* ) ]
                 )
               | ( 
                   URL = <literal>
                   SYSTEM = <literal>
                   REMOTEDB = <identifier>
                   [ USER = <literal> ]
                   [ PASSWORD = <literal> ] [ ENCRYPTED ]
                   [ ENVIRONMENT = <identifier> ]
                   [ PROPERTIES ( <literal> = <literal> [, <literal> = <literal> ]* ) ]
                 )
             }
         }
       ]
       [ CHARSET { UNICODE | RESTRICTED | DEFAULT } ]
       [ AUTHENTICATION 
         {   LOCAL 
           | LDAP <database name:identifier>.<ldap data source name:identifier>
             USERBASE = <literal> [ , <literal> ]*
             USERATTRIBUTENAME = <literal>
             USERSEARCH = <literal> 
             ROLEBASE = <literal> [ , <literal> ]*
             ROLEATTRIBUTENAME = <literal> 
             ROLESEARCH = <literal> 
             [ ROLESSEARCHAUTHENTICATION ] 
             [ WITH_ALLUSERS_ROLE ]
          } 
       ]
       [ ODBC AUTHENTICATION { NORMAL | KERBEROS } ] // NORMAL by default
       [ CHECK_VIEW_RESTRICTIONS { ALWAYS | DIRECT_QUERIES_ONLY | DEFAULT } ]
       [ <grant> ]*

..

   <grant> ::= (see section :ref:`Granting Privileges by Databases`)

Description of the main parameters of ``CREATE DATABASE``:

-  The Unicode support (``CHARSET`` parameter).
   
   If ``UNICODE``, the identifiers of elements can have any character.
   
   If ``RESTRICTED``, the identifiers of elements can only have some characters.
   
   If ``DEFAULT``, the database will use the default configuration of the
   Server.
   
   See more about this in the section :ref:`Identifiers Charset` of the Administration Guide.
   
   The value of this parameter only determines which characters can users use when creating/editing views from the Administration Tool. It does not affect the behavior of the Server.
   
-  Add the clause ``ODBC AUTHENTICATION KERBEROS`` to enable Kerberos
   authentication for ODBC/ADO.net clients. The section :doc:`/vdp/developer/access_through_odbc/access_through_odbc`
   of the Developer Guide explains how to configure a DSN so ODBC
   clients use Kerberos authentication.

-  ``CHECK_VIEW_RESTRICTIONS``: the section :ref:`Enforcing Column Privileges, Row Restrictions and Custom Policies` of the Administration Guide explains the implications of changing this parameter.


*Version Control System*

The Version Control Systems integration (VCS) support can be configured
for a database (for more details see the section :doc:`/vdp/administration/version_control_systems_integration/version_control_systems_integration` of the Administration Guide). VCS can be
deactivated, activated with the default configuration (the global configuration of the Server) and activated with specific configuration:


-  Deactivated: ``VCS OFF``.

-  Activated with default configuration: ``VCS ON``. There are several
   parameters that can be configured even if the database uses the default
   VCS configuration:


   -  ``ENVIRONMENT``. A different environment than the Server’s default one
      can be specified.

   -  ``REMOTEDB``. The database can be synchronized against a remote database
      named differently.

   -  Activated with specific configuration: also ``VCS ON``, but with a
      different set of parameters. In this case there are more parameters that
      can be configured, in addition to ``ENVIRONMENT`` and ``REMOTEDB``:

     -  ``URL``. The URL of the version control repository that is going to
        be used.
     -  ``SYSTEM``. The version control system to be used (currently, “svn”
        is supported).
     -  ``USER``. An optional user name to access the repository.
     -  ``PASSWORD`` (optionally ``ENCRYPTED``). The password corresponding
        to the specified user name.


*Authentication Method*

A Virtual DataPort database can have one of these two types of access
control:

1. Local access control (``AUTHENTICATION LOCAL``). That is, the users that
   access Virtual DataPort have to be created in Virtual DataPort (these
   users are the ones created in the section :doc:`../managing_users/managing_users`).

2. LDAP access control (``AUTHENTICATION LDAP``). That means that the
   authentication of users is delegated to an LDAP server. The benefit of
   this option is that you can rely on an LDAP server to authenticate
   users without having to explicitly creating them in Virtual DataPort.

   The Server also obtains the names of the roles that the users belong
   to, from the LDAP server. The Server uses these roles to check which
   actions the users can execute.

   .. note:: If the authentication method of this database is LDAP, read
      the section :doc:`/vdp/administration/databases_users_and_access_rights_in_virtual_dataport/administration_of_databases_users_roles_and_their_access_rights/creating_databases` of the Administration
      Guide. It explains several steps you have to do before creating the
      database. It also contains a more detailed explanation of the following
      parameters.

   If the parameter ``AUTHENTICATION`` is not present, the new database
   will have “Local access control”.

   List of parameters you have to provide when configuring a database with
   LDAP authentication:

   -  ``database name``: Virtual DataPort database where there is an LDAP
      data source that connects to the LDAP server that will be used to
      authenticate users.
   -  ``ldap data source name``: the name of the LDAP data source.
   -  ``USERBASE``: node of the LDAP server that is used as scope to search
      nodes that represent users.
      If this parameter has multiple values, the Server searches the user’s
      node in the first ``USERBASE``. If the Server does not find the node
      that represents the user, it searches it in the second ``USERBASE``
      scope. If it also fails, in the third, etc. If the Server does not
      find the node that represents the user, it denies access to the user.
   -  ``USERATTRIBUTENAME``: name of the attribute that contains the user
      name of users, in the nodes that represent users.
   -  ``USERSEARCH``: pattern used to generate the LDAP queries that will
      be executed to obtain the nodes that represent the users that try to
      connect to the Server.
   -  ``ROLEBASE``: node of the LDAP server that is used as the scope to
      search the nodes that represent roles that users of this database can
      have.
      If this parameter has multiple values, the Server executes the LDAP
      query formed with the pattern ``ROLESEARCH`` from all the nodes
      indicated in this parameter.
   -  ``ROLEATTRIBUTENAME``: name of the attribute that contains the name
      of the role, in the nodes that represent roles.
   -  ``ROLESEARCH``: pattern used to generate the LDAP queries that will
      be executed to obtain the nodes that represent the roles of a user.
      This pattern has to contain the token ``@{USERDN}`` or ``@{USERLOGIN}`` (it cannot contain both):
   
      -  ``@{USERDN}`` will be replaced with the Distinguished Name of the user that tries to connect to this database. For example, "CN=john,CN=Users,DC=acme,DC=loc".

      -  ``@{USERLOGIN}`` will be replaced with the login name of the user that tries to connect to this database. For example, "john".
      
   -  ``ROLESSEARCHAUTHENTICATION``: when a user tries to connect to a
      database with LDAP authentication, the Server validates the password
      she provided and then, it executes a LDAP query to obtain the roles
      of the user.
      If the ``ROLESSEARCHAUTHENTICATION`` clause is present, the Server
      executes this LDAP query using the credentials of the LDAP data
      source associated with the database. Otherwise, this query is
      executed using the credentials of the user that is trying to connect
      to this database.
   -  ``WITH_ALLUSERS_ROLE``: if present, the Server will grant the
      privileges of the role “allusers” to all the users that log in
      successfully into this database, even if this role is not assigned to
      the user in the LDAP server.
   
      Adding this clause to the statement is equivalent to selecting the
      check box “Assign "allusers" role for every connected user” in the
      “Create New Database” dialog.
   
      The section :ref:`Creating a Database with LDAP Authentication` of the
      Administration Guide explains this option.

|


.. code-block:: bnf
   :caption: Syntax of the ALTER DATABASE statement
   :name: Syntax of the ALTER DATABASE statement

   ALTER DATABASE <name:identifier> [ <description:literal> ]
       [ CHARSET { UNICODE | RESTRICTED | DEFAULT } ]
       [ COST OPTIMIZATION { ON | OFF | DEFAULT } ]
       [ QUERY SIMPLIFICATION { ON | OFF | DEFAULT } ]
       [ VCS
         {   OFF
           | ON
             {   (
                   [ ENVIRONMENT = <identifier> ] 
                   [ REMOTEDB = <identifier> ]
                   [ PROPERTIES ( <literal> = <literal> [, <literal> = <literal> ]* ) ]
                 )
               | ( 
                   URL = <literal>
                   SYSTEM = <literal>
                   REMOTEDB = <identifier>
                   [ USER = <literal> ]
                   [ PASSWORD = <literal> ] [ ENCRYPTED ]
                   [ ENVIRONMENT = <identifier> ]
                   [ PROPERTIES ( <literal> = <literal> [, <literal> = <literal> ]* ) ]
                 )
             }
         }
       ]
       [ AUTHENTICATION { 
             LOCAL [ <grant> ]*
           | LDAP <database_name:identifier>.<ldap_data_source_name:identifier>
             USERBASE = <literal> [ , <literal> ]*
             USERATTRIBUTENAME = <literal>
             USERSEARCH = <literal> 
             ROLEBASE = <literal> [ , <literal> ]*
             ROLEATTRIBUTENAME = <literal> 
             ROLESEARCH = <literal>
             [ ROLESSEARCHAUTHENTICATION ]
             [ WITH_ALLUSERS_ROLE ]
          } 
       ] 
       [ ODBC AUTHENTICATION { NORMAL | KERBEROS } ] // NORMAL by default
       [ CACHE
           {   DEFAULT 
             | [ ON | OFF ] (
                 [ MAINTENANCE { OFF | ON } ]
                 [ MAINTAINERPERIOD <seconds:integer> ]
                 [ TIMETOLIVE { <seconds:integer> | DEFAULT | NOEXPIRE } ]
                 [ DATASOURCE { DEFAULT | CUSTOM | <data_source_name:identifier> DATABASE <database_name:identifier> } ]
               )
           }
       ] 
       [ MEMORYCONFIG
           {   DEFAULT
             | ( SWAP [ ON | OFF ] ( 
                   [ SWAPSIZE <megabytes:integer> ]
                   [ SWAPBLOCKSIZE <megabytes:integer> ]
                 )
                 [ MAXRESULTSIZE <megabytes:integer> ]
                 [ MAXQUERYSIZE <megabytes:integer> ]
               )
           }
       ]
       [ CHECK_VIEW_RESTRICTIONS { ALWAYS | DIRECT_QUERIES_ONLY | DEFAULT } ]
       [ <grant> ]*
   
..
   
   <grant> ::= (see :ref:`Syntax of the GRANT/REVOKE clauses of CREATE DATABASE and ALTER DATABASE`)


With this statement, you can modify among others, the following settings
of a database:

-  The description of the database.
-  The authentication mode to LDAP or normal.
-  If the authentication mode *is not LDAP*, change the access
   privileges of users over the database (see section :ref:`Granting
   Privileges by Databases`).
-  The default preferences of the cache configuration of the database (see
   section :ref:`Using the Cache`), the swapping to disk of intermediate
   results of queries (see section :ref:`Configuring Swapping Policies`) and
   the locale of the database.
-  The ``MEMORYCONFIG`` clause corresponds with the settings of the dialog
   *Memory Usage* of the *Database management* wizard (menu *Administration*
   > *Database management* of the Administration Tool).
-  ...
   
Description of the main parameters of ``ALTER DATABASE`` (others are already described above, in ``CREATE DATABASE``):

-  ``COST OPTIMIZATION``: this clause enables/disables the cost-based
   optimization feature for this database. You can read more about this
   feature in the section :ref:`Cost-Based Optimization` of the Administration
   Guide.

   -  If ``ON``, the feature is enabled for this database.
   -  If ``OFF``, the feature is disabled for this database.
   -  If ``DEFAULT``, the feature is enabled in this database if it is
      enabled for the entire Virtual DataPort server. You can see if it is
      enabled in the dialog “Queries optimization” of the “Administration”
      menu. Look for the option “Automatic cost-based optimization”.

-  ``QUERY SIMPLIFICATION``: this clause enables/disables the automatic
   simplification of queries for this database. You can read more about this
   feature in the section :doc:`/vdp/administration/optimizing_queries/automatic_simplification_of_queries/automatic_simplification_of_queries` of the
   Administration Guide.

   -  If ``ON``, the feature is enabled for this database.
   -  If ``OFF``, the feature is disabled for this database.
   -  If ``DEFAULT``, the feature is enabled in this database if it is
      enabled for the entire Virtual DataPort server. You can see if it is
      enabled in the dialog “Queries optimization” of the “Administration”
      menu. Look for the option “Automatic simplification of queries”.
