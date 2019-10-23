=========
Execution
=========
   
Once the installation process has terminated, the servers are ready to
run. Each server found on the same machine as the administration tool
can be started up directly from the Web tool itself, as dealt with in
section :ref:`Adding, editing and deleting servers`. If this is not the
case, they have to be started up in the machines in which they reside.



Starting Up the Administration Tool
===================================

The administration tool can be started from the Denodo Platform Control
Center (see the :doc:`Installation Guide <../../../platform/installation/index>`), or by
using the following scripts available in the ``DENODO_HOME/bin``:

-  ``itpilot_webadmin_startup``: starts up the administration tool.
-  ``itpilot_webadmin_shutdown``: stops the administration tool.



Once the application has been properly displayed, the administration
tool will be available at http://localhost:9090/webadmin/denodo-itpilot-admin.

Starting Up the Browser Pool
============================

The Browser Pool can be started from the Denodo Platform Control Center
or by using the
following scripts available in the path ``DENODO_HOME/bin``:

-  ``browserpool_startup``: starts up the Browser Pool.
-  ``browserpool_shutdown``: stops the remote pool and all the browsers
   contained in it.

Starting Up the Wrapper Server
==============================

The Wrapper Server can be started from the Denodo Platform Control
Center or using the following scripts available in the path ``DENODO_HOME/bin``:

-  ``vqlserver_startup``: starts up the Wrapper Server.
-  ``vqlserver_shutdown``: stops the Wrapper Server.
-  ``vqlserver``: with the options ``startup`` and ``shutdown``.

Starting Up the Verification Server
===================================

The Verification Server can be started from the Denodo Platform Control
Center or
using the following scripts available in the path ``DENODO_HOME/bin``:

-  ``verification_startup``: starts up the Verification Server.
-  ``verification_shutdown``: stops the Verification Server.

Starting Up the Verification Server with a Graphical Tool
=========================================================

The verification server can be started together with a graphical tool,
described in detail in section :ref:`Verification Server Graphical
Configuration and Monitoring tool`, by using the
``verificationGUI_startup`` scripts available in the path
:file:`{<DENODO_HOME>}/bin`.

Starting Up the PDF Conversion Server
=====================================

The PDF conversion server can be started from the Denodo Platform
Control Center
or using the executable ``PdfConversionsServer.exe`` which resides in
the ``DENODO_HOME/bin`` directory, and that allows the server to be
started up and stopped, with the following format:

.. code-block:: none

   PDFConversionsServer ([-start \| -shutdown] [-conf='confFile'])

|

-  ``-start`` means that the conversion server must be started

-  ``-stop`` means that it must be stopped

-  ``confFile``, as the server
   configuration file. By default, it can be found at
   ``DENODO_HOME/conf/iebrowser``, with the name
   ``IEBrowserConfiguration.properties``.
