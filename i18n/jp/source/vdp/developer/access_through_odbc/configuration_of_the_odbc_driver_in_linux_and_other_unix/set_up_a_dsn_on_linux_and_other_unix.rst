=======================================
Install the Denodo ODBC Driver on Linux
=======================================

Denodo provides the Denodo ODBC driver compiled for Linux, compatible with the driver managers unixODBC and iODBC. To use this driver with other driver managers or on Unix, see the section :ref:`Compile the Denodo ODBC Driver`.

You have to install the Denodo ODBC driver *in the machine where the client application runs*. To do this, follow these steps:

1. Obtain the appropriate ODBC driver
#. Install unixODBC
#. Register the ODBC driver with unixODBC
#. Register a data source (DSN) that points to Denodo

Obtain the Appropriate ODBC Driver
==================================

There are several flavors of the Denodo ODBC driver. This section explains which one you have to select:

#. Obtain the package ``denodo-vdp-odbcdriver-linux.zip``. To do this:

   a. Copy it from the installation 
      (:file:`{<DENODO_HOME>}/tools/client-drivers/odbc/denodo-vdp-odbcdriver-linux.zip`).

   b. Or download it from the `ODBC page <https://community.denodo.com/drivers/odbc/32>`_ of the Denodo Community. 

      On this page, download the driver for Linux (the name ends up with *-linux*). Make sure you select a version of the package
      that is *not newer* than the Denodo server you are going to connect. For example, if your Denodo server has the update 7.0 20181011, *do not* download
      the package *denodo-vdp-odbcdriver-7.0-update-20190312-linux* because it is newer.
      
      The section :ref:`Access Through ODBC` explains the policy regarding backward compatibility of this driver.

#. Extract the contents of the file ``denodo-vdp-odbcdriver-linux.tar.gz``:

    .. code-block:: bash
   
      tar -xzf denodo-vdp-odbcdriver-linux.tar.gz

#. Choose the appropriate flavor of the driver and copy all its files to the host where the client application runs. The options are:

   -  ``unixodbc_x86``: ODBC driver for 32-bit clients and the unixODBC driver manager.
   -  ``unixodbc_x64``: ODBC driver for 64-bit clients and the unixODBC driver manager.
   -  ``iodbc_x86``: ODBC driver for 32-bit clients and the iODBC driver manager.
   -  ``iodbc_x64``: ODBC driver for 64-bit clients and the iODBC driver manager.

   For example, to connect from a 32-bit application using the unixODBC driver manager, copy the folder ``unixodbc_x86`` to the host where the client application runs.

   Denodo also provides the ANSI version of each driver. These are the files ending with "a" (e.g. ``unixodbc_x86/denodoodbca.so``). Only use the ANSI version when the Unicode encoding is not valid for your environment.

Install UnixODBC
================

Linux does not provide an ODBC driver manager by default; you have to install it. This section explains how to install and configure `unixODBC <http://www.unixodbc.org/>`_. Denodo also provides the ODBC driver compiled to be used with iODBC.

For Linux distributions based on RPM packaging system such as CentOS,
execute this:

   .. code-block:: bash

        yum install unixODBC

For the ones based on the Debian packaging system like Ubuntu execute this:

   .. code-block:: bash

        apt-get install unixODBC

Register the Denodo ODBC Driver in UnixODBC
===========================================

After installing unixODBC, register the ODBC driver. Follow these steps:

1. Create a new file ``denodoODBCDriver.template`` with the content
   below:

   .. code-block:: none
      :linenos:
      :emphasize-lines: 3

      [DenodoODBCDriver]
      Description=ODBC driver for Denodo 7.0
      Driver=/opt/denodo-odbc-driver/unixodbc_x86/denodoodbc.so
      UsageCount=1
      
   Modify line #3 to adjust the path to point to the flavor of the ODBC driver you selected.

2. Execute the following command to register the Denodo driver in the
   ODBC Driver Manager:

   .. code-block:: bash

      sudo odbcinst -install -driver -file denodoODBCDriver.template

To list the ODBC drivers registered in the driver manager, execute this:

.. code-block:: bash

   sudo odbcinst -query -driver

The result should list the new driver: ``DenodoODBCDriver``.

To uninstall the driver, execute:

.. code-block:: bash

   sudo odbcinst -uninstall -driver -name DenodoODBCDriver

Check the documentation of the iODBC driver manager if you want to use that instead.

Register a Data Source (DSN) on UnixODBC
=========================================

This section explains how to register a DSN in unixODBC. For registering a DSN with iODBC, check its documentation.

#. Create a file called ``denodoDSN.template`` with the content below:

