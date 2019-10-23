================================================
Creating the Roles of the Virtual DataPort Users
================================================

Kerberos is an authentication mechanism. That is, it verifies “who you
are”. However, you also need an authorization mechanism to verify what
each user can do, once it has been authenticated. Virtual DataPort
provides an authorization mechanism based on the groups that each user
belongs to in the Active Directory or any other LDAP server.

At runtime, when a user connects to a Virtual DataPort server with
Kerberos authentication, the Server will obtain the names of the roles
assigned to this user. The actions that the user will be authorized to
do will be defined by the privileges assigned to the roles defined in
Virtual DataPort. Therefore, you have to create the roles of the users
that match the name of the roles of these users in the Active Directory.
Then, assign privileges to these roles in Virtual DataPort.

The process to create these roles is the same described for databases
with LDAP authentication. The section :ref:`Creating a Database with LDAP
Authentication` explains how to do this.

Remember to assign at least the “Connect” privilege to the roles so the
users can connect to a Virtual DataPort database.

