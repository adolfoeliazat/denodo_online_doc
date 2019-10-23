==================================
Modifying the Privileges of a User
==================================

You can grant privileges to users that are not administrators, over the
databases or their views and stored procedures. This task can only be
executed by administrator users or users with the role "assignprivileges".

Modifying the privileges of users can be executed on a database level
for a list of users or individually, by user.

Granting Privileges by Databases
=================================================================================

The ``CREATE DATABASE`` (:ref:`Syntax of the CREATE DATABASE statement`)
and ``ALTER DATABASE`` (:ref:`Syntax of the ALTER DATABASE statement`)
statements can include the ``GRANT`` and ``REVOKE`` clauses to grant or
revoke access rights to a user/role over a database.

The privileges that you can grant to a user over a database are:
``CONNECT``, ``CREATE``, ``CREATE_DATA_SOURCE``, ``CREATE_VIEW``, ``CREATE_DATA_SERVICE``, ``CREATE_FOLDER``, ``EXECUTE``, ``METADATA``, ``WRITE``, ``FILE`` and ``ADMIN``. These
privileges are explained in detail in the section :ref:`Types of Access Rights` of the Administration Guide.

Granting the privilege ``ALL PRIVILEGES`` is equivalent to granting
``CONNECT``, ``CREATE``, ``CREATE_DATA_SOURCE``, ``CREATE_VIEW``, ``CREATE_DATA_SERVICE``, ``CREATE_FOLDER``, ``EXECUTE``, ``METADATA``, ``WRITE`` and ``FILE``.

Granting the privilege ``EXECUTE`` is equivalent to granting ``EXECUTE`` and ``METADATA`` but not vice versa.


.. code-block:: bnf
   :caption: Syntax of the GRANT/REVOKE clauses of CREATE DATABASE and ALTER DATABASE
   :name: Syntax of the GRANT/REVOKE clauses of CREATE DATABASE and ALTER DATABASE

   <grant> ::=
       GRANT <database privileges> TO [ ROLE ] <user:identifier>
     | REVOKE <database privileges> TO [ ROLE ] <user:identifier>

   <database privileges> ::=
       ADMIN
     | ALL PRIVILEGES
     | <database privilege list>

   <database privilege list> ::= 
     <database privilege> [, <database privilege> ]*

   <database privilege> ::=
       CONNECT
     | CREATE
     | CREATE_DATA_SERVICE
     | CREATE_VIEW
     | CREATE_DATA_SERVICE
     | CREATE_FOLDER
     | METADATA
     | EXECUTE
     | WRITE
     | FILE

Granting Privileges to a User/Role
=================================================================================

User privileges are granted with the statements ``CREATE USER``
(:ref:`Syntax of the CREATE USER statement`) and ``ALTER USER``
(:ref:`Syntax of the ALTER USER statement`).

Role privileges are granted with the statements ``CREATE ROLE`` and
``ALTER ROLE``.

The privileges are managed with the ``GRANT`` (assign privileges) and
``REVOKE`` (revoke privileges) clauses. Two cases can be distinguished:

-  Assign privileges over a database.
-  Assign privileges over specific views and stored procedures of the
   database.

`Syntax of the clauses GRANT/REVOKE of CREATE USER and ALTER USER`_ shows the syntax of the clauses for granting privileges over a
database or a specific view/procedure of a database.

You can grant/revoke all the access rights (``ALL PRIVILEGES``) or a
list of the following ones, to a user/role, over a database:

-  ``CONNECT``. The user can connect to the database. If the user does
   not have this privilege on a database, the other privileges are
   ignored.
-  ``CREATE``. The user can create new elements in the database.
-  ``CREATE_DATA_SOURCE``. The user can create new data sources in the database.
-  ``CREATE_VIEW``. The user can create new views in the database.
-  ``CREATE_DATA_SERVICE``. The user can create new data services (REST, SOAP web services and widgets) in the database.
-  ``CREATE_FOLDER``. The user can create new folders in the database.
-  ``METADATA``. The user has access to the metadata of the views but cannot query them.
-  ``EXECUTE``. The user will have “Execute” privileges over the elements of
   the database.
