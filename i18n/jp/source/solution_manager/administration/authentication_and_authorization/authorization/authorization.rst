=============
Authorization
=============

Solution Manager supports several kinds of users, each one with different
access rights to specific features of the tool. This type of security is
implemented using the predefined roles in the Virtual DataPort of the Solution Manager.

.. note:: For more information about how to create users or to assign roles to
          normal users, check the section
          :ref:`Administration of Databases, Users, Roles and Their Access Rights`
          of the *Virtual DataPort Administration Guide*.

The following sections explain what kind of users the Solution Manager considers
and what privileges they have.

Solution Manager Administrator
==============================

The administrators of the Solution Manager are "normal users" - not administrators - that have the role ``solution_manager_admin``.

The goal of these users is to administer the Solution Manager and manage the Denodo licenses of the organization. Specifically, these users can:

    |yes| Create, edit and remove environments, clusters and servers.

    |yes| Set the Version Control System configuration.
    
    |yes| Set the Informative Message configuration.

    |yes| Manage licenses.

but cannot:

    |no| Manage deployment configurations.

    |no| Manage load balancing variables.

    |no| Set Virtual DataPort nor Scheduler properties in environments and clusters.

    |no| Access revisions nor deployments.

    |no| Execute diagnostic and monitoring operations.

Promotion Administrator
=======================

Promotion administrators are "normal users" - not administrators - that have the role ``solution_manager_promotion_admin``.

The goal of these users is to create revisions and promote them from the development environment to testing, from testing to production, etc. Specifically, these users can:

    |yes| Access the main information of the elements of the catalog in read only mode.

    |yes| Manage deployment configurations.

    |yes| Manage load balancing variables.

    |yes| Set Virtual DataPort and Scheduler properties in environments and clusters.

    |yes| Create, edit and remove her own revisions.

    |yes| Access the revisions from other users in read only mode.

    |yes| Validate and deploy any revision in environments.

but cannot:

    |no| Create, edit nor remove environments, clusters and servers.

    |no| Set the Version Control System configuration.
    
    |no| Set the Informative Message configuration.

    |no| Manage licenses.

    |no| Edit nor remove revisions from other users.

    |no| Execute diagnostic and monitoring operations.

Promotion Administrator for Certain Environments
================================================

Promotion administrators for certain environments are "normal users" - not administrators - that have one or more of these roles:

-  ``solution_manager_promotion_admin_development``
-  ``solution_manager_promotion_admin_production``
-  ``solution_manager_promotion_admin_staging``

The goal of these users is the same as the promotion administrators, but limited to deploying revisions on specific target environments. For example, the users with the role ``solution_manager_promotion_admin_staging`` can only validate and deploy revisions on the staging environments but not the other environments.

The table `Solution manager promotion roles`_ shows an overview of the different 
solution manager promotion administrator roles with their privileges to promote 
revisions created by users to different environment types.
      
Promotion
=========

Promotion users are "normal users" - not administrators - that have the role ``solution_manager_promotion``.

This user is intended to create revisions, validate and deploy them in
environments. More in detail, this kind of user can:

    |yes| Access the main information of the elements of the catalog in read only mode.

    |yes| Create, edit and remove her own revisions.

    |yes| Validate her own revisions in environments.

    |yes| Deploy her own revisions.

but cannot:

    |no| Create, edit nor remove environments, clusters and servers.

    |no| Create revisions loading a VQL file.

    |no| Manage deployment configurations.

    |no| Manage load balancing variables.

    |no| Set Virtual DataPort nor Scheduler properties in environments and clusters.

    |no| Set the Version Control System configuration.

    |no| Set the Informative Message configuration.

    |no| Manage licenses.

    |no| Access revisions from other users.

    |no| Execute diagnostic and monitoring operations.

Promotion for Specific Environments
================================================

Promotion users for certain environments are "normal users" - not administrators - that have one or more of these roles:

-  ``solution_manager_promotion_development``
-  ``solution_manager_promotion_production``
-  ``solution_manager_promotion_staging``

