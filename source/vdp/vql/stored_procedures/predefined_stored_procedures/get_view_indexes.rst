========================
GET_VIEW_INDEXES
========================

.. rubric:: Description

The stored procedure ``GET_VIEW_INDEXES`` returns a list of the indexes of views. These are the indexes that in the administration tool you see in the tab *Indexes* of the *Options* of the view. 

Each row represents a field of an index.

.. rubric:: Syntax

.. code-block:: bnf

   GET_VIEW_INDEXES (
         input_database_name : text
       , input_view_name : text
   )

-  If you invoke the procedure using ``CALL`` and do not want to filter by a parameter, pass ``null``.

- If ``input_database_name`` and ``input_view_name`` are ``null``, the procedure returns all the fields that are part of an index.

- If ``input_view_name`` is ``null``, the procedure returns all the fields that are part of an index of the ``input_database_name``.

|

The procedure returns these fields:

-  ``database_name``: name of database where the element belongs to.
-  ``view_name``: name of the view.
-  ``unique``: true if the field belongs to a unicity index; false otherwise.
-  ``index_name``: name of the index.
-  ``type``: type of the index.
   
   -  ``1``: this is a clustered index.
   -  ``2``: this is a hashed index.
   -  ``3``: this is some other style of index.

-  ``ordinal_position``: column sequence number within index. The first column is 1.
-  ``column_name``: column name.
-  ``asc_or_desc``: column sort sequence 

   -  ``A``: ascending
   -  ``D``: descending
   -  ``null``: sort sequence is not supported

-  ``filter_condition``: filter condition or ``null`` if there is no filter condition.

.. rubric:: Privileges Required

The results of this procedure change depending on the privileges granted to the user that runs it. If the user is not an administrator user, consider the following:

-  If the parameter ``input_database_name`` is not ``null``, the procedure returns an error if the user does not have CONNECT privileges over this database.
-  The procedure will only return information about the indexes of the views over which the user has ``METADATA`` privileges.

.. rubric:: Example

.. code-block:: sql
   
   SELECT unique, index_name, type
   FROM get_view_indexes()
   WHERE input_database_name ='chinook' AND input_view_name = 'album'

The result is:

.. csv-table:: 
   :header: "index_name", "unique", "type"
   
   "pk_index", "true", "3"
   "IFK_AlbumArtistId", "false", "3"
