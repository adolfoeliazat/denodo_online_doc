=============================
Databases in Virtual DataPort
=============================

A Virtual DataPort server can contain several *databases* (do not
confuse with the possible external databases that can act as data
sources of the system). A Virtual DataPort *database* represents a
virtual schema comprised of a series of data sources, views, stored
procedures, widgets, etc.

Each database is independent of the other databases of the server and,
as described in detail in the next section, different users can have
different privileges for each database.

When a Virtual DataPort server is installed, an initial database called
``admin`` is created.
