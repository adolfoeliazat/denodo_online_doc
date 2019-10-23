========================================
Data Movements From/To Netezza Databases
========================================

Virtual DataPort benefits from the Netezza’s *external tables* feature
to decrease the time it takes to perform a data movement when the target
of the movement is a Netezza database.

If both the source and the target of a data movement are Netezza
databases, the data will be transferred directly from one database to
the other without going through Virtual DataPort. Additionally, when
Virtual DataPort and Netezza run on UNIX/Linux operating systems, the
Server takes advantage of the inter-process communication (IPC) to
insert data into Netezza.

To take advantage of this feature, the options “Use external table on
data movement” have to be enabled. To do this, in the JDBC data source
dialog, click the **Read & Write** tab, then, select *both* **Use
external tables for data movement** check boxes (“Read settings” and
“Write settings”) and store the changes.

The option “Use external tables for…” of the “Write settings” enables
the capability of inserting data into a Netezza database using external
tables.

The option “Use external tables for…” of “Read settings” enables the
capability of reading data from a Netezza database and transfer it into
another Netezza database whose data source also has the option “Use
external tables for…” enabled. When the target of a data movement is not
another Netezza database, it does not matter if this option is selected
or not in the data source.

If the target of the data movement is the cache data source (data
sources ``vdpcachedatasource`` or ``customvdpcachedatasource``), select
the check box **Use external tables to store data** in the cache’s
configuration.

If the source of the data movement is the cache data source (data
sources ``vdpcachedatasource`` or ``customvdpcachedatasource``), select
the check box **Use external tables to retrieve data for data
movements** in the cache’s configuration.
