====================================================================================
Enabling the Support for ODBC Sources When the Virtual DataPort Server Runs on Linux
====================================================================================

Virtual DataPort provides access to ODBC data sources. However, this
feature is disabled by default when the Virtual DataPort server runs on
Linux. The reason for being disabled by default is that on Linux,
depending on the configuration of the host, Virtual DataPort could load
the wrong library to connect to ODBC sources. If the wrong library is
loaded, Virtual DataPort crashes. To avoid that a user creates an ODBC
source that leads the Virtual DataPort server to crash, ODBC sources are
disabled by default.

Follow these steps to enable this feature:


#. Install the package unixODBC. The section :doc:`Install unixODBC <../../../../vdp/developer/access_through_odbc/configuration_of_the_odbc_driver_in_linux_and_other_unix/set_up_a_dsn_on_linux_and_other_unix>` of the
   Developer Guide explains how to do it.

#. Log into the user account you will use to launch the Virtual DataPort
   server.

#. Check that these files exist:


   ::
   
      /usr/local/lib64/libodbc.so
      /usr/local/lib64/libodbcinst.so

   Or,
   
   ::
   
      /usr/local/lib/libodbc.so
      /usr/local/lib/libodbcinst.so

   Their location may change depending on the Linux/Unix distribution.

#. Edit the file ``~/.bash_profile`` and add the following at the end.


.. code-block:: bash

   export LD_PRELOAD=/usr/local/lib/libodbc.so:/usr/local/lib/libodbcinst.so:$LD_PRELOAD

.. _enabling_the_support_for_odbc_sources_when_the_virtual_dataport_server_runs_on_linux_separator1:

   With this change in the value of the variable ``LD_PRELOAD``, you make
   sure that Virtual DataPort loads the files ``libodbc.so`` and
   ``libodbcinst.so`` provided by unixODBC and not the ones provided by
   other libraries.
   
   .. note:: If the two files listed above are in ``lib64`` and not in
      ``lib``, change the line above accordingly.

5. Logout and login again from this user account. Do this to apply the
   changes done in ``.bash_profile``.

#. If the Virtual DataPort server was started, stop it.

#. Start the Virtual DataPort server and login with an administrator
   account. Then, execute this command on the VQL Shell:

.. code-block:: batch

   SET 'com.denodo.vdb.ODBCDataSource.enable' = 'true';

.. enabling_the_support_for_odbc_sources-separator1

   This command enables the support for ODBC sources on Linux.

8. To check that the configuration has been updated correctly, do the
   following from the administration tool:

   a. Create an ODBC data source.
   #. Create an ODBC base view.
   #. Query this base view.

