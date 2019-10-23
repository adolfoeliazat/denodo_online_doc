================================
Compile the Denodo ODBC Driver
================================

Denodo provides the Denodo ODBC driver compiled for Linux, compatible with the driver managers unixODBC and iODBC. We recommend using the compiled driver. However, if you need to use another driver manager or use the driver on Unix, you need to compile it.

This section explains how to compile the Denodo ODBC driver. After compiling the driver, see the section :ref:`Install the Denodo ODBC Driver on Linux`.

Pre-requisites to Compile the ODBC Driver on Linux
==================================================

The Denodo ODBC driver depends on the following packages and versions:

1. ``gcc``
#. ``unixODBC`` version >= 2.2.14
#. ``unixODBC-devel`` version >= 2.2.14 (in some Linux distributions, it is called "unixodbc-dev").
#. ``openssl-devel`` (in some Linux distributions, it is called "libssl-dev"): you only need this package if you want the ODBC driver to support Kerberos authentication.
#. ``postgresql-devel`` version >= 9.x (in some Linux distributions, it is called "postgresql-server-dev-9.5").

.. note:: If you need to connect to a Denodo server with Kerberos authentication *and* at least one of the JDBC data sources in Denodo uses pass-through session credentials, **do not install the package** ``postgresql-devel``. It will be manually built later.

Before compiling the ODBC driver, check that these packages are installed in the host where you are compiling the driver:

.. rubric:: For Linux distributions based on RPM packaging system such as CentOS:

-  Check that **all the packages** listed above are installed. To do this, execute ``yum list installed <package name>``. For example,

   .. code-block:: bash
   
      yum list installed gcc postgresql-devel unixODBC unixODBC-devel openssl-devel
         
   You should see something like
   
   .. code-block:: none
      :emphasize-lines: 2-6

      Installed Packages
      gcc.x86_64                                                     4.8.2-16.2.el7_0
      openssl-devel.x86_64                                            1:1.0.1e-60.el7
      postgresql-devel.x86_64                                            9.2.18-1.el7
      unixODBC.x86_64                                                    2.3.1-11.el7
      unixODBC-devel.x86_64                                              2.3.1-11.el7

   Check that the *five* packages are installed *and* that the version of the package meets the requirements. If a package is not listed, it means that you have to install it.
   
-  To install a package, execute the command ``yum install <package name>``. For example, 

   .. code-block:: bash
   
      sudo yum install openssl-devel
      
   If this returns "No package ... available.", it means that in this particular distribution, the package is named differently. In this case, search the name of the package with the command ``yum list available <package name>``. For example, 
   
   .. code-block:: bash
   
      yum list available *openssl-devel*
   
   Note the parameter includes an asterisk. That is because you can pass a pattern and not just an exact name.
   
   At least one package should come up. Copy the name and install it with ``yum install <package name>``.

|

.. rubric:: For Linux distributions based on the Debian packaging system like Ubuntu:

-  Check that **all the packages** listed above are installed. To do this, execute ``apt list --installed <package name>``. For example,

   .. code-block:: bash
   
      apt list --installed gcc postgresql-server-dev-9.5 unixODBC unixodbc-dev libssl-dev

   You should see something like
   
   .. code-block:: none
      :emphasize-lines: 2-6
   
      Listing... Done
      gcc/trusty,now 4:4.8.2-1ubuntu6 amd64 [installed]
      libssl-dev/trusty-updates,trusty-security,now 1.0.1f-1ubuntu2.22 amd64 [installed]
      postgresql-server-dev-9.5/trusty-updates,now 9.5.14-0ubuntu0.14.04 amd64 [installed]
      unixodbc/trusty,now 2.2.14p2-5ubuntu5 amd64 [installed]
      unixodbc-dev/trusty,now 2.2.14p2-5ubuntu5 amd64 [installed]

   Check that the *five* packages are installed *and* that the version of the package meets the requirements. If a package is not listed, it means that you have to install it.

   Note that the package names are different than the ones listed when talking about RPM-based distributions. The reason is that in many Debian-based distributions, the package names are different. 

