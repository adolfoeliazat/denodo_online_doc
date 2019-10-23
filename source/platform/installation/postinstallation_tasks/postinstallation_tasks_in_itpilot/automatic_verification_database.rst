===============================
Automatic Verification Database
===============================

The Verification server uses a relational database to store information
about wrappers execution to allow detecting when the sources changes and
the wrappers stop working properly. ITPilot provides an embedded `Apache
Derby <http://db.apache.org/derby/>`_ database that
can be used for this purpose. If the embedded database is going to be
used, no action is required in this section.

An external JDBC database management system can also be used. In the
current ITPilot version, the supported databases are MySQL and Oracle.

ITPilot provides a script to create the table for these Database
Management Systems in the path :file:`{<DENODO_HOME>}/scripts/itpilot/sql`. If
an external database is going to be used for this purpose, it is needed
to install the database and run on it the corresponding tables creation
script.
