===========================
Statements to Work with GIT
===========================

This section describes the command required to work with GIT. The
commands are different than for Subversion and Microsoft TFS because GIT
is a distributed version control system and the other ones are
centralized systems.

.. contents::
   :depth: 1
   :local:
   :backlinks: none

DVCSCOMMIT
==========

``DVCSCOMMIT`` checks the specified element into the version control
repository, along with the dependencies of the element that have been
modified.


.. code-block:: bnf
   :caption: Syntax of the DVCSCOMMIT statement
   :name: Syntax of the DVCSCOMMIT statement

   DVCSCOMMIT <element-path:literal> [ LOGMESSAGE <log-message:literal> ]


Optionally, you can add a message to the commit in the ``LOGMESSAGE``
clause.

DVCSPUSH
========

``DVCSPUSH`` transfers the commits of a database from your local
repository to the remote repository.



.. code-block:: bnf
   :caption: Syntax of the DVCSPUSH statement
   :name: Syntax of the DVCSPUSH statement

   DVCSPUSH <database_name:literal>

DVCSPULL
========

``DVCSPULL`` merges the commits to a database, on the remote repository
into your local repository.



.. code-block:: bnf
   :caption: Syntax of the DVCSPULL statement
   :name: Syntax of the DVCSPULL statement

   DVCSPULL <database_name:literal>

DVCSREVERT
==========

``DVCSREVERT`` resets the local repository of a database to a specific
commit. To do so, it reverts each commit, starting from the last one,
until it reaches the selected commit.



.. code-block:: bnf
   :caption: Syntax of the DVCSREVERT statement
   :name: Syntax of the DVCSREVERT statement

   DVCSREVERT <database name:literal> <commit id:literal>
