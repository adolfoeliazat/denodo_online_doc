===============
Export a Widget
===============

.. note:: Publishing views as widgets is a deprecated feature and it may be removed in future
   major versions of the Denodo Platform.
   
   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

This command exports an existing widget to a particular widget
technology. Depending on the target, the output will be different:

-  Portlets JSR 168 and 286: Output is a *.war* file that can be
   deployed in any standard Portlet server.
-  Microsoft Web Part: Output is a *.zip* containing a *webpart* file for the
   deployment in Microsoft Office SharePoint and an XML file containing
   information about the exported element.

.. important:: In order to use the Microsoft Web Part, their auxiliary
   Web Services must be deployed. See section :ref:`Deployment and Export of
   Auxiliary Web Services`.

.. code-block:: bnf
   :caption: Syntax of the EXPORT WIDGET statement
   :name: Syntax of the EXPORT WIDGET statement

   EXPORT { PORTLETJSR168 | PORTLETJSR286 } FROM WIDGET
       <widget_name:identifier>
       NAME = <target_file_name:literal>
       URI = <vdp_server_uri:literal>
       LOGIN = <vdp_user_name:literal>
       PASSWORD = <literal> [ ENCRYPTED ]

   EXPORT WEBPART FROM WIDGET identifier_name:widgetName
       NAME = <target_file_name:literal>
       URI = <vdp_auxiliary_web_service_url:literal>


-  ``URI``: Its meaning depends on the target widget:

   -  Portlet: the URI of the Virtual DataPort server that the portlet will
      connect to in order to retrieve the data.
   -  Microsoft Web Part: the URI of the auxiliary Web Service that it will
      connect to in order to obtain the data.

-  ``LOGIN`` and ``PASSWORD``: Credentials to connect to Virtual DataPort.
