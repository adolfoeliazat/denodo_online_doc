============================
Export as Microsoft Web Part
============================

.. note:: Publishing views as widgets is a deprecated feature and it may be removed in future
   major versions of the Denodo Platform.
   
   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

A widget can be exported as a Microsoft Web Part and then deployed in a
Microsoft Office SharePoint Server 2007 or 2010 `Microsoft
Office SharePoint Server <http://sharepoint.microsoft.com/>`_.

When clicking on **Web Part** in the “Export” column of the Widgets
Status Table, you will have to fill in the URL of the auxiliary Web
Service of this widget. The default value of this field is the path of
the auxiliary Web container, embedded in the Server. Therefore, if the auxiliary
Web Service will be deployed to the embedded Web container, the default
value is correct.

After clicking **Ok**, Virtual DataPort will generate a zip file containing a
“webpart” file and an “xml” file that contain the necessary information
for displaying the view.

The new file can be obtained from:

-  In the URL :file:`https://{<host name of the Denodo server>}:9090/export/{<name of the widget>}.zip`

-  Or in the host where the Denodo server runs, in the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/webapps/export/{<name of the widget>}.zip`
