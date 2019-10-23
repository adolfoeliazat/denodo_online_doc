====================
CATALOG_PERMISSIONS
====================

.. rubric:: Description

The stored procedure ``CATALOG_PERMISSIONS`` returns information about
the privileges and custom policies granted to a user and/or a role.

Each privilege is returned in a row. That is, there is one row for each
privilege granted to a user or role over a database, a view or stored procedure.

Any user can execute this procedure but the results are different
depending on if the user is an administrator or not. Administrators can
see the privileges granted to any user or role and non-administrators
can only see the privileges granted to directly to themselves or their roles.

This procedure does not return information about Kerberos or LDAP users (i.e. users not created on Virtual DataPort), unless is about the user running the stored procedure.

.. rubric:: Syntax

.. code-block:: bnf

   CATALOG_PERMISSIONS ( 
         username_in : text
       , rolename_in : text
   )

-  ``username_in``: name of the user that you want to obtain its
   privileges of.
-  ``rolename_in``: name of the role that you want to obtain its
   privileges of.

If the user that executes the procedure is an administrator, consider
the following:

-  If both parameters are ``null``, the procedure returns all the
   privileges granted to all the users and roles. This is useful if you want to
   obtain all the privileges set in the Virtual DataPort server that you
   are connected to.
-  If ``rolename_in`` is ``null`` but ``username_in`` is not, the procedure
   returns the privileges granted to a user and the privileges granted
   to the roles of the user.
-  If ``username_in`` is ``null`` but ``rolename_in`` is not, the procedure
   returns the privileges granted to the role and also to the roles of
   this role.
-  If both parameters have a value, the procedure returns the privileges
   directly granted to ``username_in`` and the privileges granted to the
   role ``rolename_in``. In this case, ``username_in`` needs to have the
   role ``rolename_in`` granted. Otherwise, the procedure fails.

If the user that executes the procedure is not an administrator, its
behavior is similar with the following exceptions:

-  The procedure does not return the privileges of other users.
-  If ``username_in`` is not ``null``, it has to be the name of the user
   that is executing the procedure. Otherwise, the procedure returns an
   error.
-  If ``rolename_in`` is not ``null``, the user that runs
   procedure has to have this role granted directly or transitively.
   Otherwise, the procedure returns an error.

Each privilege or custom policy granted to a user/role is returned in a
row.

The output schema has the following fields:

-  ``username``: user that this privilege belongs to. ``null`` if this row refers to a privilege granted to a role.

   When the input parameter ``username_in`` is not ``null`` or when ``username_in`` and ``rolename_in`` are
   ``null``, it holds the user name. In the latter case, this field is
   ``null`` for the privileges granted to roles.

-  ``globaladmin``: the possible values are:

   -  ``null`` if the row represents a privilege granted to a role, not a user.
   -  ``true`` if the row represents a privilege granted to a user (not a role) and the user is an administrator or has the role ``serveradmin``.
   -  ``false`` otherwise.

-  ``userrolename``: when this field is not ``null``, it means that the
   row represents a privilege inherited from the role with this value.
   
   If the field ``username`` is not ``null``, the row represents a
   privilege of a user inherited from a role.
   
   If the field ``rolename`` is not ``null``, the row represents a
   privilege of a role inherited from another role.
   
   This field is ``null`` if the privilege was directly granted to the
   user or the role.

-  ``rolename``: a privilege can be inherited “transitively”. That is, a
   user can inherit a privilege from a role which itself inherits it from
   another role. This column contains the original role for which the
   permission was granted.

-  ``dbname``: name of the database over which this privilege is granted.

-  ``elementname``: name of the element of a database over which this privilege was granted. ``null`` if this privilege is granted over a database, not a single element.

-  ``elementype``: type of element of a database over which this privilege is granted. The possible values are ``Data source``, ``View``, ``Procedure``, ``Web service`` or ``Widget``.

-  ``elemensubtype``: ``null`` if the element type is not equals to ``Data source``. Otherwise, type of the data source. The possible values are ``arn``, ``df``, ``essbase``, ``gs``, ``jdbc``, ``json``, ``custom``, ``odbc``, ``olap``, ``salesforce``, ``sapbwbapi``, ``saperp``, ``ws``, ``xml``.

-  ``dbadmin``, ``dbconnect``, ``dbcreate``, ``dbcreatedatasource``, ``dbcreatedataservice``, ``dbcreateview``, ``dbcreatefolder``, ``dbexecute``, ``dbwrite``, ``dbmetadata`` and ``dbfile``: represent the privileges that can be granted over a database. ``null`` if this row represents a privilege not granted over a database (e.g. over an individual element).

-  ``elementmetadata``, ``elementexecute``, ``elementwrite``, ``elementinsert``,
   ``elementupdate`` and ``elementdelete``: represent the privileges that can be granted over an element of a database, not a database.

-  ``columnpermissions``: it is not ``null`` when the row represents a
   privilege granted over a view that has column privileges. In this case, this field contains a
   comma-separated list of the fields that can be projected.

-  ``rowpermissions``: it is not ``null`` when the row represents a
   privilege granted over a view that has row restrictions. In this case, this field is an array
   with the following fields:

   -  ``sensitivefields``: the sensitive fields of the restriction.
   -  ``condition``: the condition of the restriction.
   -  ``action``: the action taken by the restriction when the condition is
      not met.

-  ``custompermissions``: it is not ``null`` when the array represents a
   list of custom policies granted to a user/role, over a view.


.. rubric:: Privileges Required

The information returned by this procedure changes depending on the type
of user that executes the procedure:

-  Administrators: the procedure returns information about the
   privileges of any user or role.
-  Other users, including administrators of a database: the procedure only returns information 
   about the privileges granted to the user that executes the procedure, or to the roles granted to this user. 
   
The procedure returns an error in any of these cases:

1. If a non-administrator user requests the privileges granted to another user.
#. Or if a non-administrator user requests the privileges granted to a role and this user does not have this role.
#. Or the input user is a Kerberos or LDAP user (i.e. a user not created on Virtual DataPort) and is not the user running the procedure. In this case, the procedure returns an error saying that the user does not exist.