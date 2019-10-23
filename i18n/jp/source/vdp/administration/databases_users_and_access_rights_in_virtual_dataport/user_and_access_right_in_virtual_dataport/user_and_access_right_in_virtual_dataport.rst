=========================================
User and Access Right in Virtual DataPort
=========================================

.. contents:: 
   :depth: 1
   :local:
   :backlinks: none

Denodo Virtual DataPort distinguishes two types of users:

-  *Administrators*. They can create, modify and delete databases
   without any limitation. Likewise, they can also create, modify and
   delete users.
   When the server is installed, a default administrator user is created
   with user name ``admin`` and password ``admin``. There must always be
   at least one administrator user.
-  *Normal users*. They cannot create, modify or delete users or
   databases. Administrator users can grant them connection, Execute,
   create or write privileges to one or several databases or to specific
   views contained in them.
   
   An administrator can promote a normal user to *administrator of a database*
   of one or more databases, which means that this user will be able
   to perform administration tasks over these databases.
   
   A normal user with the role *serveradmin* can perform the same actions as an administrator.

A “Normal user” can have “Roles”, which are a set of access rights over
databases and their views and stored procedures. Roles allow
administrators to manage user privileges easily because by changing the
privileges assigned to a role, they change the privileges of all the
users that “belong” to that role.

Types of Access Rights
======================

Virtual DataPort access rights are applied to a specific user or a role,
to delimit the tasks they can perform over databases, views and stored
procedures.

The following access rights only affect “Normal” users. That is because
normal users promoted to local administrators of a given database can
perform any task within that database and administrators can perform any
task within the entire Server.

An administrator with the role "assignprivileges" can grant privileges to a user or a role, over an
entire database or over specific views or stored procedures of a
database.

You can grant the following privileges to a user or a role, over a
database:

-  :ref:`Connect <Connection Privilege>`
-  :ref:`Create <Create Privilege>`
-  :ref:`Create Data Source <Create Data Source Privilege>`
-  :ref:`Create View <Create View Privilege>`
-  :ref:`Create Data Service <Create Data Service Privilege>`
-  :ref:`Create Folder <Create Folder Privilege>`
-  :ref:`Metadata <Metadata Privilege>`
-  :ref:`Execute <Execute Privilege>`
-  :ref:`Write <Write Privilege>`
-  :ref:`File <vdp_admin_guide__user_and_access_rights_in_vdp_file_privilege>`
-  :ref:`Admin <Admin Privilege>`
-  :ref:`Insert, Update and Delete <Insert, Update and Delete Privileges>`


Connection Privilege
--------------------

Users with the *Connection* privilege can connect to this database.

To revoke all the privileges of a user temporarily, revoke her roles and
the connection privileges over all the databases.

Create Privilege
-----------------------

Users with the *Create* privilege can do the following:

-  Create/drop data sources, views, stored procedures, Web services, etc.
   I.e. execute ``CREATE`` statements.
   If the user wants to create a derived view, she also needs to have
   Execute access over the entire database or at least, over the views
   involved in the new view.

-  Deploy and redeploy the Denodo Web services that they created.
-  Deploy and redeploy the auxiliary Web services of the widgets that
   they created.

Create Data Source Privilege
----------------------------

Users with the *Create Data Source* privilege can do the following:

-  Create/drop data sources i.e. execute ``CREATE DATASOURCE`` statements.
   
Create View Privilege
---------------------

Users with the *Create View* privilege can do the following:

-  Create/drop views, stored procedures and types i.e. execute ``CREATE WRAPPER``, ``CREATE TYPE``, ``CREATE WRAPPER`` and ``CREATE PROCEDURE`` statements.
   If the user wants to create a derived view, she also needs to have
   Execute access over the entire database or at least, over the views
   involved in the new view.

Create Data Service Privilege
-----------------------------

Users with the *Create Data Service* privilege can do the following:

-  Create/drop web services, widgets and JMS listeners i.e. execute ``CREATE REST/SOAP WEBSERVICE``, ``CREATE WIDGET`` and ``CREATE LISTENER JMS`` statements.

-  Deploy and redeploy the Denodo Web services that they created.
-  Deploy and redeploy the auxiliary Web services of the widgets that
   they created.

