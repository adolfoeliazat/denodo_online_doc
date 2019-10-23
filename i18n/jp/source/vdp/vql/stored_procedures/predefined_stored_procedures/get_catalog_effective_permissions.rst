====================================
GET_CATALOG_EFFECTIVE_PERMISSIONS
====================================

The stored procedure ``GET_CATALOG_EFFECTIVE_PERMISSIONS`` returns the effective privileges granted to a user. It returns a row for each database and a row for each element over which the user has any privilege.

The procedure :ref:`CATALOG_PERMISSIONS` also returns information about privileges but the result is different:

-  "CATALOG\_PERMISSIONS" returns the privileges and roles granted to each user/role.
-  "GET\_CATALOG\_EFFECTIVE\_PERMISSIONS" returns the effective privileges that the user has over each element.

For example, let us say you grant the role *read* to a user over a database. "CATALOG\_PERMISSIONS" will return one row indicating this privilege. "GET\_CATALOG\_EFFECTIVE\_PERMISSIONS" will return one row per each element of this database.

.. rubric:: Syntax

.. code-block:: bnf

   GET_CATALOG_EFFECTIVE_PERMISSIONS (
         input_user_name : text
       , input_database_name : text )

-  ``input_user_name`` (mandatory): name of the user.
-  ``input_database_name`` (optional): if ``null``, the procedure returns all the elements over which the user has privileges. If not ``null``, it returns the privileges of the user over the elements of this database.

The procedure returns one row per each element over which the user has a privilege. Each row has these fields:

-  ``username``: name of the user. The value will always be the same as ``input_user_name``.
-  ``globaladmin``: ``true`` if ``input_user_name`` is an administrator. Otherwise, ``false``.

-  ``dbname``: name of the database over which this privilege is granted.

-  ``elementname``: name of the element over which this privilege is granted. ``null`` if this privilege is granted over a database, not a single element.

-  ``elementype``: type of element of a database over which this privilege is granted. The possible values are ``Database``, ``Data source``, ``View``, ``Procedure``, ``Web service`` and ``Widget``.

-  ``elemensubtype``: ``null`` if the element type is not equals to ``Data source``. Otherwise, type of the data source. The possible values are ``arn``, ``df``, ``essbase``, ``gs``, ``jdbc``, ``json``, ``custom``, ``odbc``, ``olap``, ``salesforce``, ``sapbwbapi``, ``saperp``, ``ws``, ``xml``.

-  ``dbadmin``, ``dbconnect``, ``dbcreate``, ``dbcreatedatasource``, ``dbcreatedataservice``, ``dbcreateview``, ``dbcreatefolder``, ``dbexecute``, ``dbwrite``, ``dbmetadata`` and ``dbfile``: represent the privileges that can be granted over a database.

-  ``elementmetadata``, ``elementexecute``, ``elementwrite``, ``elementinsert``,
   ``elementupdate`` and ``elementdelete``: represent the privileges that can be granted over an element of a database, not a database. ``null`` if this row represents a privilege granted over a database (e.g. not over an individual element).

-  ``columnpermissions``: it is not ``null`` when the row represents a
   privilege granted over a view that has column privileges. In this case, this field contains a
   comma-separated list of the fields that can be projected.

-  ``rowpermissions``: ``true`` if this row represents a privilege over a view and the user has a row restriction over this view. ``false`` otherwise.

-  ``custompermissions``: ``true`` if this row represents a privilege over a view and the user has a custom policy applied over this view. ``false`` otherwise.
   
.. note::
  
   The columns ``columnpermissions``, ``rowpermissions`` and ``custompermissions`` are not affected by the configuration property:
  
   *com.denodo.vdb.catalog.user.User.enableCheckViewRestrictionAlways*.
  
   This means that even the value of this columns are ``null`` or ``false`` some restrictions could be applied to the elements if the property is set to ``true``.
  
   The section :ref:`Column Privileges` explains the meaning of this property.
   
.. rubric:: Privileges Required

The result of this procedure changes depending on the privileges of the user that runs it:

-  Administrators: the procedure returns information about the
   privileges of any user.
-  Other users, including administrators of a database: the procedure only returns information 
   about the privileges granted to the user that executes the procedure. 

The procedure returns an error in any of these cases:

1. If the input parameter ``input_user_name`` is a username that does not exist.
#. If the input parameter ``input_database_name`` is a database that does not exist.   
#. If a non-administrator user requests the privileges granted to another user.
#. If the input user is a Kerberos or LDAP user (i.e. a user not created on Virtual DataPort) and is not the user running the procedure. In this case, the procedure returns an error saying that the user does not exist.