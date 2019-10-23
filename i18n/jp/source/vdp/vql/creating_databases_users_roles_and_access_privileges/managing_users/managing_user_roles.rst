===================
Managing User Roles
===================

A role is a set of access rights that you can grant to users in the same
way you assign them specific privileges.

The section :ref:`Roles` of the Administration Guide explains what roles are
with more detail.

The syntax of the commands to create a new role is the following:

.. code-block:: bnf
   :caption: Syntax of the CREATE ROLE statement
   :name: Syntax of the CREATE ROLE statement

   CREATE [ OR REPLACE ] ROLE <name:identifier>
   [ <description:literal> ]
   [ <grant> ]*

..

   <grant> ::= (see section :ref:`Granting Privileges to a User/Role`)



.. code-block:: bnf
   :caption: Syntax of the ALTER ROLE statement
   :name: Syntax of the ALTER ROLE statement

   ALTER ROLE <name:identifier>
   [ <description:literal> ]
   [ <grant> ]*

..

   <grant> ::= (see section :ref:`Granting Privileges to a User/Role`)


The access privileges that you can assign to a role and to a user are
the same, work in the same way and the syntax of their commands is the
same.

In addition, you can assign roles to other roles (this is called “Role
inheritance”). For example, if you have the following roles:

-  A role ``vdp_developer`` with the privileges ``CONNECT``, ``METADATA``, ``EXECUTE``,
    ``CREATE`` and ``WRITE`` over the database ``admin``.
-  A role ``itpilot_developer`` with the privileges ``CONNECT``, ``METADATA``,
   ``EXECUTE``, ``CREATE`` and ``WRITE`` over the database ``itpilot``.

The following statements create these roles and a new role
``denodo_developer`` that has assigned the roles ``vdp_developer`` and
``itpilot_developer`` to it.



.. code-block:: vql
   :caption: Using Role Inheritance
   :name: Using Role Inheritance

   CREATE ROLE vdp_developer GRANT CONNECT, CREATE, METADATA, EXECUTE, WRITE ON admin;

   CREATE ROLE itpilot_developer GRANT CONNECT, CREATE, METADATA, EXECUTE, WRITE ON
   itpilot;

   CREATE ROLE denodo_developer GRANT ROLE vdp_developer,
   itpilot_developer;


The users that have the role ``denodo_developer`` assigned to them will
have the privileges ``CONNECT``, ``METADATA``, ``EXECUTE``, ``CREATE`` and ``WRITE`` over
the databases ``admin`` and ``itpilot``.

.. note:: As explained in the section :ref:`Roles` of the Administration
   Guide, the “effective permissions” of this user will be the union of the
   privileges of both roles.