Create Folder Privilege
-----------------------

Users with the *Create Folder* privilege can do the following:

-  Create/drop folders i.e. execute ``CREATE FOLDER`` statements.

Execute Privilege
-----------------

Users with the *Execute* privilege over a database can do the following:

-  Perform introspection of data sources.
-  Query any view/stored procedure of the database. I.e. execute
   ``SELECT``/``CALL`` statements.
   
|

If you grant this privilege over a specific views or stored procedure instead of a database, the user will be able to do the following:

-  Query the view/procedure. I.e. execute ``SELECT``/``CALL``
   statements.

|

You can grant more fine-grained EXECUTE privileges over specific views:

-  Column privileges (see section :ref:`Column Privileges`)
-  Row restrictions (see section :ref:`Row Restrictions`)

Metadata Privilege
------------------

Users with the *Metadata* privilege can do the following:

-  View the list of views and stored procedures of the database. I.e.
   execute ``LIST`` statements.
 
-  View the schema of the views and stored procedures.

-  Open the dialogs *Edit* and *Options* of the views. However, without the *write* privilege, the user will not be able to modify the view.

-  Open the *Tree view* and *Data lineage* of the views.

-  Obtain the execution plan of any query, without actually running the query.

   To see the query plan of a query, open the *Execute* dialog of the view and click *Query plan*. Alternatively, open the VQL Shell and prefix the query with ``DESC QUERYPLAN``. For example,
   
   .. code-block:: none
      :emphasize-lines: 1
   
      DESC QUERYPLAN
        SELECT count(*), sum(total)
        FROM invoice
        GROUP BY billing_state

|
      
If you grant the privilege metadata over a set of views/procedures instead of a database, the differences are:

-  The user will only have access to these views/procedures, not all the database.

-  The query plan will only show information about the views the user has Execute access. The Tool will not show anything related to data sources. E.g. the Tool will not display the SQL queries delegated to the databases.

|

The main goals of this privilege are:

1.  Allow developers to connect to the production servers and troubleshoot issues, without seeing real-live data. For example, to see the full execution plans of queries.
#.  Let us say that you store all the data sources and their base views on a central database and each project is created in its own database. Thanks to the metadata privilege, the developers of each project will be able to see the full query plan, not just part of it.
  
Write Privilege
---------------

If you grant the privilege *Write* to a user/role, implicitly you are granting *Execute*.

Users with the *Write* privilege can do the following:

-  Delete the views and stored procedures of the database.
   A user can delete the data sources, Web services and widgets that she
   has created, but not the ones created by other users.
-  Modify the views and stored procedures of the database.
   Users can also alter their own data sources, Web services and
   widgets, but not modify the ones created by other users.
-  Execute ``INSERT``, ``UPDATE`` and ``DELETE`` statements on views of this
   database.
-  Move elements across the existing folders of the database, except if
   that element is another folder.
-  Undeploy the Denodo Web services that they created.
-  Undeploy the auxiliary Web services of the widgets that they 
   created.
-  View the VQL of the following elements:

   -  Data sources, derived views, base views, stored procedures, JMS listeners, web services and widgets.
      
|

If you grant this privilege over a specific view or stored procedure instead of a database, the user will be able to do the following:
   

-  Delete the view/procedure. If there are other elements that depend on
   the one being deleted, the user needs to have Write access over these
   other elements as well because they will be deleted on cascade.
-  Modify the view/procedure. The user can only modify a derived view if
   it has the “Execute” privilege over the subviews of the view.
-  Move the view/procedure across the existing folders of the database.
-  Execute ``INSERT``, ``UPDATE`` and ``DELETE`` statements over the
   view/procedure.
-  View the VQL of the element. 
  
.. _vdp_admin_guide__user_and_access_rights_in_vdp_file_privilege:


File Privilege
--------------

Users with the *File* privilege can browse through the directories listed in the dialog *File privilege* of the wizard *Server Configuration*.  

Admin Privilege
---------------

This privilege can only be assigned over an entire database. Users with this privilege have the privileges Connect, Create, Metadata, Execute and Write over all the elements of that database. Besides, they can do the following tasks:

-  Set the configuration parameters of the database: I18N, cache, swap,
   etc.
-  Edit the description of the database.
-  Grant / revoke privileges to normal users over the elements of the
   database.
   It cannot grant the Admin privilege to other users.
-  Grant / revoke privileges to roles over the elements of the database.
-  Create / rename / drop folders.
-  List, deploy, redeploy and undeploy all the Denodo Web services and
   the auxiliary Web services of widgets.
      
A user with this privilege cannot do the following:
   
-  Create / delete users
-  Create / delete roles
-  Grant / revoke roles to users or other roles
-  Change users’ password
-  Create / drop databases
-  Grant Admin privileges to other users.


Insert, Update and Delete Privileges
------------------------------------

Users with the *Insert* privilege can execute ``INSERT`` statements over the view.

Users with the *Update* privilege can execute ``UPDATE`` statements over the view.

Users with the *Delete* privilege can execute ``DELETE`` statements over the view.

These privileges are not applicable to stored procedures.

You can only grant these privileges over individual views, not databases. If you want to grant a user to privilege to run these statements over all the views of a database, grant her the *Write* privilege.


Column Privileges
=================

When you grant the EXECUTE privilege to a user/role over a view, this user or
the users with this role can:

-  Execute a SELECT statement to query this view
-  If the user/role also has the INSERT/UPDATE/DELETE privileges over a view, they can execute INSERT/UPDATE/DELETE statements over this view. 

In some scenarios, you may want to limit the access to certain columns of a view to users. The columns of a view that cannot be queried or modified through INSERT/UPDATE/DELETE statements are called *protected columns*.


For example, let us say you have a view "employee". You may want to forbid
users with the role "developer" to see the column "salary". That is, you want "salary" to be a protected column for the users of the role "developer". To do this, follow these steps:

#. Click the menu **Administration** > **Role Management**. In the tables with roles, select the role "developer".
#. Click **Assign privileges** and on the row of the database of the view "employee", click **edit**.
#. Select, at least, the privilege "Execute" over this view.
#. Select the row of the view "employee" and click **Assign column privileges**.
#. In this dialog, clear the check box next to the field "salary".

With this change, the user will be able to run this query:

.. code-block:: sql

   SELECT ename
   FROM employee

However, this one will fail:

.. code-block:: sql

   SELECT ename, salary 
   FROM employee

If instead of making the query fail, you want to mask the values of a column, you can specify a row restriction with column masking (see section :ref:`Row Restrictions` below).

When assigning column privileges, consider this:

-  If you assign column privileges to a user over a view, the statement executed by this user will fail if the query references a protected column of this view in any of the clauses of the query (SELECT, WHERE, GROUP BY, etc.). This is to ensure that users cannot guess the value of protected columns by for example, using them in the WHERE clause. Find an example of this in the next section.
-  Column privileges do not affect global administrators or administrators of the database to which the view belongs. They can reference protected columns in their statements.
-  By default, the Execution Engine only takes into account column privileges, row restrictions and custom policies granted over a view to a user/role, *when this view is directly referenced on the statement*; not if the statement references another view that is derived from this one. You can configure the Execution Engine to propagate these restrictions to all the views derived from the ones over which you assign these restrictions. The section :ref:`Enforcing Column Privileges, Row Restrictions and Custom Policies` below explains how to do this.

The following table lists the behavior of column privileges for each DML statement:

