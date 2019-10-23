============================================
Enabling HTTPS in the Embedded Apache Tomcat
============================================

The Denodo Platform embeds the Apache Tomcat web container to host its
web applications and web services. The communications between clients
and the web applications running in the Apache Tomcat embedded in the
Denodo Platform can be secured with HTTPS. The applications running in
this web container are:

-  ITPilot Administration Tool
-  Scheduler Administration Tool
-  Web Services published using Virtual DataPort
-  Data Catalog
-  Diagnostic & Monitoring Tool

To enable HTTPS, do the following:

1. Stop all the Denodo servers of this installation. The goal is to stop the web container of Denodo. It is important to stop them all so the Denodo web container is stopped as 
   well. If for example, you leave the Data Catalog started, the web 
   container will not shut down and the changes in the file ``tomcat.properties``
   will not take effect even if you restart the other components of Denodo.

#. Edit the file
   :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/tomcat.properties`.
   
   Uncomment the following properties and set their value:

+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.tomcat.http.port              | Port listening to http - not https - connections. Check that this port  |
|                                          | is available in this host.                                              |
|                                          |                                                                         |
|                                          | To disable http and only allow https, comment this                      |
|                                          | property.                                                               |
|                                          |                                                                         |
|                                          | To allow http and https, leave this property uncommented.               |
|                                          |                                                                         |
|                                          | If you want users to access the http interface - not https - without    |
|                                          | having to put the port in the URL, set this 80 instead of 9090.         |
|                                          | That way, the                                                           |
|                                          | user will be able to access the web container interface with a URL like |
|                                          | \http://denodo-server.acme.com/denodo-restfulws instead of              |
|                                          | \http://denodo-server.acme.com:9090/denodo-restfulws.                   |
|                                          |                                                                         |
|                                          | See the note below this table.                                          |
+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.tomcat.https.port             | Port listening to https connections. Check that this port is free in    |
|                                          | this host.                                                              |
|                                          |                                                                         |
|                                          | If you want users to access the HTTPs interface without having to put   |
|                                          | the port in the URL, set this to 443 instead of 9443. That way, the     |
|                                          | user will be able to access the HTTPs interface with a URL like         |
|                                          | \https://denodo-server.acme.com/denodo-restfulws instead of             |
|                                          | \https://denodo-server.acme.com:9443/denodo-restfulws.                  |
|                                          |                                                                         |
|                                          | See the note below this table.                                          |
+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.enabled          | Set to ``true``                                                         |
+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.keyStore         | Path to the *KeyStore* file that contains the private key for the       |
|                                          | Denodo Platform servers.                                                |
|                                          |                                                                         |
|                                          | For example,                                                            |
|                                          | ``com.denodo.security.ssl.keyStore=                                     |
|                                          | C:/denodo/denodo_server_key_store.jks``                                 |
|                                          |                                                                         |
|                                          | Even if the Denodo servers run on Windows, the path separator has to be |
|                                          | the forward slash (/).                                                  |
+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.keyStorePassword | Password of the *KeyStore* that contains the private key for the Denodo |
|                                          | Platform servers (this file is always password protected).              |
|                                          |                                                                         |
|                                          | This property can store this password in plain text or encrypted. We    |
|                                          | recommend encrypting it. To encrypt it, execute                         |
|                                          | ``{<DENODO_HOME>}/bin/encrypt_password``. You will have to              |
|                                          | enter the password of the keystore. Then, copy the output of this tool  |
|                                          | to this property.                                                       |
+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.                 | Set to ``true`` if you encrypted the password for the property          |
| \keyStorePassword.encrypted              | ``keyStorePassword``. Otherwise, set to ``false``.                      |
+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.trustStore       | Leave these three properties commented.                                 |
|                                          |                                                                         |
| com.denodo.security.ssl.                 |                                                                         |
| \trustStorePassword                      |                                                                         |
|                                          |                                                                         | 
| com.denodo.security.ssl.                 |                                                                         |
| \trustStorePassword.encrypted            |                                                                         |
+------------------------------------------+-------------------------------------------------------------------------+

.. important:: In Linux, only processes that are started by the *root* user can listen to connections on ports below 1024.

   To overcome this limitation, you can use *iptables* to redirect the incoming traffic
   from port 443 (default https port) to 9443, and from 80 (default http port) to 9090. Alternatively, you can start the
   Denodo Platform servers with the *root* account.

3. Edit the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/server.xml`

   a. Uncomment the SSL connector. I.e. Search the “Connector” element that
      starts with <Connector port="${com.denodo.tomcat.\ **https**.port}"
      and remove the ``<--`` and ``-->`` characters that surround it.
   b. To disable the access through HTTP and only allow HTTPs connections,
      comment the “Connector” element that starts with <Connector
      port="${com.denodo.tomcat.\ **http**.port}" with ``<--`` and ``-->``.
      For example,
      
