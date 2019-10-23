======
MySQL
======

To use the API of MySQL to perform bulk data loads, the system variable "local_infile" of MySQL has to be enabled.

-  For MySQL 8.x:
  
   #. Execute this command in MySQL with an administrator account to check if "local_infile" is enabled:
      
      .. code-block:: sql
         
         SHOW GLOBAL VARIABLES LIKE 'local_infile';

   #. If the setting is disabled execute this in MySQL to enable it:

      .. code-block:: sql

         SET GLOBAL local_infile = true;
   
-  MySQL 5.x: with the default configuration, "local_infile" is enabled so you do not have to change anything.