This user is interpreted from the Solution Manager point of view as a promotion user
with the difference that she can only validate and deploy her own revisions in the target
environments that have the specific scenario assigned. For example, a user with role
``solution_manager_promotion_staging`` can only validate and deploy any of her revisions
in any staging environment.

Overview of the Promotion Roles
==========================================================

The following table shows an overview of the different solution manager promotion roles with their privileges to promote revisions created by users to different environment types:

.. table:: Solution manager promotion roles
   :name: Solution manager promotion roles

   +--------------------------------------------------+-----------------------+-----------------------------------+
   |                 Role                             |         User          |          Environment Type         |
   |                                                  +------------+----------+------------+---------+------------+
   |                                                  | other user | own user | deployment | staging | production |
   +==================================================+============+==========+============+=========+============+
   | **solution_manager_promotion_development**       |            | X        | X          |         |            |
   +--------------------------------------------------+------------+----------+------------+---------+------------+
   | **solution_manager_promotion_staging**           |            | X        |            | X       |            |
   +--------------------------------------------------+------------+----------+------------+---------+------------+
   | **solution_manager_promotion_production**        |            | X        | X          | X       | X          |
   +--------------------------------------------------+------------+----------+------------+---------+------------+
   | **solution_manager_promotion**                   |            | X        | X          | X       | X          |
   +--------------------------------------------------+------------+----------+------------+---------+------------+
   | **solution_manager_promotion_admin_development** | X          | X        | X          |         |            |
   +--------------------------------------------------+------------+----------+------------+---------+------------+
   | **solution_manager_promotion_admin_staging**     | X          | X        |            | X       |            |
   +--------------------------------------------------+------------+----------+------------+---------+------------+
   | **solution_manager_promotion_admin_production**  | X          | X        |            |         | X          |
   +--------------------------------------------------+------------+----------+------------+---------+------------+
   | **solution_manager_promotion_admin**             | X          | X        | X          | X       | X          |
   +--------------------------------------------------+------------+----------+------------+---------+------------+

For example, a user with role solution_manager_promotion_deployment can only promote revisions created by herself in any deployment environment. 
A user with role solution_manager_promotion_admin_production can only promote revisions created by the own user and other users in any production environment.


JMX Administrators
==================

JMX administrators users are "normal users" - not administrators - that have the role ``jmxadmin``.

The goal of these users is to monitor the Denodo servers and diagnostic issues in them. Specifically, these users can:

    |yes| Access the main information of the elements of the catalog in read only mode.
    
    |yes| Change logging level of Virtual DataPort servers.

    |yes| Execute Denodo Monitor to gather the execution logs of the  Virtual
    DataPort servers.

but cannot:

    |no| Create, edit nor remove environments, clusters and servers.

    |no| Manage deployment configurations.

    |no| Manage load balancing variables.

    |no| Set Virtual DataPort nor Scheduler properties in environments and clusters.

    |no| Set the Version Control System configuration.

    |no| Set the Informative Message configuration.    

    |no| Manage licenses.

    |no| Access revisions nor deployments.

Global Administrator
====================

Global administrators are "normal users" with the role ``serveradmin`` or administrators.

These users can do any operation on the Solution Manager

Developer
=========

Users that do not have any of the roles mentioned above are considered developers. These users can create revisions. Specifically, these users can:

    |yes| Access the main information of the elements of the catalog in read only mode.

    |yes| Create, edit and remove her own revisions.

    |yes| Validate her own revisions in environments.

but cannot:

    |no| Create, edit nor remove environments, clusters and servers.

    |no| Create revisions loading a VQL file.

    |no| Manage deployment configurations.

    |no| Manage load balancing variables.

    |no| Set Virtual DataPort nor Scheduler properties in environments and clusters.

    |no| Set the Version Control System configuration.
    
    |no| Set the Informative Message configuration.

    |no| Manage licenses.

    |no| Access revisions from other users.

    |no| Deploy any revision.

    |no| Execute diagnostic and monitoring operations.

.. |yes| image:: ../../common_images/yes.png
.. |no|  image:: ../../common_images/no.png
