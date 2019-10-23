================
Single User Mode
================

When a server is running in “Single User Mode” it means the availability
of the Server is limited.

You may want to switch the server to “Single User Mode” when you are performing
maintenance operations such as importing a large VQL file or modifying the settings
of the Server. In addition, the Server itself may switch to Single User Mode
to perform actions that cannot be executed concurrently with other
actions.

While the Server is running in single user mode, it does not process any
request. That is:

-  Queries. I.e. ``SELECT`` and ``CALL`` statements.
-  Data Definition Language statements (DDL). I.e.
   ``CREATE``/``ALTER DATA SOURCE``, ``CREATE``/``ALTER VIEW``, etc.
-  Data Modification Language statements (DML). I.e. ``INSERT INTO``,
   ``UPDATE`` or ``DELETE``.
-  …

All the requests received while the Server is running in “single user
mode” are queued.

After exiting single user mode, the Server executes the queued requests
in the order they were received.

If the Server is in single user mode and a statement is executed from a connection different from the one
who triggered the switch, the statement will be queued. If this statement is executed from the administration tool, the administration tool will show a "waiting dialog" with the message “Waiting
for single user mode to finish”.

There are two types of single user mode:

#. *Explicit* or *global* single user mode: the user requests the Server
   to enter this mode. See section :ref:`Explicit Single User Mode`.
#. *Auto* single user mode: the Server automatically enters and exits
   single user mode. See section :ref:`Automatic Single User Mode`.

Explicit Single User Mode
=========================

An administrator user may request the Server to switch to “explicit single
user mode” to perform configuration changes or import a VQL file. Only
administrator users can make the Server switch to single user mode.

The Server switches to explicit single user mode when an administrator
performs one of the following actions:

#. Executes the statement ``ENTER SINGLE USER MODE``. To exit this mode,
   execute ``EXIT SINGLE USER MODE``.
#. Executes the ``import`` script with the option ``--singleuser`` (see
   more about this script in the section :ref:`Import Script`).
   
   We strongly recommend executing this script with the option ``--singleuser`` to avoid errors due to other clients querying the views that are being modified.
   
#. Or, imports a VQL script that contains the statement
   ``ENTER SINGLE USER MODE``.

When the administrator requests the Server to switch to single user
mode, the Server does the following before doing the switch:

#. Waits until all the currently running requests finish.
#. Queues all the requests received while waiting for the running
   requests to finish.
#. Once all operations finish, switches to single user mode.

When the Server is in “single user mode”, the server will only process statements executed *from the connection from which ENTER SINGLE USER MODE was executed*. For example, let us say that an administrator logs into the administration tool and opens two VQL Shells. If in the first one, she executes ENTER SINGLE USER MODE, she will only be able to execute queries on that one. If later, she executes a command from the other VQL Shell, this command will be queued until she either executes EXIT SINGLE USER MODE from the first VQL Shell or closes the first VQL Shell. This is because each VQL Shell opens its own connection to the server.

Only administrator users can make the Server switch to global single user mode.

The MBean VDBServerManagementInfo of the JMX interface provides the id
of the connection from which the command ``ENTER SINGLE USER MODE`` was
obtained. See more information about this MBean in the section
:ref:`Attributes of the VDBServerManagementInfo MBean`.

Automatic Single User Mode
==========================

This section describes how the Server automatically switches to single
user mode momentarily to execute certain actions, and exits this mode
immediately. It is called “automatic single user mode” as it does so
without user request.

The difference between automatic and explicit single user mode is that
when switching to automatic single user mode, the Server does *not* wait
for:

#. Queries to finish. I.e. ``SELECT`` and ``CALL`` statements.
#. Data Modification Language operations (DML) to finish. I.e.
   ``INSERT``, ``INSERT INTO``, ``UPDATE`` or ``DELETE``.

However, the Server does wait for Data Definition Language statements
(DDL) to finish. I.e. ``CREATE``/``ALTER DATA SOURCE``,
``CREATE``/``ALTER VIEW``, etc. The reason is to prevent executing
concurrently two operations that modify the Server’s metadata.

A Server switches to automatic single user mode even if the user
executing the action is not an administrator nor a local administrator
of the database.

Depending on the operation executed, the entire Server may go into
single user mode, or just the database where the operation is executed.

**Operations that only block the current database**:

#. A user creates/modifies/deletes an element that “belongs” to a database.
   Those are:

   a. Data sources
   b. Views: base views, derived views, interface views and materialized
      tables.
      
      If the materialized view is created with the syntax ``CREATE MATERIALIZED TABLE AS SELECT …`` or 
      ``SELECT … INTO …``, the database stays in single user mode until the query finishes. 
      If you create the materialized table with the syntax ``CREATE MATERIALIZED TABLE … ( <fields> )``,
      the statement finishes instantly.
      
      Read more about materialized views in the section :ref:`Creating Materialized Tables`
      of the VQL Guide.
      
      
   c. JMS Listeners
   d. REST Web services, SOAP Web services and widgets
   e. Stored procedures
   f. Associations


#. Modify the configuration of a database.


#. Obtain a list of elements of a certain type. I.e.: execute a ``LIST``
   statement.


#. Obtain information about an element. I.e.: execute a ``DESC`` statement.


While the Server is processing any of these operations, it queues all
the requests sent to this database until the operation finishes.
However, the Server will execute normally the requests sent to *other*
databases. In addition, these requests are usually executed instantly.

It is possible that one operation blocks more than one database. The
reason is that the Server blocks the databases of all the elements
involved in the operation. For example, if you create a view in the
database DB1 and this view is a selection of another view in the
database DB2, the ``CREATE VIEW`` statement will block the databases DB1
and DB2.

**Operations that block the entire server**


1. Creating/modifying/deleting any of the following elements:

   a. I18n maps
   b. Jars
   c. Users
   d. Roles
   e. VCS environments


#. Modifying the global VCS settings


#. Modifying the global settings of the cache.


When a user has started a transaction, these operations will be queued
until the transaction finishes.
