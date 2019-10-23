==============
Managing Users
==============


.. toctree::
   :hidden:

   modifying_and_deleting_users.rst
   connecting_to_another_database.rst
   modifying_the_privileges_of_a_user.rst
   managing_user_roles.rst

The ``CREATE USER`` statement (see :ref:`Syntax of the CREATE USER statement`) allows creating a new
user in the Server. There are two types of users:

#. “Administrator” users (created by adding the ``ADMIN`` parameter to
   this statement). These users can perform any action over any
   database. You cannot restrict the privileges of an administrator
   user.
   
   Only administrator users can create administrator users.
#. “Normal” users. After creating them you have to grant them privileges
   so they can perform operations over the elements of the Server
   databases.

There are two authentication methods for Virtual DataPort users:

1. “Normal”: the credentials of the user are stored in Virtual DataPort.


#. “LDAP” (indicated with the ``LDAP`` parameter): when the user tries to
   connect to the Server, the Server connects to an LDAP server to check
   that the password provided by the user is correct.

   To use this authentication method, you have to create an LDAP data
   source that will be used to connect to the LDAP server, in order to
   check that the passwords provided by users are correct.

   The parameter ``LDAP`` has two parameters:

   a. ``DATASOURCE``. The syntax is ``<databaseName>.<dataSourceName>``.
      ``<databaseName>`` is the Virtual DataPort database where the LDAP
      data source is stored and ``<dataSourceName>`` is the name of the
      data source.
   b. ``USERNAME``. It is the name of the user in the LDAP server. For
      example, the value ``'cn=test,ou=People,dc=denodo,dc=com'``
      identifies the ``test`` user in an organizational unit ``People`` for
      the domain ``denodo.com``.

   .. code-block:: bnf
      :caption: Syntax of the CREATE USER statement
      :name: Syntax of the CREATE USER statement
   
      CREATE [ OR REPLACE ] USER [ ADMIN ] <name:identifier> 
          <authentication> 
          [ <description:literal> ]
          [ <grant> ]*
          
      <authentication> ::= 
            <password:literal> SHA512
          | LDAP ( 
                DATASOURCE <databaseName:identifier>.<dataSourceName:identifier>
                USERNAME <name:literal>
            )
      
..
      
      <grant> ::= (see :ref:`Syntax of the clauses GRANT/REVOKE of CREATE USER and ALTER USER`)


The section :doc:`./modifying_the_privileges_of_a_user` explains how to modify the privileges
of existing users.

``SHA512`` indicates that the password will be stored as a SHA512 hash.

.. note:: The LDAP authentication of users is different from databases
   with LDAP authentication.

   When the authentication type of a user is ``LDAP``, the LDAP
   server is only used to check that the password provided by the user is
   correct. However, the privileges of this user are still managed from
   Virtual DataPort.

   In a database with LDAP authentication, the privileges of the users
   are also obtained from the LDAP server.

.. note:: We do not recommend creating users with LDAP authentication.
   Instead, create databases with LDAP authentication, which will simplify
   the management of users and their privileges. See more about this type
   of databases in the section :ref:`Creating a Database with LDAP Authentication` of the Administration Guide.

.. note:: If an LDAP data source is deleted on cascade (see section :ref:`Removing Elements from the Catalog`), then the users depending on it
   will be also deleted. This operation can only be executed by an
   administrator user.
   
.. rubric:: Example
   
Creating a user with some privileges over the database "customer": 

.. code-block:: vql

   -- Encrypt the password that you want the new user to have
   ENCRYPT_PASSWORD 'new password of the user';
   
   -- Create the user
   CREATE OR REPLACE USER new_user '<result of the command ENCRYPT_PASSWORD>' ENCRYPTED TRANSFER
   GRANT CONNECT, METADATA, EXECUTE, WRITE ON customer;

