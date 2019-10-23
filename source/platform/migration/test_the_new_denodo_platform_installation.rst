=========================================
Test the New Denodo Platform Installation
=========================================

If there is a test plan in place execute it against both versions and
check that all tests are successful in both of them and the results are
the same.

If there is no test plan in place, prepare a set of tests, covering:

-  Queries at least one base view of each data source. This ensures that the
   connection to that data source works. The connection could not work
   due to a firewall closing the connections from the host where the new
   Denodo Platform is installed.
-  Queries to several business views to check that the results are the
   expected ones.
-  Performance of queries that involve a lot of data.
-  Check that all the interfaces work correctly for JDBC, ODBC and Web
   service clients.
-  User authentication and authorization: users can access the server
   and they can only access the correct resources specified by server
   permissions.

 
