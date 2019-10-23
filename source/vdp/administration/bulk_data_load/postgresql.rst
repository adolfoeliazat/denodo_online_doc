============
PostgreSQL
============

To use the API of PostgreSQL to perform bulk data loads, first follow these steps:


1. Install the `command line tools <https://www.postgresql.org/download/>`_ on the host of the Virtual DataPort server.

   -  For Windows:

      #. Download the `PostgreSQL installer <https://www.postgresql.org/download/windows/>`_ of the same version you want to connect to.
      #. During the installation, the wizard will ask for the components you want to install. Select *Command Line Tools*.

   -  For Linux: install the `client package <https://www.postgresql.org/download/>`_  for your distribution. For example, in CentOS the name of the package is *postgresql-client*. 
      If the package for the version of the PostgreSQL you want to connect to is not available, use the closest version.
    
2. Edit the JDBC data source and click the tab **Read & Write**. Select **Use bulk data load APIs** and in the **PostgreSQL executable location** box, enter the path to the file ``psql``. This is the PostgreSQL client.
   
