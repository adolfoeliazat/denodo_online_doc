======================================================
Before Beginning the Migration
======================================================

Before beginning the migration of the VQL statement from earlier versions, complete these tasks:

#. If you use the cache engine of Denodo, create a new schema on
   the database that stores the cached data. You can use the same
   database to store cached data from several versions. However, it is very important that it is
   stored on different schemas. Otherwise, the cached data may end up corrupted.
   
#. If you use the support for version control, create a new repository or a new branch that will be used to store the VQL statements of 
   Denodo |version|. Some VQL statements of Denodo |version| do not work on previous versions and vice versa. Therefore, Denodo servers from different major versions cannot point to the same VCS repository or branch. 

#. If you installed the Denodo Platform |version| in the host
   where the "old" Denodo installation runs, choose one of these
   options:

   a. Only start one version of the Denodo Platforms at the same time. E.g., you can
      start either Virtual DataPort version |version| or a previous version, but
      not both at the same time because there will be a port collision.

   #. Or, configure the new installation or the old one so they use different ports to listen
      to incoming connections. If they use different ports, they can run simultaneously on the same host.


 

 

 

 
