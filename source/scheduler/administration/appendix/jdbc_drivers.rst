============
JDBC Drivers
============

The following table shows the JDBC adapters included with Denodo
Scheduler. For each driver the databases for which it has been tested,
the name of the class that must be specified when creating a JDBC data
source that uses the adapter and the URI format used are shown.

 
.. table:: JDBC Drivers
   :name: scheduler_administration_guide_JDBC_Drivers_Supported_by_Scheduler
   
   +-------------------------+-------------------------+-------------------------+
   |   Database              |   Class                 |   URI                   |
   +=========================+=========================+=========================+
   | Derby 10                | org.apache.derby.jdbc.  | \jdbc:derby://<host>    |
   |                         | ClientDriver            | :<port>/<database>      |
   +-------------------------+-------------------------+-------------------------+
   | Excel                   | sun.jdbc.odbc.JdbcOdbcD | \jdbc:odbc:<database>   |
   |                         | river                   |                         |
   +-------------------------+-------------------------+-------------------------+
   | Oracle 8i               | oracle.jdbc.OracleDrive | \jdbc:oracle:<protocol>:|
   |                         | r                       | @<host>:<port>:         |
   | Oracle 9i               |                         | <database>              |
   |                         |                         |                         |
   | Oracle 10g              |                         | Protocols: thin         |
   |                         |                         | (recommended), oci,     |
   | Oracle 11g              |                         | oci8, kprb              |
   |                         |                         |                         |
   | Oracle 12c              |                         |                         |
   +-------------------------+-------------------------+-------------------------+
   | PostgreSQL 8            | org.postgresql.Driver   | \jdbc:postgresql://     |
   |                         |                         | <host>:<port>/<database>|
   | PostgreSQL 9            |                         |                         |
   +-------------------------+-------------------------+-------------------------+
   | SQL Server 8.00.194     | net.sourceforge.jtds.jd | \jdbc:jtds:sqlserver:// |
   |                         | bc.Driver               | <host>:<port>/<database>|
   | SQL Server 2000         |                         |                         |
   |                         |                         |                         |
   | SQL Server 2005         |                         |                         |
   |                         |                         |                         |
   | SQL Server 2008         |                         |                         |
   |                         |                         |                         |
   | SQL Server 2008 R2      |                         |                         |
   |                         |                         |                         |
   | SQL Server 2012         |                         |                         |
   |                         |                         |                         |
   | SQL Server 2014         |                         |                         |
   +-------------------------+-------------------------+-------------------------+
   | Sybase Adaptive Server  | net.sourceforge.jtds.jd | \jdbc:jtds:sybase://    |
   | Enterprise12.5B         | bc.Driver               | <host>:<port>/<database>| 
   |                         |                         |                         |
   | Sybase Adaptive Server  |                         |                         |
   | Enterprise15            |                         |                         |
   +-------------------------+-------------------------+-------------------------+
   | JDBC-ODBC Bridge        | sun.jdbc.odbc.JdbcOdbcD | \jdbc:odbc:<database>   |
   |                         | river                   |                         |
   +-------------------------+-------------------------+-------------------------+
   | Denodo Virtual DataPort | com.denodo.vdp.jdbc.Dri | \jdbc:vdb://<host>:     |
   | 6.0                     | ver                     | <port>/<database>       |
   +-------------------------+-------------------------+-------------------------+


Adapters for IBM DB2 and MySQL, as well as those created by their
manufacturers for Microsoft SQL Server and Sybase, are not included in
the distribution of Scheduler, but can be downloaded from the Web sites
of these companies. They have also been tested successfully, and their
data are shown in the following table.


.. table:: IBM, MySQL, Microsoft, and Sybase Drivers
   :name: IBM, MySQL, Microsoft, and Sybase Drivers 

   +-------------------------+-------------------------+-------------------------+
   |   Database              |   Class                 |   URI                   |
   +=========================+=========================+=========================+
   | DB2 8.2                 | com.ibm.db2.jcc.DB2Driv | \jdbc:db2://<host>:     |
   |                         | er                      | <port>/<database>       |
   +-------------------------+-------------------------+-------------------------+
   | MySQL 4.0.15            | com.mysql.jdbc.Driver   | \jdbc:mysql://<host>    |
   |                         |                         | :<port>/<database>      |
   | MySQL 4.1.1             |                         |                         |
   |                         |                         |                         |
   | MySQL 5.x               |                         |                         |
   +-------------------------+-------------------------+-------------------------+
   | SQL Server 8.00.194     | com.microsoft.jdbc.sqls | \jdbc:microsoft:        |
   |                         | erver.SQLServerDriver   | sqlserver://<host>      |
   | SQL Server 2000         |                         | ;DatabaseName=<database>|
   |                         |                         |                         |
   | SQL Server 2005         |                         |                         |
   |                         |                         |                         |
   | SQL Server 2008         |                         |                         |
   |                         |                         |                         |
   | SQL Server 2008 R2      |                         |                         |
   |                         |                         |                         |
   | SQL Server 2012         |                         |                         |
   |                         |                         |                         |
   | SQL Server 2014         |                         |                         |
   +-------------------------+-------------------------+-------------------------+
   | Sybase Adaptive Server  | com.sybase.jdbc3.jdbc.S | \jdbc:sybase:Tds:       |
   | Enterprise 12.5B        | ybDriver                | <host>:<port>/          |
   |                         |                         | <database>              |
   +-------------------------+-------------------------+-------------------------+



The JDBC drivers in the above lists have been successfully tested,
although any other JDBC driver should work alongside Denodo Scheduler.
