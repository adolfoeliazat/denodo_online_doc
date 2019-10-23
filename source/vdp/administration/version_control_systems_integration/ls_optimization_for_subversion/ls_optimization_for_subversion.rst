==============================
LS Optimization for Subversion
==============================

This section is not relevant if you are using Git. It is only relevant for Subversion.

The uniqueness detection feature described in the previous section,
requires examining the contents of the current database in the remote
repository before performing a check-in or a check-out. To do this, the
Server has to execute the LS operation several times. If you are going
to use the VCS integration with Subversion repositories accessed through
the http or https protocols, we recommend activating the LS optimization
for Subversion because with this protocol, examining the remote
repository can be an expensive operation.

You do not need to activate this optimization when using Microsoft Team
Foundation Server (TFS) or Subversion through the ``svn`` or ``file``
protocols. In these cases, examining the contents of the remote
repository is an efficient operation.

By default, the LS optimization for Subversion is enabled when Virtual
DataPort runs on Windows, but disabled on Linux. We recommend activating
this optimization on Linux as well because the LS operation is executed
at different points by Virtual DataPort VCS commands, so the
optimization affects the overall performance of the VCS integration.

Activating the LS Optimization
==============================

To activate this optimization, follow these steps:

#. Open the “VQL Shell” (Tools menu)
#. Execute this command:

   .. code-block:: sql
   
      SET 'com.denodo.vdb.vdbinterface.server.vcs.VCSConfigurationManager.lsopt' 
          = '3'
   

3. Restart the Virtual DataPort Server.

The value of the property
``com.denodo.vdb.vdbinterface.server.vcs.VCSConfigurationManager.lsopt``
can be one of the following:

-  ``0``: The LS optimization is disabled.
-  ``1``: The LS optimization is enabled if Virtual DataPort runs on
   Windows.
-  ``2``: The LS optimization is enabled if Virtual DataPort runs on
   Linux.
-  ``3``: The LS optimization is enabled regardless of the operating
   system.

|

To make sure that this optimization has been enabled, follow these
steps:

#. Connect to a version-controlled database.
#. Open the VQL Shell and execute the following commands:

   .. code-block:: vql

      CALL LOGCONTROLLER('com.denodo.vdb.interpreter.execution.vcs.svn.SvnPlugin', 
          'DEBUG');
          
      VCSCONTENTS '/databases/<database_name>' FILES;
      
      CALL LOGCONTROLLER('com.denodo.vdb.interpreter.execution.vcs.svn.SvnPlugin',
          'ERROR');

3. Open :file:`{<DENODO_HOME>}/logs/vdp/vdp.log` and check that at the end of
   the file, there is something like:

.. code-block:: none
   
   com.denodo.vdb.interpreter.execution.vcs.svn.SvnPlugin - Executing
   <DENODO\_HOME>/dll/vdp/svn-fastls/linux/denodo-fastsvnls

4. The execution time for the operation is also logged:

.. code-block:: none

   Total execution time = <time_in_milliseconds>

Tested Environments
===================

This feature has been tested on Windows and in the following Linux
distributions:

-  Red Hat Enterprise Linux 6.5 x86\_64
-  Red Hat Enterprise Linux 6.6 x86
-  Red Hat Enterprise Linux 7 x86\_64
-  Ubuntu 13.10 x86
-  Ubuntu 13.10 x86\_64
-  Ubuntu 14.04 LTS x86
-  Ubuntu 14.04 LTS x86\_64

If your environment is different to any of these, you still can benefit
from this enhancement: you will need to build the native executable that
implements the optimization. To do this, follow the steps detailed in
:file:`{<DENODO_HOME>}/dll/vdp/svn-fastls/linux/build/README`.
