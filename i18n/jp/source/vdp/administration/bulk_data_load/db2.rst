===
DB2
===

To use the API of IBM DB2 to perform bulk data loads, first follow these steps:

1. Install the package "IBM Data Server Runtime Client" on the host of the Virtual DataPort server.

   Check the documentation of your version of DB2 to see how to do this. You need to install the client for the architecture where the Denodo server runs. For example, if the Denodo server runs on Windows, install the Windows package regardless of the platform on which DB2 runs.
   
   For DB2 11.1:
   
   -  `Installing IBM data server clients and drivers (Windows) <https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.swg.im.dbclient.install.doc/doc/t0007315.html?pos=2>`__.
   -  `Installing IBM data server clients (Linux and UNIX) <https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.swg.im.dbclient.install.doc/doc/t0007317.html>`__.
   
   For DB2 10.5.0:
   
   -  `Installing IBM data server clients and drivers (Windows) <https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.swg.im.dbclient.install.doc/doc/t0007315.html?pos=2>`__.

   -  `Installing IBM data server clients (Linux and UNIX) <https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.swg.im.dbclient.install.doc/doc/t0007317.html>`__.


#. Edit the JDBC data source and click the tab **Read & Write**. Select **Use bulk data load APIs** and in the **DB2 executables location** box, enter the path to the directory (*not the file*) where you installed the IBM client.