======================
Auxiliary Web Services
======================

.. note:: Publishing views as widgets is a deprecated feature and it may be removed in future
   major versions of the Denodo Platform.
   
   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

The Microsoft Web Parts, as they are not developed in Java, cannot
directly connect to the Virtual DataPort server, as the Java Portlets
do. Therefore, they require an auxiliary Web Service that will retrieve
the data from Virtual DataPort and send it back to these widgets.

Every time a widget is created, its auxiliary Web Service is also
created, but they still need to be deployed.

There are two options to deploy an auxiliary Web Service:

#. Deploy it in the embedded container: click on **Deploy** in the
   “Deployment” column of the Widgets Status panel.
#. Export it to an external J2EE Web Container such as IBM WebSphere Application Server: click on **war** in the “Deployment”
   column of the Widgets Status panel and follow the required steps to
   deploy a web application in your specific environment.

.. note:: Before using a Microsoft Web Part, you have to deploy its
   auxiliary Web Service. Otherwise, the widget will not display any data.
