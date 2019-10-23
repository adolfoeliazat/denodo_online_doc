===================
Materialized Tables
===================


.. toctree::
   :hidden:

   creating_materialized_tables/creating_materialized_tables.rst
   inserting_data_into_materialized_tables/inserting_data_into_materialized_tables.rst

A *Materialized table* in Virtual DataPort is a special type of base
view whose data is stored in the database where the data is cached,
instead of in an external data source. Unlike the other types of views,
its schema and its data are completely managed from Virtual DataPort.

The following sections explain how to create and delete materialized
tables and how to insert data into them. You can insert rows into a
materialized table but cannot delete or update them.

.. note:: Before creating any materialized table, enable the cache
   engine. To do this, follow the steps described in the section
   :ref:`Configuring the Cache` of the Administration Guide.
