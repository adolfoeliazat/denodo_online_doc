===================
Materialized Tables
===================

A *Materialized table* in Virtual DataPort is a special type of base
view whose data is stored in the database where the data is cached
instead of in an external data source. Unlike the other types of views,
its schema and its data are completely managed from Virtual DataPort so
its structure does not depend on any external source.

The section :doc:`Materialized Tables </vdp/vql/materialized_tables/materialized_tables>` of the VQL Guide
explains how to create these tables and insert data into them.

.. note:: Before creating a materialized table, enable the cache engine.
   To do this, follow the steps described in the section :ref:`Configuring the
   Cache`.
