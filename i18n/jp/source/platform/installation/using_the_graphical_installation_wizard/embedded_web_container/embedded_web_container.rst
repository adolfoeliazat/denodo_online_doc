======================
Embedded Web Container
======================

The Denodo Platform embeds the Apache Tomcat Web container, which is used,
among other things, to deploy some of the Denodo Platform administration
tools, the Data Catalog, etc.

When you select *Custom Installation* in the step 2 of the installation
wizard, you can configure the following settings of the Web container:

-  **HTTP port number**: port that the Web container will listen for
   incoming requests.
-  **Shutdown port number**: port that the Web container will listen for
   shutdown requests.
-  **JMX management port number**: port that the Web container will
   listen for requests from JMX monitoring tools such a Java VisualVM or
   JConsole. In addition, it is used by the other modules of the Denodo
   Platform to send requests to the web container such as deploy or
   undeploy a Denodo administration tool, a web service, etc.
-  **Auxiliary management port number**: auxiliary port used by the web
   container to communicate with its clients. I.e. JMX monitoring tools
   and other modules of the Denodo Platform.