.. table:: Behavior of column privileges for each type of statement

   +--------------------------------------------------------------+----------------------------------------------+
   |  Statement                                                   | Behavior                                     |
   +=================================+============================+==============================================+
   | SELECT                          | Does not reference         | Query runs                                   |
   |                                 | protected columns          |                                              |
   |                                 +----------------------------+----------------------------------------------+
   |                                 | References protected       | Query fails                                  |
   |                                 | columns                    |                                              |
   +---------------------------------+----------------------------+----------------------------------------------+
   | CREATE MATERIALIZED TABLE       | Does not reference         | Query runs                                   |
   | <materialized table> AS SELECT  | protected columns          |                                              |
   | ... FROM <view with restricted  +----------------------------+----------------------------------------------+
   | columns>                        | References                 | Query fails                                  |
   |                                 | protected columns          |                                              |
   +---------------------------------+----------------------------+----------------------------------------------+
   | INSERT                                                       | Statement always runs                        |
   +---------------------------------+----------------------------+----------------------------------------------+
   | INSERT INTO <materialized view> | Does not reference         | Statement runs                               |
   | SELECT ... FROM <view with      | protected columns          |                                              |
   | restricted columns>             +----------------------------+----------------------------------------------+
   |                                 | References protected       | Statement fails                              |
   |                                 | columns                    |                                              |
   +---------------------------------+----------------------------+----------------------------------------------+
   | UPDATE                          | Does not reference         | Statement runs                               |
   |                                 | protected columns          |                                              |
   |                                 +----------------------------+----------------------------------------------+
   |                                 | References protected       | Statement fails                              |
   |                                 | columns                    |                                              |
   +---------------------------------+----------------------------+----------------------------------------------+
   | DELETE                          | Does not reference         | Statement runs                               |
   |                                 | protected columns          |                                              |
   |                                 +----------------------------+----------------------------------------------+
   |                                 | References protected       | Statement fails                              |
   |                                 | columns                    |                                              |
   +---------------------------------+----------------------------+----------------------------------------------+

.. note:: In Denodo 7.0 update 20181011 and previous ones, column restrictions do not affect the write/update/delete operations.
   They only affect the ``SELECT`` ones.

Row Restrictions
=================

When a user/role has the EXECUTE/UPDATE/DELETE privilege over a view, you can define a restriction over that view
to restrict the rows returned to that user/role or affected by the UPDATE/DELETE statements.
That way, when that user executes a DML operation over the view, they will only obtain/update/delete the rows that
match a certain condition and/or some of the fields of some rows will be masked.

For example, let us say you want to restrict the access to the view "employee" to users with role "developer".
To assign a row restriction to a user/role: edit that user/role, select the view you want to restrict
("employee" in our example), click *Assign restrictions* and then click *New restriction*.

In this dialog, you first need to specify a condition. The user will be able to see the rows that do meet the condition you enter (e.g. ``position <> 'manager'``). For the rows that *do not* meet the condition, you have to select one of the following actions:

1. **Reject row**. In our example, the user will only see/update/delete the employees who are not managers.
#. **Reject row if any or all sensitive fields are used** (you can select which ones). For example, you could allow the user to see all the employees' data if the statement does not reference use the column "salary". If the select/update/delete operation references the column "salary" then the statement will only affect the employees that are not managers.
#. **Mask sensitive fields if any or all of them are used** (you can select which ones). For example, you could allow the user to see all the employees' data, but for the ones who are managers, the values of the field salary will be replaced by NULL.

You can develop what is called a *custom policy* that takes into account any information you want to decide if the result of the query
is filtered or not. The section
:doc:`Custom Policies <../../../developer/custom_policies/custom_policies>` of the Developer Guide explains what they
are and how to develop them.

When assigning row restrictions, consider this:

-  The row restrictions do not affect global administrators or administrators of the database to which the view belongs. I.e. no row will be rejected or masked regardless for them.
-  By default, the Execution Engine *only applies* column restrictions, row restrictions and custom policies granted over a view to a user/role, *when this view is directly referenced on the statement*; not if the statement references another view that is derived from this one. You can configure the Execution Engine to propagate these restrictions to all the views derived from the ones over which you assign these restrictions. The section :ref:`Enforcing Column Privileges, Row Restrictions and Custom Policies` below explains how to do this.
-  The option *Reject row if any/all sensitive fields are used* and the masking are applied when the query references a sensitive field of this view from any of the clauses of the query (SELECT, WHERE, GROUP BY, etc.). This is to avoid users guessing the value of a sensitive field by using it from the WHERE clause. For example, if the sensitive field salary was only masked on the SELECT clause, the user could try to guess the values using queries like:

.. code-block:: sql

   SELECT ename FROM employee WHERE salary > 50000 and salary < 100000;

or

.. code-block:: sql

   UPDATE employee SET ename = ename || '_100000' WHERE salary > 100000

|