-  ``WRITE``. The user will have “Write” privileges over the elements of
   the database.
-  ``FILE``. The user will have the privilege "FILE", which will allow her to browse through the directories listed in the dialog "File privilege" of the wizard *Server Configuration*.
   
   The section :ref:`The FILE Privilege` of the Administration Guide explains how to enable this privilege and how it affects users.
   
-  ``ADMIN``. The user will have “Admin” (local administrator)
   privileges over the database.

You can grant/revoke a list of the following privileges to a user/role,
over a specific view/procedure:

-  ``EXECUTE``
-  ``METADATA``
-  ``WRITE``
-  ``INSERT`` (not applicable to stored procedures)
-  ``UPDATE`` (not applicable to stored procedures)
-  ``DELETE`` (not applicable to stored procedures)

The ``CREATE`` privilege implies ``CREATE_DATA_SOURCE``, ``CREATE_VIEW``, ``CREATE_DATA_SERVICE`` and ``CREATE_FOLDER``.

The ``WRITE`` privilege implies ``INSERT``, ``UPDATE`` and ``DELETE``.

The ``EXECUTE`` privilege implies ``METADATA``.

The privileges assigned over a specific element are only taken
into account when the user does *not* have the execute or write privilege
over the entire database.

The section :ref:`Types of Access Rights` of the Administration Guide lists
the actions a user can perform depending on the assigned privileges.


