=======================
JDBC Extraction Section
=======================

The databases containing the required data will be accessed using the
JDBC standard (`Java Database Connectivity <https://www.oracle.com/technetwork/java/javase/jdbc/index.html>`_).
Appendix :doc:`JDBC Drivers </scheduler/administration/appendix/jdbc_drivers>` lists the JDBC drivers included and/or normally used
by Denodo Scheduler. It is also possible to easily incorporate other JDBC
drivers into the system (see section :doc:`Drivers <../../administration/server_configuration/plugins_and_jdbc_adapters>`).

The JDBC job extraction section is configured in the same way as the VDP
jobs (see section :ref:`VDP Extraction Section`).
The only difference is that, instead of selecting a VDP data source, you
have to select a JDBC data source, and instead of specifying a
parameterized statement in VQL language, it has to be specified in SQL.

.. important:: JDBC jobs are deprecated and will be removed in a later version of the Denodo Platform.
   
   The section :ref:`Features Deprecated in Scheduler 7.0` lists all the features that are deprecated.