The following table shows the behavior of "Reject Row" restrictions, for each type of DML statement:

.. table:: Behavior of "Reject Row" restrictions, for each type of statement

   +--------------------------------------------------+----------------------------------------------+
   | Statement                                        | Behavior                                     |
   +==================================================+==============================================+
   | SELECT                                           | Virtual DataPort executes the DML command    |
   |                                                  | including the condition from the restriction |
   +--------------------------------------------------+----------------------------------------------+
   | CREATE MATERIALIZED TABLE <materialized table>   | Virtual DataPort executes the DML command    |
   | AS SELECT ... FROM <view with restrictions>      | including the condition from the restriction |
   +--------------------------------------------------+----------------------------------------------+
   | INSERT                                           | No restrictions                              |
   +--------------------------------------------------+----------------------------------------------+
   | INSERT INTO <materialized table>                 | Virtual DataPort executes the DML command    |
   | SELECT ... FROM <view with restrictions>         | including the condition from the restriction |
   +--------------------------------------------------+----------------------------------------------+
   | UPDATE                                           | Virtual DataPort executes the DML command    |
   |                                                  | including the condition from the restriction |
   +--------------------------------------------------+----------------------------------------------+
   | DELETE                                           | Virtual DataPort executes the DML command    |
   |                                                  | including the condition from the restriction |
   +--------------------------------------------------+----------------------------------------------+

For example, let us say you assign a row restriction over the view "employee" to the role "sales_manager". 
This row restriction rejects rows that do not verify the condition ``department = 'sales'``.

If a user with role "sales_manager" executes:

.. code-block:: sql

   SELECT * FROM employee

| Virtual DataPort will return the results corresponding to the query:

.. code-block:: sql

   SELECT * FROM employee WHERE department = 'sales'

If the user has the UPDATE privilege over employee and executes:

.. code-block:: sql

   UPDATE employee SET manager_id = 1 WHERE manager_id = 2

| Virtual DataPort will perform the update:

.. code-block:: sql

   UPDATE employee SET manager_id = 1 WHERE manager_id = 2 AND department = 'sales'

|

The following table shows the behavior of restrictions "Reject Row if sensitive fields are used", for each type of DML statement:

.. table:: Behavior of restrictions "Reject Row if sensitive fields are used", for each type of statement

   +---------------------------------------------------------------+----------------------------------------------+
   | Statement                                                     | Behavior                                     |
   +==================================+============================+==============================================+
   | SELECT                           | Not using sensitive fields | No restrictions                              |
   |                                  +----------------------------+----------------------------------------------+
   |                                  | Using sensitive fields     | Virtual DataPort executes the DML command    |
   |                                  |                            | including the condition from the restriction |
   +----------------------------------+----------------------------+----------------------------------------------+
   | CREATE MATERIALIZED TABLE        | Not using sensitive fields | No restrictions                              |
   | <materialized table> AS SELECT   |                            |                                              |
   | ... FROM <restricted view>       +----------------------------+----------------------------------------------+
   |                                  | Using sensitive fields  in | Virtual DataPort executes the DML command    |
   |                                  | SELECT query               | including the condition from the restriction |
   +----------------------------------+----------------------------+----------------------------------------------+
   | INSERT                                                        | No restrictions                              |
   +----------------------------------+----------------------------+----------------------------------------------+
   | INSERT INTO <materialized table> | Not using sensitive fields | No restrictions                              |
   | SELECT ... FROM <view with       |                            |                                              |
   | restrictions>                    +----------------------------+----------------------------------------------+
   |                                  | Using sensitive fields in  | Virtual DataPort executes the DML command    |
   |                                  | SELECT query               | including the condition from the restriction |
   +----------------------------------+----------------------------+----------------------------------------------+
   | UPDATE                           | Not using sensitive fields | No restrictions                              |
   |                                  +----------------------------+----------------------------------------------+
   |                                  | Using sensitive fields     | Virtual DataPort executes the DML command    |
   |                                  |                            | including the condition from the restriction |
   +----------------------------------+----------------------------+----------------------------------------------+
   | DELETE                           | Not using sensitive fields | No restrictions                              |
   |                                  +----------------------------+----------------------------------------------+
   |                                  | Using sensitive fields     | Virtual DataPort executes the DML command    |
   |                                  |                            | including the condition from the restriction |
   +----------------------------------+----------------------------+----------------------------------------------+