.. _PostgreSQL ODBC driver: default configuration for Linux:

   .. code-block:: none
      :linenos:

      [Denodo_DSN]
      Description = Denodo connection
      Driver = DenodoODBCDriver
      Servername = <host name>
      Port = <Virtual DataPort ODBC port. Default is 9996>
      UserName = <Virtual DataPort user name>
      Password = <Password>
      Database = <Virtual DataPort database>
      Application_name = <name of the application that will use the DSN>
      Protocol = 7.4
      BoolsAsChar = 0
      ByteaAsLongVarBinary= 1
      ConnSettings = SET QUERYTIMEOUT TO 3600000; SET I18N TO us_pst; /*krbsrvname=HTTP*/
      Debug = 0
      DebugFile = ~/unixODBC/log/debug.log
      FakeOidIndex = 0
      Fetch = 1000
      Ksqo = 0
      LFConversion = 1
      Optimizer = 0
      ReadOnly = 0
      RowVersioning = 0
      ServerType = Postgres
      ShowOidColumn = 0
      ShowSystemTables = 0
      # Uncomment the "Sslmode" property if SSL is enabled in the
      # Virtual DataPort Server
      # Sslmode = require
      Trace = 0
      TraceFile = ~/unixODBC/log/trace.log
      UniqueIndex = 1
      UpdatableCursors = 0
      UseDeclareFetch = 1
      UseServerSidePrepare= 0

   In line #8 (``Database``), if the name of the database contains non-ASCII characters, they have to
   be URL-encoded. For example, if the name of the database is “テスト”,
   set the property to ``%E3%83%86%E3%82%B9%E3%83%88``.

   In line #33 (``UseDeclareFetch``), if the value is ``1``, the DSN will use DECLARE
   CURSOR/FETCH to handle SELECT statements. The effect is that the DSN
   will retrieve the rows of the result set in blocks, instead of
   retrieving them all at once. The ``Fetch`` property establishes the
   number of rows of each block. This property is equivalent to the “Fetch
   size” of the JDBC connections.

   In line #14 (``Debug``), if you set the property to ``1``, the driver logs all the
   requests received by this DSN to the file set in the property
   ``DebugFile``. On a production environment, we strongly recommend setting the value
   of this property to ``0`` because logging all the requests impacts
   the performance of the driver and the log file may grow to a very
   large size.

   In line #13 (``ConnSettings``), you can set the properties of the
   connection established with Virtual DataPort, by adding the following
   statements:

   a. ``SET QUERYTIMEOUT TO <value>`` to change the query time out (value
      in milliseconds).
   b. ``SET i18n TO <i18n>`` to change the i18n of the connection.

   For example, to set the default timeout of the queries to one hour, set
   the value of the property ``ConnSettings`` to the following:

   .. code-block:: sql
   
      ConnSettings=SET QUERYTIMEOUT TO 3600000; SET I18N TO us_pst

   Note the ``;`` between each statement.

   Read :ref:`developer_guide-configuration_of_the_odbc_driver_on_windows_parameter_of_the_ODBC_driver` to learn
   how these properties work, and their default value.

   If you have enabled SSL in the Virtual DataPort server to secure the
   communications, add the following property to this configuration file:

   .. code-block:: sql
    
      Sslmode=require

   c. Add the following ``ConnSettings`` property to connect to Virtual
      DataPort using Kerberos authentication:

      .. code-block:: sql
        
         /*krbsrvname=HTTP*/

      .. important:: This line has to be the last thing on the
         ``ConnSettings`` property.

      If Kerberos authentication is enabled on the Denodo database you are connecting to, the driver will ignore the value of the properties "UserName" and "Password". Instead, it will obtain a Kerberos ticket from the system cache.
      

      To be able to use Kerberos authentication, the configuration of the DSN
      has to meet these conditions:

         1. The database that the DSN will connect to is configured with the
            option “ODBC/ADDO.net authentication type” set to “Kerberos”.
         #. The client has to belong to the Windows domain. The reason is that
            the ODBC driver uses the ticket cache of the operating system to
            obtain “ticket-granting ticket” (TGT).
         #. In the property ``Servername``, enter the fully qualified domain name of the Denodo server. That is, 
            if in the Kerberos configuration of the Denodo server the field *Server principal* is 
            ``HTTP/denodo-prod.subnet1.contoso.com@CONTOSO.COM``, enter ``denodo-prod.subnet1.contoso.com``.

   d. The value of the property ``application_name`` should be the name of the
      application that will use this DSN. We recommend setting this attribute
      in all the connections to Virtual DataPort because is very useful for
      logging purposes and debugging problems caused by a particular client.

2. Execute this to register the new DSN:

.. code-block:: bash

   odbcinst -install -s -l -f denodoDSN.template


..

   The parameter ``-l`` registers the DSN as a “system DSN”. “System DSNs”
   are available to all the users.

   If you do not have enough privileges to register a “system DSN”, replace
   ``-l`` with ``-h`` to register the DSN as a “user DSN” instead. If you
   do this, execute this command with the same user name that you execute
   the client application that needs to access to this DSN. The reason is
   that “user DSNs” are only available to the user that registers them.

   To list the DSNs registered in the ODBC driver manager, execute this:

.. code-block:: bash

   odbcinst -query -s
..

   The result should list the new DSN: ``denodo_acme_DSN``.
   


   To delete a DSN from the driver manager, execute this:

.. code-block:: bash

   odbcinst -uninstall -s -name denodo_acme_DSN
..
   
   After setting up the DSN, we recommend reading the section :ref:`Integration
   with Third-Party Applications`.


Compiling UnixODBC
==================

If you cannot install unixODBC using the package manager of your operating system, download it and
compile it. To do this, follow these steps:

#. Download the latest version of the source code from
   http://www.unixodbc.org/download.html.

#. Execute the following commands to extract the source code and compile
   it:

   .. code-block:: bash

        tar -zxf unixODBC*.tar.gz
        cd unixODBC
        ./configure.sh
        make

3. Execute the following command:

   .. code-block:: bash
        
        sudo make install