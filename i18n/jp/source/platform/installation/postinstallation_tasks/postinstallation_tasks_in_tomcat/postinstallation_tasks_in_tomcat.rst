================================================================
Post-Installation Tasks: Web Container
================================================================

This page contains the post-installation tasks for the web container included in Denodo (Apache Tomcat):

.. contents::
   :local:
   
.. important:: Before doing any of the changes described here, stop all the Denodo Platform servers.

   The goal is to stop the web container of Denodo. It is important to stop them all so the Denodo web container is stopped as well. If for example, you leave the Data Catalog started, the web container will not shut down and the changes will not take effect.
    
Hide Name and Version in Error Pages
====================================

By default, error pages returned by this web container include its version. We recommend hiding this information to avoid disclosing this information to external users.

To do this, edit the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/server.xml`. Search for the element ``Host`` and inside it, add the following line:
    
.. code-block:: xml
  
  <Valve className="org.apache.catalina.valves.ErrorReportValve" showReport="false" showServerInfo="false" />
  
After this change, this section of the file should look like this:

.. code-block:: xml

  <Host autoDeploy="false" deployOnStartup="false" name="localhost" unpackWARs="true">
  
     <Valve className="org.apache.catalina.valves.ErrorReportValve" showReport="false" showServerInfo="false" />
     
     <!-- More elements --> 

Secure the "Export" Endpoint of the Web Container
====================================================================

This section explains how to secure the endpoint :file:`https://denodo-server.acme.com:9090/export`.

With Virtual DataPort you can publish data services (SOAP and REST web services). Usually, you deploy them on the web container embedded in Denodo. However, you can export a web service to a war file, which you can deploy it on any Java web container (IBM WebSphere, Oracle WebLogic...). When you export a web service to a war file, it is available to download in the URL :file:`https://{<host name of the Denodo server>}:9090/export`.

By default, this endpoint does not require authentication. We recommend disabling this endpoint or enable user/password authentication to access it.

To disable this endpoint, edit the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/webapps/export/WEB-INF/web.xml` and search for this block of XML.

.. code-block:: xml
   
   <servlet-mapping>
       <servlet-name>listing</servlet-name>
       <url-pattern>/*</url-pattern>
   </servlet-mapping>  
   
Then, add the following block **below** ``</servlet-mapping>``:
   
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
      
After this, the access to \https://{<host name of the Denodo server>}:9090/export will be forbidden. When you export a data service to a 
war file, you will have to copy the file from the host where the Virtual DataPort server runs, from the path :file:`{<DENODO_HOME>}/resources/apache-tomcat/webapps/export`.

|

If instead of disabling this endpoint, you want to enable authentication for it, follow these steps:

1. Edit the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/tomcat-users.xml` and add the following:

   .. code-block:: xml

      <role rolename="tomcat"/>
      <user username="export_endpoint" password="PASSWORD_FOR_EXPORT_ENDPOINT" roles="tomcat" />

   The file should end up looking like

   .. code-block:: xml

      <tomcat-users>
          <role rolename="tomcat"/>
          <user username="export_endpoint" password="PASSWORD_FOR_EXPORT_ENDPOINT" roles="tomcat" />
      </tomcat-users>

   In the attributes ``username`` and ``password`` you can put the user name and password you want. These are the credentials the users will have to provide for this endpoint.
    
   You can add as many entries "<user>" as needed. In all of them, the value of the attribute "roles" has to be "tomcat".

