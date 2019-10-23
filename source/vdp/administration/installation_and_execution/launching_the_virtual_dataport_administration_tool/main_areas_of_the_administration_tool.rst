=====================================
Main Areas of the Administration Tool
=====================================

The Virtual DataPort Administration tool has three main areas: the menu
bar, the Server Explorer and the Work Space.

The *menu bar* has the following menus:


-  **File**: it has the following submenus:

   -  **New**: it has submenus to create any type of element: folders, data
      sources, views, etc.
   -  **Change password**: it allows you to change your password.
   -  **Export database**: to export the metadata of a database or a
      specific element of it.
   -  **Export**: to export the metadata of the entire Server.
   -  **Import**: to import the VQL statements stored in a file.

      The section :ref:`Exporting and Importing the Server Metadata` explains
      how to use the dialogs “Export database”, “Export” and “Import”.

   -  **Extension management**: to import extensions to the server. I.e. jar
      files containing custom functions, custom wrappers, etc. See section
      :ref:`Importing Extensions`.
   -  **Refresh**: if there have been changes in the metadata of the
      Server, you must refresh the Tool to reflect these changes.
   -  **Disconnect**: disconnects the Administration Tool from the Virtual
      DataPort database.


-  **Administration**:

   -  **Database management**: it allows managing the Virtual DataPort
      databases. See section :ref:`Databases in Virtual DataPort`.
   -  **Role management**: it allows managing the roles of the Virtual
      DataPort users. See section :ref:`Roles`.
   -  **User management**: it allows managing the Virtual DataPort users
      and their privileges. See more about this in the :ref:`Administration Guide<User and Access Right in Virtual DataPort>`.
   -  **Server configuration**: dialog to change the settings of the
      Server. See section :ref:`Server Administration - Configuring the
      Server`.
   -  **VCS management**: dialog to change the settings of the Version
      Control Systems integration (VCS) of Virtual DataPort. See section
      :ref:`Version Control Systems Integration`


-  Tools:

   -  **VQL Shell**: tool to execute VQL statements. See section :ref:`Using the
      VQL Shell`.
   -  **Query Monitor**: tool to monitor the queries that the Server is
      currently executing. See section :doc:`/vdp/administration/installation_and_execution/launching_the_virtual_dataport_administration_tool/query_monitor`.
   -  **Catalog search**: tool to search elements in the Virtual DataPort
      server database. See section :doc:`Catalog Search <./catalog_search>`.
   -  **Invalidate cache**: tool to invalidate the cache of several views
      at once. See section :ref:`Invalidate Cache`.
   -  **Manage statistics**: tool to gather the statistics of several views
      at once. These statistics are used by the cost-based optimization
      process. See more about this in the section :ref:`Cost-Based
      Optimization`.
   -  *Trace viewer**: tool to load information about the execution of a query that was stored in a zip file using the *Save* button of the tab *Query Results*.
   -  **Web services container** and **Widgets container**: they show the
      dialogs that list the Denodo Web services and widgets. See sections
      :doc:`/vdp/administration/publication_of_web_services/publication_of_web_services` and :ref:`Publication of Views as Widgets`.
   -  **JMS listeners**: dialog to manage the JMS listeners of the database
      you are currently connected to. See section :doc:`/vdp/administration/jms_listeners/jms_listeners`.
   -  **OAuth credentials wizards**: contains two submenus to open the
      OAuth 1.0a wizard and the OAuth 2.0 wizard. See more about these
      wizards in the section :ref:`OAuth Authentication <vdp_admin_guide_path_types_oauth_authentication>`.
   -  **Admin Tool preferences**: contains submenus to configure several
      aspects of the Administration Tool. See section :ref:`Tool Preferences`.
   -  **Reset layout**: sets the layout of the Tool to its default state. That is, all the tabs go back to its default position.


-  **Help** has sub-menus to access the on-line help and
   a list of the existing functions and their syntax.
   The *Functions list* dialog lists all the available functions, including the custom functions imported by users into the Server.


The *Server Explorer* lists all the databases of the Virtual DataPort
server; and for each database, its data sources, base views, derived
views, stored procedures, etc. These elements can be organized in
Folders so they can be found easily. Using the Server Explorer, you can
do, among others, the following actions:

-  *Create* new elements: right-click on a database or on a folder,
   click **New** and then, on the desired submenu.
-  *Delete* an element: right-click on the element and click **Drop**.
-  *Copy and Paste* an element: right-click on the element and click
   **Copy**. Then, right-click on the target folder and then, **Paste**.
   As there cannot be two elements with the same name, even if they are
   in different folders, you will have to provide a name for the new
   element.
-  *Move* an element to another folder: drag an element and drop it into
   another folder. You cannot move an element between two databases.
-  See the *properties of an element*: right-click on the element and
   click **Properties**. In this dialog, you can see information about
   the selected element such as owner, description, date of creation and
   last modification, etc.
-  Expand and collapse all the nodes of the Server Explorer: right-click
   on the database or a folder and click **Expand all** or **Collapse
   all**.
-  To add a prefix to several views and/or associations at once, select
   them, right-click them and click **Prefix selected
   views/associations**.

   This option is useful when you create many JDBC base views from the same
   data source and you want to add a prefix to their name to distinguish them.
-  If you click **Discover associations** (only displayed when just JDBC base views are selected), the Server will analyze if in the database of the JDBC data source, there are foreign key constraints between the tables of the base views of this data source. If there are, the Tool will show a dialog that will allow you to create associations that mirror these foreign key constraints.
   The process of discovering associations works in the same way as when you discover the associations during the creation of JDBC base views (described in the section :ref:`Creating Base Views from a JDBC Data Source`). However, if an association is already defined, the Tool will not list it.


You can create a view from several views and, delete or move several
elements at the same time by selecting these elements and performing the
desired action. For example, to delete two views at the same do the
following:

-  Hold the Ctrl key.
-  Click on these two views.
-  Right-click on one of them and click **Drop**.

Above the Server Explorer, you can find the *Quick Search*. Type the name
of the element you are looking or select the type of that element.

The “Work Space” is the main part of the Tool and it is where all the
dialogs are displayed. You can open several elements at once. They will
be opened in tabs. Any tab can be moved to a new window and be moved
back to the main window.
