===========================================================
Statements to Work with Centralized Version Control Systems
===========================================================

This section describes the command required to work with centralized
version control systems supported by Virtual DataPort. That is, with
Subversion and Microsoft TFS repositories.

.. contents::
   :depth: 1
   :local:
   :backlinks: none

VCSCOMMIT
=========

``VCSCOMMIT`` checks the specified element into the configured version
control repository (with its dependencies). If the element already
exists, the command commits the element’s local changes (and those of
its dependencies).



.. code-block:: bnf
   :caption: Syntax of the VCSCOMMIT statement
   :name: Syntax of the VCSCOMMIT statement

   VCSCOMMIT [ DBELEMENTS ] <element-path:literal> [ SIMULATE ]

   VCSCOMMIT NOCONFLICTS [ DBELEMENTS ] <element-path:literal> SIMULATE

   VCSCOMMIT [ DBELEMENTS ] <element-path:literal> [ FORCE ]
       [ LOGMESSAGE <log-message:literal> ]

   VCSCOMMIT NOCONFLICTS [ DBELEMENTS ] <element-path:literal>
       [ LOGMESSAGE <log-message:literal> ]


It is mandatory to specify an element’s path. This path can refer to a
database, a folder or a single element. The path syntax is as follows:


-  A database: "/databases/<databaseName>", for example, "/databases/db1".

-  A folder (note that it must end with a ``/``):
   ``/databases/<databaseName>/folder.<folderName1>/[folder.folderNameN/]*``.
   For example, ``/databases/db1/folder.Folder 1/folder.Folder 2/``.

-  A single element:
   ``/databases/<databaseName>/[folder.folderNameN/]*<elementType>/[elementSubType/]<elementName>``.
   For example, ``/databases/db1/folder.Folder 1/views/My Base View``.

-  The possible element types are: ``views`` (for views and base views),
   ``datasources``, ``associations``, ``webservices``, ``widgets``,
   ``storedprocedures`` and ``listeners``.

   -  The possible element sub-types (this only applies to data sources) are:
      ``arn``, ``custom``, ``df``, ``gs``, ``itp jdbc``, ``json``, ``ldap``,
      ``odbc``, ``olap``, ``saperp``, ``ws`` and ``xml``.

   -  Jars and i18n maps are special cases and the syntax for their paths is
      different:

      -  Jars: ``/extensions/jars/<jarName>``
      -  I18n maps: ``/maps/i18n/<mapName>``

When checking a database or a folder in, all of their content and the
content’s dependencies will be checked in or committed (in the case of
databases, the associated JMS listeners will also be included).

The clause ``DBELEMENTS`` indicates that you are only committing elements
that belong to the database and not elements that are common to all the
databases: i18n maps or jars.

An optional log message can be specified with using the ``LOGMESSAGE``
clause.

The ``NOCOMMIT`` clause disables the detection of conflicts. Add this
clause to increase the performance of this statement. On the other hand,
if there are conflicts, the operation will fail. In this case, the user
will have to execute the statement again with the ``FORCE`` clause.

The check in operation will fail if conflicts are found. In this case,
``VCSCOMMIT`` will return a list of conflicting elements (i.e. elements
that are not up-to-date and contain local changes). By using the
``FORCE`` clause, conflicts will be resolved by overriding the remote
changes with the local ones.

Alternatively, by using the ``SIMULATE`` clause, the command will not
attempt to do any actual change in the remote repository, but will
inform the user about what conflicts will be found and what files will
be checked in when actually executing the command.

VCSUPDATE
=========

``VCSUPDATE`` checks the specified element out from the configured
version control repository (with its dependencies, and its contents in
the case of folders and databases) and loads it into the server. If the
element already exists, the command updates it and its dependencies.



.. code-block:: bnf
   :caption: Syntax of the VCSUPDATE statement
   :name: Syntax of the VCSUPDATE statement

   VCSUPDATE [ DBELEMENTS ] <element path:literal> [ FORCE ]
       [ REVISION <revision:literal> ] [ WITH DROPS ]

   VCSUPDATE [ NOCONFLICTS ] [ DBELEMENTS ] <element path:literal> [ FORCE ]
       [ REVISION <revision:literal> ]


It is mandatory to specify an element’s path that can be a database, a
folder or a single element (see the path syntax in :ref:`VCSCOMMIT`). In the
case of databases, their associated JMS listeners will be checked-out or
updated as necessary.

The clause ``DBELEMENTS`` indicates that you are only updating elements
that belong to the database and not elements that are common to all the
databases such as i18n maps or jars.

A revision can be specified by using the ``REVISION`` clause (see
:ref:`REVISIONS`).

The ``NOCOMMIT`` clause disables the detection of conflicts. Add this
clause to increase the performance of this statement. On the other hand,
if there are conflicts, the operation will fail. In this case, the user
will have to execute the statement again with the ``FORCE`` clause.

The check out operation will fail if there are conflicts. In this case,
``VCSUPDATE`` will return a list of conflicting elements (i.e. elements
that are not up-to-date and contain local changes). By using the
``FORCE`` clause, conflicts will be resolved by overriding the remote
changes with the local ones.

The check out operation will also fail if it detects that the deletion
of elements is required to complete the operation. In this case,
``VCSUPDATE`` will return a list of elements that need to be deleted
(e.g. elements that did not exist in the specified revision). By using
both the ``FORCE`` and the ``WITH DROPS`` clauses, the required
deletions will be performed.

VCSCONTENTS
===========

``VCSCONTENTS`` returns the elements or the actual files contained in a
database or folder of the configured version control repository. It can
also be used to obtain a list of the databases contained in the
repository.



.. code-block:: bnf
   :caption: Syntax of the VCSCONTENTS statement
   :name: Syntax of the VCSCONTENTS statement

   VCSCONTENTS { <element path:literal> [ FILES ] | LIST DATABASES }


A path to a database or folder must be specified (see the path syntax in
:ref:`VCSCOMMIT`) in order to obtain a list of the elements contained in
them. Using the ``FILES`` clause will show a list of actual files
instead of VDP elements.

``VCSCONTENTS LIST DATABASES`` stores a list of the databases contained
in the repository.

VCSSHOW
=======

``VCSSHOW`` returns the VQL of a local element and the VQL of that same
element that is stored in the VCS server. It only works for elements
under version control.



.. code-block:: bnf
   :caption: Syntax of the VCSSHOW statement
   :name: Syntax of the VCSSHOW statement

   VCSSHOW <path:literal> [ REVISION <revision:literal> ] 
       [ WITH_DEPENDENCIES ]

For example,

.. code-block:: vql
   :caption: Example of using VCSSHOW
   :name: Example of using VCSSHOW

   VCSSHOW '/databases/customer360/views/p_view1' WITH_DEPENDENCIES

The section :ref:`VCSCOMMIT` describes the syntax of ``path``.

When obtaining the remote version of an element, a revision can be
specified with the ``REVISION`` clause (see :ref:`REVISIONS`).

By adding ``WITH_DEPENDENCIES``, ``VCSSHOW`` returns the elements on
which this element depends.

REVISIONS
=========

``REVISIONS`` returns a list of existing revisions for the specified
element (see the element path syntax in :ref:`VCSUPDATE`).

.. code-block:: bnf
   :caption: Syntax of the REVISIONS statement
   :name: Syntax of the REVISIONS statement

   REVISIONS <path:literal>