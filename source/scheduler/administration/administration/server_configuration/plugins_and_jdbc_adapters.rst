=========================
Plugins and JDBC Adapters
=========================

Denodo Scheduler lets you manage the extensions added to Scheduler via
the “Plugins” and “Drivers” configuration areas. In the
following sections, these configuration areas are described in detail.

 

.. note:: Deleting extensions can cause parts of Scheduler that depend
   on them to cease functioning (for example, a JDBC data source that uses
   an adapter that has just been deleted).


.. note:: The maximum allowed size for a file is 100 MB.

.. _sched_plugins:

Plugins
=================================================================================

Denodo Scheduler allows the user to create their own filters (deprecated), exporters,
handlers or crawlers (deprecated) for those functionalities not supported by
the server or that are specific to a particular project.

The administration tool shows a table with the extensions registered in
Scheduler. For each extension it shows its name, the name of the
implementation class, its type (``filter``, ``exporter``, ``handler`` or
``crawler``), the name of the JAR file that it contains, and a link to
delete the extension from the system.

.. important:: Filters and crawlers are deprecated and will be removed in a later version of the Denodo Platform.

To create a new extension, certain Java interfaces need to be
implemented (according to the extension type), a configuration file
created, and everything packed together in a JAR file (see section
:ref:`Extensions (Plugins)`).

To register a new extension in Scheduler, the JAR that contains it needs
to be selected to upload it to the server. Scheduler analyzes the JAR
and, based on the metadata contained in the file ``MANIFEST.MF``,
detects the type of extension and the implementation class.


.. _sched_drivers:

Drivers
=================================================================================

The sources of JDBC data defined using the JDBC data sources use drivers
that need to be previously registered in Scheduler. In particular,
Denodo Scheduler includes preinstalled drivers for some managers (see
appendix :doc:`JDBC Drivers <../../appendix/jdbc_drivers>`).

 

It is possible to add drivers for new relational managers by specifying
the following mandatory information:

-  **Database adapter**. The adapter name will be used, together with
   the version, to identify the adapter in Scheduler.
-  **Database Version**. Version of the database that the adapter
   applies to.
-  **Class name**. The JDBC adapter’s Java class.
-  **Connection URI template**. The sample connection URI for the
   manager to use the adapter.
-  **JAR file to upload**. JAR file containing the JDBC adapter classes.

 

Once a new driver has been added, it can be deleted. However, it is not
possible to delete drivers included in the product distribution.
