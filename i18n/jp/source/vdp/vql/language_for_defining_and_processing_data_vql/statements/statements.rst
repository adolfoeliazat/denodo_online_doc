==========
Statements
==========

There are two types of statements in VQL:

1. DDL (Data Definition Language) statements. They allow creating new data
   sources, wrappers, views, etc. The DDL commands are:

   -  ``CREATE``: creates or replaces new base views, views, stored
      procedures, wrappers, data sources, published web services, users,
      etc.
   -  ``DROP``: removes elements such as base views, views, stored
      procedures, wrappers, data sources, published web services, users,
      etc.
   -  ``ALTER``: modifies specific properties of views such as its
      internationalization configuration, cache, swapping configuration,
      etc. It also allows modifying database and user permissions.
   -  ``DESC``: shows the description of data types, views, stored
      procedures, maps, operators, wrappers, data sources, etc.
   -  :ref:`LIST <Listing Elements in the Catalog>`: enumerates the elements of the catalog (data
      sources, views, etc.)
   -  ``GRANT`` and ``REVOKE``: Allow to establish or revoke user
      permissions over databases, stored procedures and/or views.

2. DML (Data Manipulation Language) statements. They allow querying and
   updating data. Virtual DataPort provides the following DML statements:

   -  :ref:`SELECT <Queries: SELECT Statement>` to execute queries and define new
      views.
   -  :ref:`INSERT <INSERT Statement>`, :ref:`UPDATE <UPDATE Statement>` and :ref:`DELETE <DELETE Statement>` for inserting, updating and deleting rows from a view, respectively.
   -  ``BEGIN``, ``COMMIT``, ``ROLLBACK`` for beginning, committing and
      rolling back transactions, respectively. See more information about this in the section :ref:`Transactions in Virtual DataPort`.
   -  ``CALL`` to execute Virtual DataPort stored procedures.