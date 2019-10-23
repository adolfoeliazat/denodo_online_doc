========
Teradata
========

Teradata does not require special configuration.

At runtime, when Virtual DataPort uses the Teradata APIs to do a bulk
load, it opens a new connection with the parameter ``TYPE=FASTLOAD``. This
connection cannot be obtained from the pool of connections of the data
source so the execution engine creates a new one to do the bulk data load.