-  If a package is *not* installed, install it with the command ``apt-get install <package name>``. For example, 

   .. code-block:: bash
   
      sudo apt-get install unixODBC
      
   If this returns "Unable to locate package <package name>", it means that in this particular distribution, the package is named differently. In this case, search the name of the package with the command ``apt list <package name>``. For example,

   .. code-block:: bash
   
      apt list *unixODBC*
   
   Note the parameter includes an asterisk. That is because you can pass a pattern and not just an exact name. 

   At least one package should come up. Copy the name and install it with ``apt-get install <package name>``.

.. note:: If you do not have privileges to execute ``sudo``, type ``su``, press Enter and provide the "root" password. You will be the root user for the duration of the session.

Compile the ODBC Driver (Standard Method)
=========================================

This section explains how to compile the Denodo ODBC driver. Open a command line and execute the commands below. Notice that the Denodo ODBC driver should be **compiled in the client machine** where it is going to be used.

.. note:: If you need to connect to a Denodo server with Kerberos authentication *and* at least one of the JDBC data sources in Denodo uses pass-through session credentials, do **not** follow the instructions of this section. Instead, go to the section :ref:`Compile the ODBC Driver to Obtain Forwardable Tickets`.
 
.. code-block:: bash
   :emphasize-lines: 1-4

   # Important: in the line below, replace "<DENODO_ODBC_HOME>" with the path to the 
   # directory that contains the denodo-vdp-odbcdriver-linux.tar.gz file.
   # This file can be obtained from <DENODO_HOME>/tools/client-drivers/odbc in
   # the machine where the Denodo server is installed.

   export DENODO_ODBC_HOME=<DENODO_ODBC_HOME>

   cd $DENODO_ODBC_HOME
   
   # Building the denodoODBC driver.
   tar -xzf denodo-vdp-odbcdriver-linux.tar.gz
   cd denodo-vdp-odbcdriver-linux/src/denodo-pgsqlodbc
   ./configure --prefix=$DENODO_ODBC_HOME/dist
   make install

The driver is now compiled. It is located in "<DENODO_ODBC_HOME>/dist/lib".

Continue to the section :ref:`Install the Denodo ODBC Driver on Linux`.
       
If ``./configure`` or ``make`` failed because of missing dependencies, go to the section `Troubleshooting the Compilation of the Denodo ODBC Driver`_. For example, if ``./configure`` failed with this error:
   
.. code-block:: none

   configure: error: odbc_config not found (required for unixODBC build)


Compile the ODBC Driver to Obtain Forwardable Tickets
=====================================================

This section explains how to compile the Denodo ODBC driver so it can obtain "forwardable" Kerberos tickets. This is necessary if you need to use Kerberos authentication and at least one of the JDBC data sources in Denodo uses pass-through session credentials. You will be able to use this driver for login/password authentication as well. If you do not need this feature, follow the steps of the section :ref:`above <Compile the ODBC Driver (Standard Method)>`.

.. note:: During the following process, the system needs to have the ``patch`` command available.

Connect to the client machine where the driver is going to be used, open a command line and execute these commands:

.. code-block:: bash
   :emphasize-lines: 1-4

   # Important: in the line below, replace "<DENODO_ODBC_HOME>" with the path to the 
   # directory that contains the denodo-vdp-odbcdriver-linux.tar.gz file.
   # This file can be obtained from <DENODO_HOME>/tools/client-drivers/odbc in
   # the machine where the Denodo server is installed.
   
   export DENODO_ODBC_HOME=<DENODO_ODBC_HOME>
      
   cd $DENODO_ODBC_HOME   
   tar -xzf denodo-vdp-odbcdriver-linux.tar.gz
   cd denodo-vdp-odbcdriver-linux/src
   
   # Downloading the source code of PostgreSQL
   wget https://ftp.postgresql.org/pub/source/v9.5.14/postgresql-9.5.14.tar.gz   
   tar -xzf postgresql-9.5.14.tar.gz
   
   # The file "denodo-vdp-odbcdriver-linux.tar.gz" includes a patch that modifies the
   # libpq library of PostgreSQL so the Kerberos authentication works with Denodo.
   cd postgresql-9.5.14
   patch ./src/interfaces/libpq/fe-auth.c ./libpq.patch
   
   # Building libpq library included with PostgreSQL.
   ./configure --with-krb-srvnam=HTTP --with-openssl --without-readline --prefix=$DENODO_ODBC_HOME/dist/postgresql
   make install
   
   # Building the denodoODBC driver.
   cd $DENODO_ODBC_HOME/denodo-vdp-odbcdriver-linux/src/denodo-pgsqlodbc
   ./configure --with-libpq=$DENODO_ODBC_HOME/dist/postgresql/bin/pg_config --prefix=$DENODO_ODBC_HOME/dist
   make install
      
   # Moving libpq next to the denodoODBC driver.
   cd $DENODO_ODBC_HOME/dist
   cp postgresql/lib/libpq.so.5 lib/
 