.. code-block:: bnf
   :caption:  Syntax of the clauses GRANT/REVOKE of CREATE USER and ALTER USER
   :name:  Syntax of the clauses GRANT/REVOKE of CREATE USER and ALTER USER

   <grant> ::=
         GRANT <database privileges> ON <database:identifier>
       | GRANT ROLE <role list>
       | GRANT <datasource privileges> ON <data source type> <database:identifier>.<datasource:identifier>
       | GRANT <view privileges> ON <database:identifier>.<view:identifier>
       | GRANT <procedure privileges> ON PROCEDURE 
               <database:identifier>.<procedure:identifier>
       | GRANT <data service privileges> ON <service type> <database:identifier>.<data service:identifier>
       | GRANT ADMIN ON <database:identifier>
       | GRANT METADATA ON <database:identifier>
       | GRANT METADATA ON <database:identifier>.<view:identifier>
       | GRANT EXECUTE ( <column list> ) ON <database:identifier>.<view:identifier>
       | GRANT EXECUTE ( <column list> ) ON PROCEDURE
               <database:identifier>.<procedure:identifier>
       | GRANT EXECUTE WHEN [ ANY ] ( <column list> ) THEN <condition:literal>
               [ MASKING ] ON <database:identifier>.<view:identifier>
       | GRANT EXECUTE WHEN [ ANY ] ( <column list> ) THEN <condition:literal>
               [ MASKING ] ON PROCEDURE 
               <database:identifier>.<procedure:identifier>
       | GRANT EXECUTE CUSTOM <policy:identifier>
               [ PARAMETERS ( <parameters list> ) ]
               ON <database:identifier>.<view:identifier>
       | GRANT EXECUTE CUSTOM <policy:identifier>
               [ PARAMETERS ( <parameters list> ) ]
               ON PROCEDURE <database:identifier>.<procedure:identifier>
       | REVOKE <database privileges> ON <database:identifier>
       | REVOKE ROLE <role list>
       | REVOKE <datasource privileges> ON <data source type> <database:identifier>.<datasource:identifier>
       | REVOKE <view privileges> ON <database:identifier>.<view:identifier>
       | REVOKE <procedure privileges> 
                ON PROCEDURE <database:identifier>.<procedure:identifier>
       | REVOKE <data service privileges> ON <service type> <database:identifier>.<data service:identifier>
       | REVOKE ADMIN ON <database:identifier>
       | REVOKE METADATA ON <database:identifier>
       | REVOKE METADATA ON <database:identifier>.<view:identifier>
       | REVOKE EXECUTE ( <column list> ) ON <database:identifier>.<view:identifier>
       | REVOKE EXECUTE ( <column list> )
                ON PROCEDURE <database:identifier>.<procedure:identifier>
       | REVOKE EXECUTE WHEN [ ANY ] ( <column list> ) THEN <condition:literal>
                [ MASKING ] ON <database:identifier>.<view:identifier>
       | REVOKE EXECUTE WHEN [ ANY ] ( <column list> ) THEN <condition:literal>
                [ MASKING ] 
                ON PROCEDURE <database:identifier>.<procedure:identifier>
       | REVOKE EXECUTE CUSTOM <policy:identifier>
                [ PARAMETERS ( <parameters list> ) ] 
                ON <database:identifier>.<view:identifier>
       | REVOKE EXECUTE CUSTOM <policy:identifier>
                [ PARAMETERS ( <parameters list> ) ]
                ON PROCEDURE <database:identifier>.<procedure:identifier>
   
   <database privileges> ::=
         ALL PRIVILEGES
       | <database privilege list>
   
   <database privilege list> ::= 
     <database privilege> [, <database privilege> ]*
   
   <database privilege> ::=
       CONNECT
     | CREATE
     | CREATE_DATA_SERVICE
     | CREATE_VIEW
     | CREATE_DATA_SERVICE
     | CREATE_FOLDER
     | FILE
     | EXECUTE
     | METADATA
     | WRITE
   
   <role list> ::= 
     <role:identifier> [, <role:identifier> ]*
   
   <column list> ::= 
     [ <column:identifier> [, <column:identifier>, ]* ]
   
   <view privileges> ::=
       ALL PRIVILEGES
     | <view privileges list>
   
   <view privileges list> ::= 
     <view privilege> [, <view privilege> ]*
   
   <view privilege> ::=
       EXECUTE
     | METADATA
     | WRITE
     | INSERT
     | UPDATE
     | DELETE
   
   <procedure privileges> ::=
       ALL PRIVILEGES
     | <procedure privileges list>
   
   <procedure privileges list> ::=
     <procedure privilege> [, <procedure privilege> ]*
   
   <procedure privilege> ::=
       EXECUTE
     | METADATA
     | WRITE
     | INSERT
     | UPDATE
     | DELETE
   
   <datasource privileges> ::=
       ALL PRIVILEGES
     | <datasource privileges list>
   
   <datasource privileges list> ::=
     <data source privilege> [, <data source privilege> ]*
   
   <datasource privilege> ::=
       METADATA
     | EXECUTE
     | WRITE
   
   <data source type> ::=
       ARN 
     | CUSTOM
     | DF 
     | ESSBASE 
     | GS 
     | ITP 
     | JDBC 
     | JSON
     | LDAP 
     | ODBC 
     | SALESFORCE
     | SAPBWBAPI 
     | SAPERP 
     | WS 
     | XML
   
   <data service privileges> ::=
       ALL PRIVILEGES
     | <data service privileges list>
   
   <data service privileges list> ::=
     <data service privilege> [, <data service privilege> ]*
   
   <data service privilege> ::=
       METADATA
     | WRITE
   
   <service type> ::=
       WEBSERVICE
     | WIDGET
     | LISTENER
   
   <parameters list> ::= 
     <name:literal <value> [, name:literal <value> ]*
   
   <value> ::=
       NULL
     | <number>
     | <boolean>
     | <literal>
  
Besides assigning specific privileges, you can assign roles to a user with the parameter ``GRANT ROLE`` (see section :ref:`Managing User Roles`).

In addition to assigning “Execute” privileges over a database or some of
its elements, you can assign more fine-grained
privileges:

