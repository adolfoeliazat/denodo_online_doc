=====================================
Launching the Virtual DataPort Server
=====================================

There are two options to start and stop the Virtual DataPort server:


#. Use the Denodo Platform Control Center, which allows to, among other things, start and stop
   all servers and tools of the Denodo Platform.


#. With the scripts of the :file:`{<DENODO_HOME>}/bin/` directory:

   -  To start the Server: ``vqlserver_startup``
   -  To stop the Server: ``vqlserver_shutdown``
   -  To stop the Server in “Safe shutdown mode”, 
      ``vqlserver_shutdown safe``. When entering this mode, the Server does
      not accept more connections and does not shutdown until all the
      queries finish.