The driver is now compiled. It is located in "<DENODO_ODBC_HOME>/dist/lib".
The recompiled library libpq is located in the same directory.

.. note:: For the system to be able to find the dependent library libpq.so when loading the Denodo ODBC driver, you may need to add the path to the recompiled libpq.so.5 to the environment variables ``LD_LIBRARY_PATH`` or ``LIBPATH``

Continue to the section :ref:`Install the Denodo ODBC Driver on Linux`.

If ``./configure`` or ``make`` failed because of missing dependencies, go to the section :ref:`below <Troubleshooting the Compilation of the Denodo ODBC Driver>`. For example, if ``./configure`` failed with this error:
   
.. code-block:: none

   configure: error: odbc_config not found (required for unixODBC build)


Troubleshooting the Compilation of the Denodo ODBC Driver
=========================================================

If you find any problems during the compilation of the Denodo ODBC driver, try the following:

1. Check that the `packages required by the Denodo ODBC driver are installed <Pre-requisites to Compile the ODBC Driver on Linux_>`_, including its version.

   
#. If they are correct, try compiling manually the packages UnixODBC and PostgreSQL-devel. To do this, execute the following commands:

.. rubric:: Compile libpq module of the "PostgreSQL-devel" library.

.. code-block:: bash
   :emphasize-lines: 1-4

   # Important: in the line below, replace "<DENODO_ODBC_HOME>" with the path to the 
   # directory that contains the denodo-vdp-odbcdriver-linux.tar.gz file.
   # This file can be obtained from <DENODO_HOME>/tools/client-drivers/odbc in
   # the machine where the Denodo server is installed.

   export DENODO_ODBC_HOME=<DENODO_ODBC_HOME>

   cd $DENODO_ODBC_HOME
   
   # Downloading and building libpq included with PostgreSQL.
   wget https://ftp.postgresql.org/pub/source/v9.5.14/postgresql-9.5.14.tar.gz   
   tar -xzf postgresql-9.5.14.tar.gz
   cd postgresql-9.5.14
   ./configure --with-krb-srvnam=HTTP --with-openssl --without-readline --prefix=$DENODO_ODBC_HOME/dist/postgresql
   make install

.. rubric:: Compile UnixODBC

.. code-block:: sh

   cd $DENODO_ODBC_HOME
   
   # Downloading and building unixODBC.
   wget http://www.unixodbc.org/unixODBC-2.3.4.tar.gz
   tar -xzf unixODBC-2.3.4.tar.gz
   cd unixODBC-2.3.4
   ./configure
   make install
   
.. rubric:: Compile the Denodo ODBC driver again using the libpq library compiled above

.. code-block:: sh

   cd $DENODO_ODBC_HOME
   
   # Building the denodoODBC driver.
   tar -xzf denodo-vdp-odbcdriver-linux.tar.gz
   cd $DENODO_ODBC_HOME/denodo-vdp-odbcdriver-linux/src/denodo-pgsqlodbc
   ./configure --with-libpq=$DENODO_ODBC_HOME/dist/postgresql/bin/pg_config --prefix=$DENODO_ODBC_HOME/dist
   make install
   
If none of these commands fail, the driver should now be in "<DENODO_ODBC_HOME>/dist/lib".
