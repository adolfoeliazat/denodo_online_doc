===================================
Commands for Environment Management
===================================

Environments, in the VCS integration context, consist on a name and an
optional description (see section :ref:`Environment Management` of the
Administration Guide). The following figure shows the syntax to create,
modify and delete environments, and to list the environments available
in the configured version control repository:



.. code-block:: bnf
   :caption: Syntax to create, modify, delete and list the available environments
   :name: Syntax to create, modify, delete and list the available environments

   CREATE ENVIRONMENT <name:identifier> [ <description:literal> ]

   ALTER ENVIRONMENT <name:identifier> <description:literal>

   DROP ENVIRONMENT <name:identifier>

   LIST ENVIRONMENTS


VCS must be configured and activated in order to execute any
environment-related command. In addition, only global administrator
users are allowed to create, alter or drop an environment.

By creating an environment, a properties file named like the
environment and containing the environment’s name and description is
checked into the VCS repository configured for the VDP server.
Environment files are located under
``<path_to_remote_repository>/environments`` in the remote repository
(e.g. ``svn://svn.acme.com/denodorepo/trunk/environments``).

When dropping an environment, all the properties files associated to
the environment are also removed from the corresponding remote
repository.