2. Edit the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/webapps/export/WEB-INF/web.xml`. Search for this block of XML.

   .. code-block:: xml
   
       <servlet-mapping>
           <servlet-name>listing</servlet-name>
           <url-pattern>/*</url-pattern>
       </servlet-mapping>  
   
   Add the following block **below** ``</servlet-mapping>``:
   
   .. code-block:: xml
   
      <security-constraint>
          <web-resource-collection>
              <url-pattern>/*</url-pattern>
          </web-resource-collection>
          <auth-constraint>
              <role-name>tomcat</role-name>
          </auth-constraint>
      </security-constraint>
      <login-config>
          <auth-method>DIGEST</auth-method>
      </login-config>

3. Edit the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/server.xml`. Search for the element ``Host`` and inside it, add the following line:

   .. code-block:: xml
      
      <Valve className="org.apache.catalina.valves.ErrorReportValve" showReport="false" showServerInfo="false" />
      
   After this change, this section of the file should look like this:
   
   .. code-block:: xml
   
      <Host autoDeploy="false" deployOnStartup="false" name="localhost" unpackWARs="true">
      
         <Valve className="org.apache.catalina.valves.ErrorReportValve" showReport="false" showServerInfo="false" />
         
         <!-- More elements --> 


After this, the users that want to connect to this endpoint will have to provide the user and password you entered in ``tomcat-users.xml``.

Enable Authentication on the Monitoring Interface
=================================================

By default, the monitoring interface - Java Management Extensions (JMX) - of the Denodo web container (Apache Tomcat) 
does not require authentication to connect to it. Note that by default,
only applications that run in the same host as the Denodo server can connect to this interface. 
That is because the value of the property 
``com.denodo.tomcat.jmx.rmi.host`` in 
:file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/tomcat.properties` is
``localhost`` by default so only connections from that host are allowed.


We recommend enabling authentication in the monitoring interface of the web container, even though only local connections can connect.
To do this, follow these steps:

1. Stop all the Denodo Platform servers. The goal is to stop the web container of Denodo. It is important to stop them all so the Denodo web container is stopped as 
   well. If for example, you leave the Data Catalog started, the web 
   container will not shut down and the changes in the file ``tomcat.properties``
   will not take effect.

#. Edit the file
   :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/tomcat.properties` and
   set the property ``com.denodo.tomcat.jmx.auth.enabled`` to ``true``.

#. Edit the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/jmxremote.access` (this is the value of the property  
   ``com.denodo.tomcat.jmx.auth.access.file``).
   
   Make sure this file contains at least one line for the role ``controlRole`` with the 
   ``readwrite`` access level. That is, at least one line of this file is like this:
   
   ::
   
      controlRole readwrite   
   
   Any other role definitions are optional. See
   https://docs.oracle.com/javase/8/docs/technotes/guides/management/agent.html
   for details on JMX access files.

#. Edit the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/jmxremote.password` (this is the value of the property  
   ``com.denodo.tomcat.jmx.auth.password.file``).
   
   In this file, the line that starts with ``controlRole`` contains the password of that user.
   
   So, if the line is like this:
   
   :: 
     
      controlRole denodojmx
   
   The default password is "denodojmx". That means that, for a monitoring application that wants to monitor the web container, the user name is ``controlRole`` and the password ``denodojmx``.
   
   To change the password, replace "denodojmx" with the desired password.
   
   This file must contain an entry for all the roles defined in the file "jmxremote.access". See
   https://docs.oracle.com/javase/8/docs/technotes/guides/management/agent.html
   for details on JMX password files.

#. Change the privileges of the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/jmxremote.password`
   so it can only be read by the same user account that starts the
   Denodo servers.

   To do this, execute these commands:
      
   -  On Linux, run the following from the user account that starts the Denodo servers:
      
   .. code-block:: bash
      
      chmod 600 <DENODO_HOME>/resources/apache-tomcat/conf/jmxremote.password
         
   -  On Windows, right-click the icon **Command Prompt** of the *Windows menu* and click **Run as administrator**.
      
      In this prompt, run the following commands (replace ``<denodo_user>`` with the user account with which the Denodo 
      servers are started):
      
   .. code-block:: batch
      
      cd <DENODO_HOME>\resources\apache-tomcat\conf\
      icacls jmxremote.password /setowner <denodo_user>
      icacls jmxremote.password /grant <denodo_user>:F
      icacls jmxremote.password /inheritance:r
