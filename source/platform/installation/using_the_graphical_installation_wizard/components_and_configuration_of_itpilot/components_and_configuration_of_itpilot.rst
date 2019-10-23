=======================================
Components and Configuration of ITPilot
=======================================

You can install the following ITPilot modules:

-  *Navigation Sequence Generator*. Use it to graphically generate
   automated Web browsing sequences on Web sources. It is installed as a
   toolbar in the Microsoft Internet Explorer browser.
-  *Wrapper Generator Tool*. Used to create wrappers on Web sources.
   See the ITPilot Generation Environment Guide.
-  *Browser Pool*. Wrapper execution environment component required to run automated
   browsing sequences based on Microsoft Internet Explorer, and that can
   also use the Denodo browser.
-  *Wrapper Server*. Used to run the wrappers created with the previous
   components.
-  *Verification Server*. Used to automatically verify that ITPilot
   wrappers continue running properly after there were changes in the
   Web sources.
-  *Administration Tool*. Web administration console for executing
   wrappers and configuring the Browser Pool, the wrapper server and the
   Verification Server.
-  *Wrapper Client Environment*. API for developing applications that
   use ITPilot wrappers.

If you selected *Custom Installation* in the step 2 of the installation
wizard, you can configure the following settings of the ITPilot modules:

-  Browser Pool: see section :ref:`Initial Configuration of the Browser
   Pool`.
-  Wrapper Server: see section :ref:`Wrapper Server`.
-  Verification Server: see section :ref:`Verification Server`.

Initial Configuration of the Browser Pool
=========================================

When you select *Custom Installation* in the step 2 of the installation
wizard, you can configure the following settings of the ITPilot Browser
Pool:

-  **Server port number**: port that the Browser Pool will listen for
   incoming connections.
-  **Shutdown port number**: port that the Browser Pool will listen for
   shutdown requests.
-  **Auxiliary port number**: auxiliary port used by the Browser Pool to
   communicate with its clients.
-  **Initial MSIE browser port**: port used by ITPilot to communicate with
   the first opened Internet Explorer browser. Consecutive ascending
   port numbers will be used when additional browsers are requested.
-  In Windows operating systems, select **Install as a Windows service**
   to install the Browser Pool as a Windows service.

In the next wizard, configure the path to several application that may
be required by ITPilot:

-  **Acrobat Professional installation directory**: path where Acrobat
   Professional is installed. You need this if you are going to extract
   data from PDF documents using the Adobe Professional software.
-  **Open Office installation directory**: path where OpenOffice is
   installed. You need this if you are going to extract data from
   Microsoft Word or Microsoft Excel documents.

Wrapper Server
==============

When you select *Custom Installation* in the step 2 of the installation
wizard and you do not install the Virtual DataPort server, you can
configure the following settings for the ITPilot Wrapper Server:

-  **Server port number**: port that the ITPilot Wrapper server will
   listen for incoming requests.
-  **Shutdown port number**: port that the ITPilot Wrapper server will
   listen for shutdown requests.
-  **Auxiliary port Number**: auxiliary port used by the ITPilot Wrapper
   server to communicate with its clients.
-  In Windows operating systems, select **Install as a Windows service**
   to install the ITPilot Wrapper server as a Windows service.

If you install the Browser Pool separately from the Wrapper Server,
configure the following settings in the Wrapper Server:

-  Browser Pool IP address.
-  Browser Pool listening port.

In the event of also installing a browser pool, the values indicated for
the pool during installation will be used as values for these
parameters.

Verification Server
===================

When you select *Custom Installation* in the step 2 of the installation
wizard, you can configure the following settings of the ITPilot
Verification server:

-  **Server port number**: port that the ITPilot Verification server
   will listen for incoming requests.
-  **Shutdown port number**: port that the ITPilot Verification server
   will listen for shutdown requests.
-  **Auxiliary port number**: auxiliary port used by the Verification
   server to communicate with its clients.
-  In Windows operating systems, select **Install as a Windows service**
   to install the ITPilot Verification server as a Windows service.

Furthermore, if a wrapper server is not being installed in the same
installation, the wrapper server connection data must be configured:

-  Wrapper server IP address.
-  Wrapper server listening port.

In the event of also installing a wrapper server, the values indicated
for this server during installation will be used as values for these
parameters.
