==================
Data Module: Cache
==================

Virtual DataPort incorporates a cache module that stores local copies of
the data retrieved from the data sources, in a JDBC database. This may
reduce the impact of repeated queries hitting the data source and speed
up data retrieval, especially with certain type of sources.

By default, the cache module stores the cache data in the `Apache Derby
<http://db.apache.org/derby/>`_ database embedded
in Virtual DataPort. However, we strongly recommend using an external
Database Management System (DBMSs) for this task, especially in
production environments.

The section :ref:`Cache Module` explains how the cache module works and
lists the DBMSs that Virtual DataPort can use to store the cached data.
