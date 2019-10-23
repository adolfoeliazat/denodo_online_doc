================
Creating Folders
================

The elements created in Virtual DataPort can be organized in folders.
These are: data sources, stored procedures, wrappers, views, Web
services and widgets.

Although these elements can be organized in folders, you cannot create
two elements with the same name in a database, even if they are located
different folders.

.. code-block:: bnf
   :caption: Syntax of the CREATE FOLDER statement
   :name: Syntax of the CREATE FOLDER statement

   CREATE [ OR REPLACE ] FOLDER <folder_name:literal>
   [ DESCRIPTION <description> ]


``folder_name`` is the full path of the new folder. That means that this
parameter has to start with the character ``/``. For example,
``CREATE FOLDER '/folder1'``.

If you want to create a subfolder ``/folderA/subfolderA``, you have to
create the folder ``folderA`` first and then, the ``subfolderA`` (see
:ref:`below <Creating a subfolder>`).



.. code-block:: vql
   :caption: Creating a subfolder
   :name: Creating a subfolder

   CREATE FOLDER '/folderA'
   CREATE FOLDER '/folderA/subfolderA'


To manage an existing folder and its contents, use the statement
``ALTER FOLDER``. With
it, you can do the following actions:

-  Change the description of a folder: ``ALTER FOLDER`` ...
   ``DESCRIPTION`` ...
-  Rename a folder: ``ALTER FOLDER`` … ``RENAME`` …
-  Move a folder to a different location ``ALTER FOLDER`` ...
   ``RENAME`` ...
-  Move an element (data source, view, etc.) to another folder
-  Copy an element (data source, view, etc.): ``ALTER FOLDER COPY`` ...

.. code-block:: bnf
   :name: Syntax of the statement ALTER FOLDER
   :caption: Syntax of the statement ALTER FOLDER

   ALTER FOLDER <source folder name:literal> <operation>
   
   <operation> ::=
       RENAME <target folder name:literal> [ DESCRIPTION <description:literal> ] 
     | DESCRIPTION <description:literal> 
     | MOVE <element> <name:identifier>
     | COPY <element> <element old name:identifier> AS <element new name:identifier> 

   <element> ::= 
       DATASOURCE <datasource type> 
     | WRAPPER <wrapper type>
     | TABLE
     | VIEW
     | STOREDPROCEDURE
     | WEBSERVICE
     | WIDGET
   
   <data source type> ::=
       ARN
     | CUSTOM 
     | DF 
     | ESSBASE 
     | GS 
     | JDBC 
     | JSON 
     | LDAP 
     | ODBC 
     | OLAP 
     | SALESFORCE
     | SAPBWBAPI 
     | SAPERP 
     | WS 
     | XML
       
   <wrapper type> ::= 
       <data source type> 
     | ITP  

To *change the description of a folder*, execute the command
``ALTER FOLDER <folder-name> DESCRIPTION...``.

For example:

.. code-block:: vql
   :caption: Changing the description of an existing folder
   :name: Changing the description of an existing folder

   ALTER FOLDER '/folder for data sources'
       DESCRIPTION 'This folder will contain data sources';


To *rename a folder*, execute the command
``ALTER FOLDER <folder-name> RENAME...``

The :ref:`following <Renaming a folder>` statement changes the name of the
folder ``folder for data sources`` to ``folder_for_ds``.

.. code-block:: vql
   :caption: Renaming a folder
   :name: Renaming a folder

   ALTER FOLDER '/folder for data sources'
       RENAME '/folder_for_ds';


You also have to use this syntax to *move a folder into another folder*.
The :ref:`following <Moving a folder into another folder>` statement moves
the folder ``/folder_for_ds`` into the folder ``/folderA``.



.. code-block:: vql
   :caption: Moving a folder into another folder
   :name: Moving a folder into another folder

   ALTER FOLDER '/folder_for_ds'
       RENAME '/folderA/folder_for_ds';


To *move an element to another folder*, execute
``ALTER FOLDER <folder_name> MOVE...`` being ``folder_name`` the
target folder.

Depending on the element that you want to move, you have to use the
parameter ``DATASOURCE JDBC``, ``DATASOURCE ODBC...``, ``TABLE`` (*for
moving base views*), ``VIEW`` (*for moving combined views*), etc.

The :ref:`following <Moving a JDBC data source to a folder>` statement
moves a data source JDBC into the folder ``/folderA``.


.. code-block:: vql
   :caption: Moving a JDBC data source to a folder
   :name: Moving a JDBC data source to a folder

   ALTER FOLDER '/folderA'
       MOVE DATASOURCE JDBC ds_jdbc_internet_inc;


To *copy an element*, execute the command
``ALTER FOLDER <folder_name> COPY...`` being ``folder_name`` the
target folder for the new element. The syntax is similar to
``ALTER FOLDER ... MOVE``, but with ``AS <element new name:identifier>``
at the end to indicate the name of the target folder.

Remember that you cannot create two elements in the same database, with
the same name and type, even if they are in different folders.

The :ref:`following <Copying a Web service data source>` statement copies a Web service
data source into the folder ``folderA``.

.. code-block:: vql
   :caption: Copying a Web service data source
   :name: Copying a Web service data source

   ALTER FOLDER '/folderA'
       COPY DATASOURCE WS ds_ws_sales AS ds_ws_sales_2;


Use the ``DROP FOLDER...`` statement to *delete a folder* (see :ref:`Syntax
of the DROP statement`)
