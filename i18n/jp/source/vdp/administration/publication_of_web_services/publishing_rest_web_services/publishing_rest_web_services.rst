============================
Publishing REST Web Services
============================

.. toctree::
   :hidden:
   
   resources_tab.rst
   settings_tab_rest.rst
   advanced_tab_rest.rst   

A REST Web service publishes a set of views and associations and makes
them available to external applications that cannot use the JDBC or ODBC
interfaces.

REST Web services provide similar features and work pretty much in the same
way as the RESTful Web service described in the section :ref:`RESTful Web service`
(i.e. the service at \http://denodo-dv1-prod.denodo.com:9090/denodo-restfulws).
However, there are a few features that the REST web services have and the RESTful web service do not:

  -  You can provide access to a limited set of views, instead of to all the views.
  -  You can rename the names of the fields of the published views.
  -  Set mandatory input fields.
  -  Support for RSS.
  -  …


To create a REST Web service, right-click on the view you want to
publish, in the Server Explorer and click **REST Web service** on the menu
**New** > **Data service** or click **REST Web service** in the menu
**File** > **New** > **Data service**.

The “Create REST Web service” dialog has the following tabs:

-  **Resources**: lists the views and associations published by the Web
   service. You can add more by dragging a view. See section :ref:`Resources
   Tab`.
-  **Settings**: manages the configuration of the REST Web service. See
   section :ref:`Settings Tab (REST)`.
-  **Advanced**: manages the parameters of the connection between the
   Web service and the Virtual DataPort Server and other advanced
   settings. See section :ref:`Advanced Tab (REST)`.
-  **Metadata**: select the target folder of the Web service and provide
   a description for it.

After configuring everything, click **Save** to create the Web service.
The Tool will display the “Web service container status” table that
lists the existing Web services. Then, you can deploy the Web service in
the embedded Web container or generate the war file and deploy it in an
external Web container (see section :ref:`Web Service Container Status
Table`).

