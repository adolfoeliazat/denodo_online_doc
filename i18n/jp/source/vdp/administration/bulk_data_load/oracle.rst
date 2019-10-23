======
Oracle
======

To be able to use the Oracle API to perform bulk data loads, first follow these steps:

#. On the Oracle database, enable direct path loads. To prepare it for direct path loads, the administrator of Oracle needs to run the setup script ``catldr.sql`` of Oracle, which creates the necessary views in Oracle to do direct loads. 
   The administrator only needs to run this script once for each Oracle installation you plan to do direct loads to.

   The documentation of Oracle has more information about `direct path loads <https://docs.oracle.com/cd/E11882_01/server.112/e22490/ldr_modes.htm#SUTIL1288>`_.

   .. important:: If you cannot enable this feature on the database, do **not** continue with these steps. If you enable bulk data loads in Virtual DataPort and direct path loads are not enabled in Oracle, the performance will be much worse than with the regular method of inserting data into Oracle (i.e. executing ``INSERT`` statements).
   
#. Download the package *Oracle Database Client* (you do not need the installer of the full database). Download it from the URLs below:

   For Oracle 11g Release 2:
   
   a. If the Denodo server runs on 64-bit Windows:
      https://www.oracle.com/technetwork/database/enterprise-edition/downloads/112010-win64soft-094461.html
   b. If the Denodo server runs on Linux:
      https://www.oracle.com/technetwork/database/enterprise-edition/downloads/112010-linx8664soft-100572.html

   In this page, search the "Oracle Database 11g Release 2 Client".

#. Install this package on the host where the Virtual DataPort server runs.
   
   During the installation, select the **Administrator** installation type, which includes the program ``sqlldr``. This is necessary to do bulk data loads.
   
   The default options of the installer are correct. 

#. Edit the JDBC data source and click the tab **Read & Write**. Then, select **Use bulk data load APIs** and in the **Sqlldr executable location** box, enter the path to the file ``sqlldr``. This file is located where the "Oracle Database Client" was installed.