For example, let us say you assign a row restriction over the view "employee" to the role "developer".
This row restriction rejects rows that do not verify the condition ``position <> 'manager'``, but only if the statement uses
the sensitive column salary.

If a user with role "developer" executes:

.. code-block:: sql

   SELECT ename FROM employee

| the query will return all rows from employee. However, if the user executes:

.. code-block:: sql

   SELECT ename FROM employee WHERE salary > 50000

| it will return the results corresponding to the query:

.. code-block:: sql

   SELECT ename FROM employee WHERE salary > 50000 and position <> 'manager'

The same way, if the same user executes:

.. code-block:: sql

   CREATE MATERIALIZED TABLE employee_salary AS SELECT ename, salary FROM employee

| Virtual DataPort will create a materialized table with the results obtained from the query:

.. code-block:: sql

   SELECT ename, salary FROM employee WHERE position <> 'manager'

|

The following table show the behavior of restrictions "Mask sensitive fields if ANY/ALL of them are used", for each type of statement:

.. table:: Behavior of restrictions "Mask sensitive fields if ANY/ALL of them are used", for each type of statement

   +----------------------------------+----------------------------+----------------------------------------------+
   | DML Operation                                                 | Behavior                                     |
   +==================================+============================+==============================================+
   | SELECT                           | Not using sensitive fields | No restrictions                              |
   |                                  +----------------------------+----------------------------------------------+
   |                                  | Using sensitive fields     | Virtual DataPort masks the value for         |
   |                                  |                            | the sensitive columns                        |
   +----------------------------------+----------------------------+----------------------------------------------+
   | CREATE MATERIALIZED TABLE        | Not using sensitive fields | No restrictions                              |
   | <materialized table> AS          |                            |                                              |
   | SELECT * FROM <view with         +----------------------------+----------------------------------------------+
   | restrictions>                    | Using sensitive fields  in | Virtual DataPort masks the value for         |
   |                                  | SELECT query               | the sensitive columns                        |
   +----------------------------------+----------------------------+----------------------------------------------+
   | INSERT                                                        | No restrictions                              |
   +----------------------------------+----------------------------+----------------------------------------------+
   | INSERT INTO <materialized table> | Not using sensitive fields | No restrictions                              |
   | SELECT * FROM <view with         +----------------------------+----------------------------------------------+
   | restrictions>                    | Using sensitive fields  in | Virtual DataPort masks the value for         |
   |                                  | SELECT query               | the sensitive columns                        |
   +----------------------------------+----------------------------+----------------------------------------------+
   | UPDATE                           | Not using sensitive fields | No restrictions                              |
   |                                  +----------------------------+----------------------------------------------+
   |                                  | Using sensitive fields     | Virtual DataPort executes the DML command    |
   |                                  |                            | including the condition from the restriction |
   +----------------------------------+----------------------------+----------------------------------------------+
   | DELETE                           | Not using sensitive fields | No restrictions                              |
   |                                  +----------------------------+----------------------------------------------+
   |                                  | Using sensitive fields     | Virtual DataPort executes the DML command    |
   |                                  |                            | including the condition from the restriction |
   +----------------------------------+----------------------------+----------------------------------------------+

Using the previous example, if a user with role "developer" executes:

.. code-block:: sql

   SELECT ename, salary FROM employee

It will return all rows, but for those rows corresponding to managers, the values of salary will be null.

The same way, if the same user executes the query:

.. code-block:: sql

   SELECT ename FROM employee WHERE salary > 50000

The user will see the result of the following query:

.. code-block:: sql

   SELECT ename FROM employee WHERE CASE WHEN (position <> 'manager') THEN salary ELSE null END > 50000

This is, the names for the employees whose position is not manager.

If the same user has the privilege DELETE over employee and executes:

.. code-block:: sql

   DELETE FROM employee

It will delete all employees. However, if the user executes:

.. code-block:: sql

   DELETE FROM employee WHERE salary > 50000

