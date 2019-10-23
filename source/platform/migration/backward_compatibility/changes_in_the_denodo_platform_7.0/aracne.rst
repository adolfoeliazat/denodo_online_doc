================================
Changes Introduced in Aracne 7.0
================================

The Aracne Module Has Been Deprecated
============================================================================

The Aracne module has been deprecated and may be removed in the next version of the Denodo Platform.

The Aracne module no longer appears in the Control Center.

.. rubric:: Changes to Aracne Index/Search Engine Server

The component *Aracne Index* is now a component of Scheduler and has been renamed *Scheduler Index Server*.

In the Control Center, it is listed in the *Scheduler* module.

|

.. rubric:: Changes to Aracne Crawling Server and the Aracne Administration Tool

The components *Aracne Crawling Server* and its administration tool are now automatically installed when installing Denodo Scheduler.

They cannot be started from the Control Center. To start them use these scripts located on :file:`{<DENODO_HOME>}/bin`:

-  ``arn_startup.sh`` to start the Aracne Server (``arn_startup.bat`` in Windows).
-  ``arn_shutdown.sh`` to stop the Aracne Server (``arn_shutdown.bat`` in Windows).
-  ``arn_webadmin_startup.sh`` to start the Aracne Web Administration Tool (``arn_webadmin_startup.bat`` in Windows).
-  ``arn_webadmin_shutdown.sh`` to stop the Aracne Web Administration Tool (``arn_webadmin_shutdown.bat`` in Windows).

The default ports of the Aracne Crawler Server and the Aracne Administration Tool have changed:

-  Aracne Crawler Server (localhost:11000). To modify them, edit the 
   file :file:`{<DENODO_HOME>}/conf/arn/ConfigurationParameters.properties`.
-  Aracne Administration Tool (http://localhost:9090/webadmin/denodo-aracne-admin)

This change affected other modules of the Denodo Platform:

-  *Denodo Virtual DataPort*: Aracne wrappers and sources created with previous versions of the Platform
   still work, but they are deprecated. This means no new elements related to Aracne may be created.

-  *Denodo Scheduler*: Aracne data sources and jobs created with previous versions 
   still work, but they are deprecated. This means no new elements related to Aracne 
   (Aracne and ARN-Index jobs, Aracne data sources and filter sequences) can be created. 
   They can only be modified and used in already existing jobs from previous versions.