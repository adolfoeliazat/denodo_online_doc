====================================================================
Disable "Export" Endpoint of the Web Container
====================================================================

This section explains how to deny access to the endpoint :file:`https://{<host name of the Solution Manager>}:19090/export`.

The Solution Manager includes a Virtual DataPort server, which includes a web container.
The Solution Manager relies on this Virtual DataPort server to authenticate users. By default, this web container
exposes the endpoint "export". However, this endpoint will never be used so we recommend disabling it.

To disable this endpoint, follow these steps:

1. Stop all the components of the Solution Manager installation. The goal is to stop the web container of Denodo. It is important to stop them all so the 
   Denodo web container is stopped as 
   well. If for example, you leave the Solution Manager administration tool started, the web 
   container will not shut down and the changes in the file ``tomcat.properties``
   will not take effect.

#. Edit the file :file:`{<SOLUTION_MANAGER_HOME>}/resources/apache-tomcat/webapps/export/WEB-INF/web.xml`. Search for this block of XML.

   .. code-block:: xml
   
      <servlet-mapping>
          <servlet-name>listing</servlet-name>
          <url-pattern>/*</url-pattern>
      </servlet-mapping>  
   
   Add the following block **below** ``</servlet-mapping>``:
   
   .. code-block:: xml
      
      <filter>
          <filter-name>Remote IP Filter</filter-name>
          <filter-class>org.apache.catalina.filters.RemoteHostFilter</filter-class>
          <init-param>
              <param-name>deny</param-name>
              <param-value>.*</param-value>
          </init-param>
      </filter>

      <filter-mapping>
          <filter-name>Remote IP Filter</filter-name>
          <url-pattern>*</url-pattern>
      </filter-mapping>

#. Edit the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/server.xml`. Search for the element ``Host`` and inside it, add the following line:

   .. code-block:: xml
      
      <Valve className="org.apache.catalina.valves.ErrorReportValve" showReport="false" showServerInfo="false" />
      
   After this change, this section of the file should look like this:
   
   .. code-block:: xml
   
      <Host autoDeploy="false" deployOnStartup="false" name="localhost" unpackWARs="true">
      
         <Valve className="org.apache.catalina.valves.ErrorReportValve" showReport="false" showServerInfo="false" />
         
         <!-- More elements --> 

After this, the access to :file:`https://{<host name of the Solution Manager>}:19090/export` will be forbidden. If a client tries to access this URL,
the container will return an "Unauthorized" error.