It will only delete the rows WHERE salary > 50000 and position is not 'manager'

.. note:: In Denodo 7.0 update 20181011 and previous ones, row restrictions do not affect the write/update/delete statements.
   They only affect the ``SELECT`` statements.

Enforcing Column Privileges, Row Restrictions and Custom Policies
=================================================================

By default, the Execution Engine *only applies* column restrictions, row restrictions and custom policies granted over a view to a user/role, when this view is directly referenced from the statement. For example, let us say you define a column restriction for the role "developer" over the view "employee". This restriction forbids this role to project the column "salary".

If a user with this role executes the following query, the query will fail:

.. code-block:: sql

   SELECT ename, salary 
   FROM employee

But if another user created the following view:

.. code-block:: sql

   CREATE VIEW employee_dept1 AS 
     SELECT ename, salary 
     FROM employee
     WHERE deptno = 1

| the users with the role "developer" that also have the privilege EXECUTE over the whole database or over this new view, they will be able to query this view (i.e. obtaining the field "salary"), even though this role has a column restriction defined over "employee".

Because of this behavior, if you want to assign restrictions to users/roles over some views, do not grant them the privilege EXECUTE over the whole database. Instead, only grant them privileges over each view individually.

A user/role with column/row restrictions or custom privileges over a view is not allowed to create a view derived from this view. This means that if a user with the role "developer" runs this:

.. code-block:: sql

   CREATE VIEW employee_new AS
     SELECT ename, salary 
     FROM employee

| the command will fail.

Enforce Restrictions on All Queries
----------------------------------------

If you want the Execution Engine to always enforce the column/row restrictions and custom policies regardless of if the view is directly involved in the statement or not, follow these steps:

1. Log into the administration tool with an administrator account:

2. Execute this from the VQL Shell:

   .. code-block:: vql

      SET 'com.denodo.vdb.catalog.user.User.enableCheckViewRestrictionAlways' = 'true';

   The change is applied immediately, you do not need to restart.

3. Choose if you want to enable this behavior over a specific database or all of them:

   a. To enable this behavior on all databases, execute this:

      .. code-block:: vql

         SET 'com.denodo.vdb.catalog.user.User.checkViewRestrictionAlways' = 'true';

      The change is applied immediately, you do not need to restart.

   b. To enable this behavior only on some databases, execute this:

      .. code-block:: vql

         ALTER DATABASE <database> 
         CHECK_VIEW_RESTRICTIONS { ALWAYS | DIRECT_QUERIES_ONLY | DEFAULT }

      For example:

      .. code-block:: none

         ALTER DATABASE core 
         CHECK_VIEW_RESTRICTIONS ALWAYS;
      
      If you specify ``DEFAULT``, the database will follow the behavior specified by the property ``com.denodo.vdb.catalog.user.User.checkViewRestrictionAlways``.

This affect queries (SELECT) and INSERT, UPDATE and DELETE statements

After enabling this behavior, the column/row restrictions and custom policies defined over a view will always be enforced, whether the
statement references the view directly or indirectly. This means that even if a user with the role "developer" can query the view "employee_dept1", the query:

.. code-block:: sql

   SELECT * from employee_dept1

| will fail because it is using the column "salary" of the view "employee".

When this option is enabled, the user is allowed to create views over a view with restrictions if it has the right privileges (CREATE or CREATE VIEW). That is because even if the user creates the view, the query will still fail. Note that this is different from when this option is disabled; in that case, the user is never allowed to create derived views over views with restrictions.

Roles
=====

A role is a set of access rights that we can grant to users. The benefit
of assigning roles to users instead of assigning privileges is that the
management of permissions is easier. If you change the privileges of a
role, the changes are applied to all the users that “belong” to that
role. Without roles, you would have to edit the privileges of each user.

A user can have more than one role assigned and her “effective
permissions” will be the union of the permissions assigned by each role.
For example, if there is a role A that grants execute permission over the
database “admin” and another role B that grants execute permission over the
database “tests”, a user with the roles A and B will have execute
privileges over the databases “admin” and “tests”.

In addition, you can assign roles to other roles. This is called “Role
inheritance”. For example, if you have the following roles:

