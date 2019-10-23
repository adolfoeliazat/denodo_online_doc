========================
Connecting to the Server
========================


.. toctree::
   :hidden:

   obtaining_wrappers/obtaining_wrappers.rst
   using_wrappers/using_wrappers.rst
   processing_query_results/processing_query_results.rst
   example_of_use/example_of_use.rst

There are two ways in which a connection to the ITPilot execution server
can be added depending on whether the Virtual DataPort server is installed in the same location as ITPilot.



If Denodo ITPilot has been installed separately, then the default server
connection mode should be used (constructor
``HTMLWrapperServerProxy(String host, int port)``) indicating the
machine and port in which the server is executed.



If ITPilot is installed jointly with Virtual DataPort,
then Virtual DataPort will be used as an execution server for ITPilot. In this
case, it is possible to specify any database created in the Virtual
DataPort server in the connection to the server and use any user defined
in it. The actions allowed for the user will be coherent with the
permissions assigned to said user in the Virtual DataPort server for the
specified database.



In this case the constructor
``HTMLWrapperServerProxy(String host, int port, String dbName, String login, String password)`` may
be used. In this constructor, in addition to the machine and port in
which the server is executed, the name of the database of the Virtual
DataPort server to which the connection is to be made should be
specified as well as the user ID with which access is to be made and the
associated password.



It is important to highlight that, even if Virtual DataPort is
installed, it is equally possible to access the server using the default
mode (constructor ``HTMLWrapperServerProxy(String host, int port)``). In
this case, a default database called ‘itpilot’ will be accessed. The
predefined user ‘admin’ (with the initial password ‘admin’) will be used
to gain access.