.. code-block:: xml

   <!--
      <Connector port="${com.denodo.tomcat.http.port}"
         maxThreads="150" minSpareThreads="25"
         redirectPort="${com.denodo.tomcat.https.port}"
         connectionTimeout="20000"
         URIEncoding="UTF-8"/>
   -->

..
 
   Check the documentation of `Apache Tomcat <https://tomcat.apache.org/tomcat-8.0-doc/config/http.html#SSL_Support>`_ to know how to change the default SSL/TLS settings of the web container: to limit the ciphers, enable client authentication, etc.
   
4. When enabling TLS/SSL on the Tomcat, a user that can to connect to the host where the Denodo servers run, with the user account with which you launch the Denodo servers, will be able to see the password of the
   keystore, in the list of running processes. That is because by default, this password is passed as a parameter to the script that starts the web container.
   
   To avoid this (i.e. adding this password to the command line of Tomcat), follow these steps:

   i. Edit the file
      :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/tomcat.properties` and 
      uncomment the property ``com.denodo.security.management.jmxremote.ssl.config.file``. Only uncomment the property, leave the default value as is (i.e. :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/jmxssl.properties`).

   #. Change the privileges of the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/conf/jmxssl.properties`
      so it can only be read by the same user account that starts the
      Denodo servers.
      
      To do this, execute these commands:
      
      -  On Linux, run the following from the user account that starts the Denodo servers:
      
      .. code-block:: bash
      
         chmod 600 <DENODO_HOME>/resources/apache-tomcat/conf/jmxssl.properties
         
      -  On Windows, right-click the icon **Command Prompt** of the *Windows menu* and click **Run as administrator**.
      
         Run the following commands (replace ``<denodo_user>`` with the user account with which the Denodo 
         servers are started):

      .. code-block:: batch
   
         cd <DENODO_HOME>\resources\apache-tomcat\conf\
         icacls jmxssl.properties /setowner <denodo_user>
         icacls jmxssl.properties /grant <denodo_user>:F
         icacls jmxssl.properties /inheritance:r

      If you do not change these privileges, the web container will not start.

5. Start the Denodo servers you use.

#. To check that https was enabled successfully, open the URL 
   \https://denodo-server.acme.com:9443/denodo-restfulws/admin (9443 is the default value of the 
   property ``com.denodo.tomcat.https.port``). 


.. rubric:: Enabling HTTPS on the Web Container but not in the Virtual DataPort Server

You can enable https on the web container without enabling TLS/SSL in the Virtual DataPort server. If you did not enable SSL/TLS on the Virtual DataPort server, comment the property ``com.denodo.security.ssl.enabled`` of the file ``tomcat.properties``.

.. rubric:: Additional Information about the *TrustStore* (cacerts file)

On the file ``tomcat.properties``, we recommend leaving the properties ``com.denodo.security.ssl.trustStore``, ``com.denodo.security.ssl.trustStorePassword`` and ``com.denodo.security.ssl.trustStorePassword.encrypted`` commented. That way, the web container uses the default *TrustStore* of the installation (i.e. :file:`{<DENODO_HOME>}/jre/lib/security/cacerts`).
   
It is possible to configure the Denodo web container to use a *TrustStore*
that is not the default one. However, we do **not** recommend 
doing so because it makes the management of the Denodo servers harder because 
you have to maintain a new *TrustStore* file.

To use a different *TrustStore*, uncomment these properties:

-  ``com.denodo.security.ssl.trustStore`` = Path to the TrustStore.   

   For example,    
   ``com.denodo.security.ssl.trustStore=c:/denodo/custom_cacerts``

   Even if the Denodo servers run on Windows, the path separator has to be the forward slash (/).
   
-  ``com.denodo.security.ssl.trustStorePassword`` = Password of the *TrustStore*. 
   The default password of a *TrustStore* is ``changeit``. The value of the password can be stored as clear text
   or encrypted. To obtain a valid encrypted value, use the ``{<DENODO_HOME>}/bin/encrypt_password`` script.
   
-  ``com.denodo.security.ssl.trustStorePassword.encrypted`` = Set it to ``true`` if the password of the *TrustStore* is encrypted.
   