-  A role ``vdp_developer`` with the privileges ``CONNECT``, ``EXECUTE``,
   ``CREATE`` and ``WRITE`` over the database ``admin``.
-  A role ``itpilot_developer`` with the privileges ``CONNECT``,
   ``EXECUTE``, ``CREATE`` and ``WRITE`` over the database ``itpilot``.

You can create a role ``denodo_developer`` that has assigned the roles
``vdp_developer`` and ``itpilot_developer`` to it. The users with this
new role will have the privileges ``CONNECT``, ``EXECUTE``, ``CREATE`` and
``WRITE`` over the databases ``admin`` and ``itpilot``.

The permissions assignable to a role are the same that we can assign to
a user.

|

Virtual DataPort defines the following default roles. They cannot be modified nor deleted.

-  ``allusers``: granted by default to new local users. Additionally, you can automatically grant it to all users that connect to Virtual DataPort using Kerberos authentication, SAML 2.0 or to a database with LDAP authentication. 

-  ``diagnostic_monitoring_tool_admin``: grants :ref:`administration privileges over the Diagnostic and Monitoring tool <dmt-authentication-authorization>`.

-  ``jmxadmin``: grants the privilege of connecting to the JMX interface of Virtual DataPort.
   
   You need this privilege to monitor the Server using JMX tools such 
   as Denodo Monitor, Oracle VisualVM, Oracle Java Mission Control, Nagios, etc.


-  ``data_catalog_admin``, ``data_catalog_classifier``, ``data_catalog_content_admin``, ``data_catalog_editor``,
   ``data_catalog_exporter``, ``data_catalog_manager``: each of these roles grants different privileges over the Data Catalog.
   The section :ref:`Administration <data_catalog_administration>` of the Data Catalog Guide explains what privileges each of these roles grant.
   
   The roles ``selfserviceadmin`` and ``selfserviceexporter`` are deprecated and should not be granted to users anymore. 
   These roles exist to keep backward compatibility with Denodo 6.0 but you should not grant them to users anymore. Instead, grant the roles ``data_catalog_admin`` and ``data_catalog_exporter``, which are equivalent to ``selfserviceadmin`` and ``selfserviceexporter`` respectively.
   
   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

-  ``scheduler_admin``: used by the Scheduler Administration
   Tool. The users that have this role assigned can perform any task in
   the Scheduler Administration Tool.

   See more about this in the section :doc:`Permissions <../../../../scheduler/administration/administration/server_configuration/permissions>` 
   of the Scheduler Administration Guide.

-  ``serveradmin``: equivalent to being an administrator user of  Virtual DataPort, except that it does not grant the privilege
   of connecting to Virtual DataPort via JMX. That is, a user with this
   role can manage databases, change settings of the Server, etc. A user with this role also needs the role "assignprivileges" (see below) to manage the privileges of users and roles.

-  ``solution_manager_admin``, ``solution_manager_promotion``, ``solution_manager_promotion_admin``, ``solution_manager_promotion_admin_development``, ``solution_manager_promotion_admin_production``, ``solution_manager_promotion_admin_staging``, ``solution_manager_promotion_development``, ``solution_manager_promotion_production``, ``solution_manager_promotion_staging``: each of these roles grants different privileges over the Solution manager. The section :ref:`Authorization` of the Solution Manager Administration Guide explains what privileges each of these roles grant.

-  ``web_panel_admin``: grants :ref:`administration privileges <web_panel_authentication>` over the Web Panel. 
         
-  ``assignprivileges``: grants the privilege of granting/revoking privileges to other users.
   
   .. note:: Without this role, a user cannot grant/revoke privileges to the users/roles, not even an administrator.
 
   Take the following into account:
   
   -  New administrator users have this role by default but you can revoke it from them.
   -  You cannot grant privileges or roles to it. This is why it is not listed in the "Role Management" panel, but it is listed in the "Assign roles" dialogs.
   -  You can only assign it to administrators or to users that are administrators of at least one database.
   -  A non-administrator user with this role can only modify the privileges of the databases for which it is an administrator.
   -  Only administrators with this role can grant/revoke roles to users or other roles.
   -  Only administrators with this role can modify the description of a role.   

