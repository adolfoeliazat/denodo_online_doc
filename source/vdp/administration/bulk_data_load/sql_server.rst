====================
Microsoft SQL Server
====================

To be able to use the Microsoft SQL Server API to perform bulk data loads, first you need to follow these steps:

#. Install the *Microsoft Command Line Utilities for SQL Server* on the host where the Virtual DataPort server runs:

  a. If the Denodo server runs on Windows, download the `package <https://www.microsoft.com/en-us/download/details.aspx?id=53591>`_ and install it.
  
  b. If the Denodo server runs on Linux, follow `the steps <https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-setup-tools?view=sql-server-2017>`_ for your platform.

2. Edit the JDBC data source and click the tab **Read & Write**. Then, select **Use bulk data load APIs** and in the **Bcp executable location** box, enter the path to the file ``bcp``.
  
   - Windows: it usually is in :file:`c:\\Program Files\\Microsoft SQL Server`. For example, :file:`c:\\Program Files\\Microsoft SQL Server\\bcp.exe`.
  
   - Linux: it usually is in :file:`/opt/mssql-tools/bin`. For example, :file:`/opt/mssql-tools/bin/bcp`.

The process is the same regardless of if you selected a jTDS adapter or a Microsoft adapter.
