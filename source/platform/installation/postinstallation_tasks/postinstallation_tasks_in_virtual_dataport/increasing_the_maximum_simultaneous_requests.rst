============================================
Increasing the Maximum Simultaneous Requests
============================================

The Denodo Platform embeds the Apache Tomcat Web container that Virtual
DataPort uses, among other things, to deploy SOAP and REST Web services.
If you expect these Web services to receive a high number of concurrent
requests, consider increasing the maximum number of threads that Tomcat
will create to attend requests. To do this, follow these steps:

#. Open the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/server.xml`

#. Look for the attribute ``maxThreads`` and replace its default value
   (``150``) with a higher one. For example, ``300``.

   There are two occurrences of the ``maxThreads`` attribute, one for the
   non-SSL *Connector* and another one for the SSL *Connector*. You can
   have a different value for each one.

#. From the Administration Tool, do the following:

   a. Open the wizard “Concurrent Requests” on the menu “Administration >
      Server configuration”.
   b. Make sure that *if* “Limit concurrent requests” is “On”, the value of
      “Max concurrent requests” is greater than or equal to the
      ``maxThreads`` attribute. Otherwise, Tomcat will process the
      requests, but Virtual DataPort will not be able to attend them.
      
      If you have enabled SSL on Tomcat (explained in the section :doc:`Enabling
      HTTPS in the Embedded Apache Tomcat<../enable_ssl_connections_in_the_denodo_platform_servers/enabling_https_in_the_embedded_apache_tomcat>`),
      “Max concurrent requests” has to be greater than or equal to the sum
      of the ``maxThreads`` attribute of the non-SSL connector and the SSL
      connector.


#. To apply these changes, stop the Virtual DataPort server and all the
   Denodo administration tools. Once they are all stopped, start them
   again.

Each incoming request requires a thread for the duration of that
request. If Tomcat receives more simultaneous requests than the number
of available request processing threads, Tomcat creates additional
threads up to ``maxThreads``. If Tomcat still receives more simultaneous
requests, they are stacked up, up to the value of the ``acceptCount``
attribute of the ``Connector`` element. Any further simultaneous
requests will receive “connection refused” errors.
