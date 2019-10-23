============================
Who Can Create JMS Listeners
============================

What a user can do regarding JMS listeners depends on if she is a local
user or a LDAP user and if she is an administrator or not:

-  Administrator users (local administrators or LDAP users with the role
   ``serveradmin``) can create JMS listeners and select any local user
   in the “VDP user name” box.
-  Administrators of the database can create JMS listeners and in the
   “VDP user name” box, select any local user that has the CONNECT
   privilege granted over this database. Note that there must be at
   least one non-administrator user with CONNECT privileges granted over
   this database.
-  Local users that are not administrators and that have the CREATE
   privilege granted can create JMS listeners but only select themselves
   in the “VDP user name” box.
-  LDAP users that are not administrators cannot create JMS listeners.

Note that LDAP users can never be selected in the “VDP user name”.

 

 
