=======================
Kerberos Authentication
=======================

.. toctree::
   :hidden:
   
   creating_the_roles_of_the_virtual_dataport_users.rst
   creating_an_ldap_data_source.rst
   setting-up_the_kerberos_authentication_in_the_virtual_dataport_server.rst
   configuring_the_administration_tool_to_use_kerberos_authentication.rst
   

Virtual DataPort provides support to authenticate its clients using the
Kerberos authentication protocol, which is the default authentication
method used in Microsoft Windows networks (i.e. networks using Microsoft
Active Directory). The benefits of enabling Kerberos are:

-  Single sign-on: the clients of Virtual DataPort will not have to
   provide its user credentials. E.g. when you launch the Administration
   Tool, users will not have to enter their credentials and neither JDBC
   clients.
-  The authentication of users is delegated to Active Directory. This
   simplifies the management of users and their privileges, compared to
   having to create all the users in Virtual DataPort and manage their
   passwords.

If you are interested in delegating the authentication of users to
Active Directory, but not on single-sign on, create databases with LDAP
authentication, which are easier to set up than Kerberos. To enable
Kerberos, you have to create a new user in Active Directory, create a
Service Principal Name, create a keytab file, etc. Whereas creating a
database with LDAP authentication does not require any configuration
change.

The section :ref:`Creating a Database with LDAP Authentication` explains how
to create databases with LDAP authentication.

|

When you enable Kerberos in Virtual DataPort, the following users are
still able to connect using the regular authentication method:

-  Users created locally in Virtual DataPort.
-  Users that connect to a database with LDAP authentication that use
   their credentials in the LDAP directory.

Before configuring Kerberos, you have to perform the post-installation
tasks described in the section :ref:`Setting-up Kerberos Authentication` of
the Installation Guide. Then do the following from the Administration
Tool:

#. Create the roles for the users: see section :doc:`Creating the Roles of
   the Virtual DataPort Users <./creating_the_roles_of_the_virtual_dataport_users>`.
#. Create an LDAP data source: see section :doc:`/vdp/administration/server_administration_-_configuring_the_server/kerberos_authentication/creating_an_ldap_data_source`.
#. Set up the Kerberos authentication: see section :ref:`Setting-Up the
   Kerberos Authentication in the Virtual DataPort Server`.
#. Configure the Administration Tool to use Kerberos authentication: see
   section :ref:`Configuring the Administration Tool to Use Kerberos
   Authentication`.

