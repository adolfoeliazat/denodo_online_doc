================
Temporary Tables
================


.. toctree::
   :hidden:

   creating_temporary_tables/creating_temporary_tables.rst

A *Temporary table* in Virtual DataPort is a special type of view that
only lives for the duration of the session of the user that creates it,
or until she deletes it. The view and the data inserted in it are only
visible by the user that creates the table.

The data of these tables is stored in the database where the data is
cached, instead of in an external data source. Unlike the other types of
views, its schema and its data are completely managed from Virtual
DataPort.

When you create a temporary table, the Server creates a table (a real table, not a temporary one) in the cache database that will hold the data you insert in it.

You can insert rows into temporary tables but cannot delete or update
them.

.. note:: Before creating any temporary table, enable the cache engine.
   To do this, follow the steps described in the section :ref:`Configuring the
   Cache` of the Administration Guide.

The differences between temporary tables and materialized tables are the
following:

.. table:: Differences between temporary tables and materialized tables
   :name: Differences between temporary tables and materialized tables

   +--------------------------------------+--------------------------------------+
   | Temporary Tables                     | Materialized Tables                  |
   +======================================+======================================+
   | Only visible by the user that        | Visible to all the users with the    |
   | creates the table.                   | appropriate privileges.              |
   +--------------------------------------+--------------------------------------+
   | Automatically deleted once the user  | They are persisted across session.   |
   | closes the session to the database.  |                                      |
   | I.e. when creating the table from    |                                      |
   | the “VQL Shell” of the               |                                      |
   | Administration Tool, until the user  |                                      |
   | disconnects or switches to another   |                                      |
   | database.                            |                                      |
   +--------------------------------------+--------------------------------------+
   | Two users may create a temporary     | There cannot be two materialized     |
   | table each, both with the same name. | tables with the same name.           |
   | Each user will only have access to   |                                      |
   | her own.                             |                                      |
   +--------------------------------------+--------------------------------------+
   | They cannot be created nor managed   | They cannot be graphically created   |
   | graphically from the Administration  | from the Administration Tool (only   |
   | Tool. They can be created only from  | from the “VQL Shell”), but after     |
   | the “VQL Shell”.                     | refreshing the Administration Tool   |
   |                                      | (clicking “Refresh” on the “File”    |
   |                                      | menu), they can be managed           |
   |                                      | graphically.                         |
   +--------------------------------------+--------------------------------------+
   | You cannot create derived views over | You can create derived views over    |
   | temporary tables.                    | materialized tables.                 |
   +--------------------------------------+--------------------------------------+

After 48 hours of creating a temporary table, Virtual DataPort removes
the table from the database. It does so to prevent that the data of a 
table is kept in the database indefinitely. This scenario may
occur if there is an error while a user closes her session and she has
created a table.
