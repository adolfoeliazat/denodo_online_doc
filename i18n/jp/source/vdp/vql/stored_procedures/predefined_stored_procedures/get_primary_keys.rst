==================
GET_PRIMARY_KEYS
==================

.. rubric:: Description

The stored procedure ``GET_PRIMARY_KEYS`` returns the list of fields that
make up the primary key of a view. Each row represents a field that is part of a primary key.

.. rubric:: Syntax

.. code-block:: bnf

   GET_PRIMARY_KEYS (
         input_database_name : text
       , input_view_name : text
   )

-  If ``input_database_name`` and ``input_view_name`` are ``null``, the procedure returns all the fields that are part of a primary key of all the databases.

-  If ``input_view_name`` is ``null``, the procedure returns all the fields that are part of a primary key of all the views of ``input_database_name``.

|

The procedure returns these fields:

-  ``database_name``: name of database where the element belongs to.
-  ``view_name``: name of the view.
-  ``column_name``: name of the field.
-  ``primary_key_name``: always the value ``PRIMARY``.

.. rubric:: Privileges Required

The results of this procedure change depending on the privileges granted to the user that runs it. If the user is not an administrator user, consider the following:

-  If ``input_database_name`` is not ``null``, the procedure returns an error if the user does not have CONNECT privileges over this database.
-  The procedure will only not return information about the primary keys of views over which the user has ``METADATA`` privileges.

.. rubric:: Example

.. code-block:: sql

   SELECT view_name, column_name, primary_key_name
   FROM GET_PRIMARY_KEYS()
   WHERE input_database_name ='chinook'

The result is:

.. csv-table:: 
   :header: "view_name", "column_name", "primary_key_name"
   
   "chinook_customer", "CustomerId", "PRIMARY"
   "playlisttrack", "PlaylistId", "PRIMARY"
   "playlisttrack", "TrackId", "PRIMARY"
   "genre", "GenreId", "PRIMARY"
   "playlist", "PlaylistId", "PRIMARY"
   "invoiceline", "InvoiceLineId", "PRIMARY"
   "track", "TrackId", "PRIMARY"
   "invoice", "InvoiceId", "PRIMARY"
   "album", "AlbumId", "PRIMARY"
   "chinook_employee", "EmployeeId", "PRIMARY"
   "mediatype", "MediaTypeId", "PRIMARY"
   "artist", "ArtistId", "PRIMARY"