-  **Column privileges**. When a user has EXECUTE privilege over a view, the
   user can query this view to obtain all its data. However, if you want a
   user to project only some of the columns of a view, you can use “Column
   privileges”.
   
   To do this, use the following parameters:

   -  ``GRANT EXECUTE ( <column list> ) ON...`` to restrict the columns that a
      user can obtain when querying a certain view. The user will only be
      able to obtain the columns of ``column list``.
   -  ``GRANT EXECUTE ( <column list> ) ON PROCEDURE...`` to restrict the
      columns that a user can obtain when executing a certain stored
      procedure.
      The sections :ref:`Types of Access Rights` and :ref:`Column Privileges` of the
      Administration Guide contain more information about this.

-  **Restrictions**. When a user/role has EXECUTE access over a view, you can
   add a restriction that filters the rows returned to that user. That way,
   when this user sends a query to a view, the result will only contain
   obtain the rows that match certain criteria.
   
   You can also specify that the filtering condition should be applied only
   when the query asks for a list of specific fields.
   
   To do this, use the following parameters:

   -  ``GRANT EXECUTE WHEN [ ANY ]( [ <column list> ] ) THEN <condition:literal> ON...``
      
      When the user queries this view, the result will only contain the
      rows that match the ``condition``.
      
      If ``column list`` is empty, the condition will be evaluated always.
      
      If it is not empty, the condition will only be evaluated when the
      query projects, at least, all the columns of ``column list``.
      
      If ``column list`` is not empty, the modifier ``ANY`` is **not**
      present and the query does **not** project all the fields of
      ``column list``, the query will return all the rows.
      
      If ``column list`` is not empty, the modifier ``ANY`` is present and
      the query projects **any** of the fields in ``column list``, the
      condition will be evaluated and it will only return all the rows that
      match it.
   -  ``GRANT EXECUTE WHEN [ ANY ] ( [ <column list> ] ) THEN <condition:literal> ON PROCEDURE...``
     
      It does the same as the previous structure, but it has to be used to
      assign these privileges over Virtual DataPort stored procedures,
      instead of views.

-  **Custom policies (CUSTOM)**. When a user/role has EXECUTE access over a
   view, you can add a custom policy that filter the rows returned to that
   user/role. The custom policies are similar to restrictions but can take
   into account more complex information.
   
   ``PARAMETERS`` contain the parameters passed to the custom policy.
   
   See the section :doc:`/vdp/developer/custom_policies/custom_policies` of the Developer Guide to learn how
   to develop custom policies and the section :doc:`Modifying the Privileges of a User </vdp/administration/databases_users_and_access_rights_in_virtual_dataport/administration_of_databases_users_roles_and_their_access_rights/modifying_and_deleting_users>` of the Administration Guide to learn how to apply them to
   users/roles.

`Example of assigning privileges to a user`_ shows an example in which
two databases are created, “database1” and “database2”. One user called
“user1” is also created. The new user is assigned the following
privileges on “database1”, “database2” and “admin”:

-  Has full privileges over the database ``database1``.
-  Has connection and creation access to ``database2``. It only has
   execute/write access to ``view1``.
-  Can only project the fields ``summary`` and ``taxid`` of the view
   ``internet_inc`` of the database ``admin``.
-  When she queries the view ``phone_inc`` of the database ``admin``,
   the Server will only return the rows that match the condition
   ``description containsand 'ADSL'``.

.. code-block:: vql
   :name: Example of assigning privileges to a user
   :caption: Example of assigning privileges to a user
   
   CREATE DATABASE database1 'Database1 Description';
   CREATE DATABASE database2 'Database2 Description';
   CREATE USER user1 'user1password' 'User1 Description'
      GRANT ALL PRIVILEGES ON database1
      GRANT CONNECT, CREATE ON database2
      GRANT EXECUTE, WRITE ON database2.view1
      GRANT EXECUTE (summary, taxid) ON admin.internet_inc
      GRANT EXECUTE WHEN() THEN 'description containsand ''ADSL'''
      ON admin.phone_inc;
  
