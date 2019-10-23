==================
Obtaining Wrappers
==================

As mentioned in the preceding section, connection to the execution
server consists of creating an instance of the class
``com.denodo.itpilot.client.HTMLWrapperServerProxy``. This class
incorporates methods for obtaining data on the execution server and
accessing wrappers present in it:

-  ``Collection getHTMLWrapperNames()``. Obtains a collection with the
   name of the wrappers present in the execution server. Note that if
   Virtual DataPort is being used as execution server, the connection
   will have been made to a Virtual DataPort database and only those
   wrappers associated with said database will be obtained.
-  ``HTMLWrapperProxy getHTMLWrapper(String wpName)``. Obtains a
   reference to the wrapper of the name specified as parameter.
-  ``Collection getDatabaseNames()``. This method can only be invoked by
   users with administration rights in Virtual DataPort. It returns a
   collection with the name of the databases that exist in the server.
-  ``void deleteWrapper(String wpName)``. Deletes the wrapper which name
   is specified as parameter from the Server.
-  ``void loadWrapper(String vql)``. Takes as input argument the VQL
   that defines a collection of wrappers, that are loaded in the
   execution server.
-  ``String getVQL()``. Returns the VQL description of all wrappers in
   the ITPilot execution server.
