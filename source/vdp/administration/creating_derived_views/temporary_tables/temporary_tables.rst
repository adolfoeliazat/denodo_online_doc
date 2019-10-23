================
Temporary Tables
================

A *Temporary table* in Virtual DataPort is a special type of view that
only lives for the duration of the session of the user that creates it,
or until she deletes it. Also, the view and the data inserted in it are
only visible by the user that creates the table.

The data of these tables is stored in the database where the data is
cached, instead of in an external data source. Unlike the other types of
views, its schema and its data are completely managed from Virtual
DataPort. 

When you create a temporary table, the Server creates a table (a real table, not a temporary one) in the cache database that will hold the data you insert in it.

You can insert rows into temporary tables but cannot delete or update
them.

The section :doc:`/vdp/vql/temporary_tables/temporary_tables` of the VQL Guide explains how to
create these tables and insert data into them.

.. note:: Before creating a temporary table, enable the cache engine. To
   do this, follow the steps described in the section :ref:`Configuring the
   Cache`.

