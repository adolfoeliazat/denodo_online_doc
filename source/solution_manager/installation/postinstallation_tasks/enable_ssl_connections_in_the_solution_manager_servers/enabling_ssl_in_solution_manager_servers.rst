============================================
Enabling SSL/TLS in Solution Manager Servers
============================================

Follow these steps to secure with SSL/TLS the incoming connections to the servers of 
a Solution Manager installation and https on the web container.

By doing this, the communications between the Solution Manager servers and their 
administration tools, and between the Solution Manager servers and its clients 
will be encrypted.

1. Stop all the components of the Solution Manager. The goal is to stop the web container. It is important to stop them all so the Denodo web container is stopped as 
   well. If for example, you leave the Data Catalog started, the web 
   container will not shut down and the changes in the file ``tomcat.properties``
   will not take effect.

2. Edit the file :file:`{<SOLUTION_MANAGER_HOME>}/conf/vdp/VDBConfiguration.properties`. Uncomment the following
   properties and change their values:
   
+--------------------------------------------+--------------------------------------------------------------------+
| com.denodo.security.ssl.enabled            | Set to ``true``                                                    |
+--------------------------------------------+--------------------------------------------------------------------+
| com.denodo.security.ssl.keyStore           | Path to the *KeyStore* that                                        |
|                                            | contains the private key for the Solution Manager. You can use the |
|                                            | same one you use on the installations of the Denodo Platform.      |
|                                            |                                                                    |
|                                            | E.g. ``C:/DenodoSolutionManager/denodo_server_key_store.jks``      |
|                                            |                                                                    |
|                                            | Even if the Solution Manager runs on Windows, the path separator   |
|                                            | has to be the forward slash (``/``).                               |
+--------------------------------------------+--------------------------------------------------------------------+
| com.denodo.security.ssl.keyStorePassword   | Password of the *KeyStore* that contains the private key of the    |
|                                            | Solution Manager (this file is always password protected).         |
|                                            |                                                                    |
|                                            | This property can store this password in plain text or encrypted.  |
|                                            | We  recommend encrypting it. To encrypt it, execute                |
|                                            | ``{<DENODO_HOME>}/bin/encrypt_password``. You will have to         |
|                                            | enter the password of the keystore. Then, copy the output of this  |
|                                            | tool to this property.                                             |
+--------------------------------------------+--------------------------------------------------------------------+
| com.denodo.security.ssl.                   | Set to ``true`` if you encrypted the password for the property     |
| \keyStorePassword.encrypted                | ``keyStorePassword``. Otherwise, set to ``false``.                 |
+--------------------------------------------+--------------------------------------------------------------------+
| com.denodo.security.ssl.trustStore         | Leave these three properties commented.                            |
|                                            |                                                                    |
| com.denodo.security.ssl.                   |                                                                    |
| \trustStorePassword                        |                                                                    |
|                                            |                                                                    |
| com.denodo.security.ssl.                   |                                                                    |
| \trustStorePassword.encrypted              |                                                                    |
+--------------------------------------------+--------------------------------------------------------------------+

3. Edit these files:

   i. :file:`{<SOLUTION_MANAGER_HOME>}/conf/solution-manager/SMConfigurationParameters.properties`
   #. :file:`{<SOLUTION_MANAGER_HOME>}/conf/license-manager/LMConfigurationParameters.properties`

   In these files, uncomment the following
   properties and change their values:

+--------------------------------------------+--------------------------------------------------------------------+
| server.ssl.key-store                       | Path to the *KeyStore* that contains the private key for the       |
|                                            | Solution Manager servers. You can use the same keystore you use in |
|                                            | the Virtual DataPort servers.                                      |
|                                            |                                                                    |
|                                            | E.g. ``C:/DenodoSolutionManager/denodo_server_key_store.jks``      |
|                                            |                                                                    |
|                                            | Even if the Denodo servers run on Windows, the path separator      |
|                                            | has to be the forward slash (``/``).                               |
+--------------------------------------------+--------------------------------------------------------------------+
| server.ssl.key-store-password              | Password of the *KeyStore* that contains the private key of the    |
|                                            | Solution Manager (this file is always password protected).         |
|                                            |                                                                    |
|                                            | This property can store this password in plain text or encrypted.  |
|                                            | We recommend encrypting it. To encrypt it, execute                 |
|                                            | ``{<DENODO_HOME>}/bin/encrypt_password``. You will have to         |
|                                            | enter the password of the keystore. Then, copy the output of this  |
|                                            | tool to this property.                                             |
+--------------------------------------------+--------------------------------------------------------------------+
| server.ssl.key-store-password.encrypted    | Set to ``true`` if you encrypted the password for the property     |
|                                            | ``keyStorePassword``. Otherwise, set to ``false``.                 |
+--------------------------------------------+--------------------------------------------------------------------+
| com.denodo.security.ssl.trustStore         | Leave these three properties commented.                            |
|                                            |                                                                    |
| com.denodo.security.ssl.                   |                                                                    |
| \trustStorePassword                        |                                                                    |
|                                            |                                                                    |
| com.denodo.security.ssl.                   |                                                                    |
| \trustStorePassword.encrypted              |                                                                    |
+--------------------------------------------+--------------------------------------------------------------------+

4. Edit the file
   :file:`{<SOLUTION_MANAGER_HOME>}/resources/apache-tomcat/conf/tomcat.properties`. Uncomment the following properties and set their value:

+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.tomcat.http.port              | If you want to disable http and only allow https connections, comment   |
|                                          | this property.                                                          |
|                                          |                                                                         |
|                                          | If you want to allow http and https, leave this property as is.         |
+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.tomcat.https.port             | Port listening to https connections. Check that this port is free in    |
|                                          | this host.                                                              |
|                                          |                                                                         |
|                                          | If you want clients to access the HTTPs interface without having to put |
|                                          | the port in the URL, set this to 443 instead of 9443. That way, the     |
|                                          | user will be able to access the HTTPs interface with a URL like         |
|                                          | \https://denodo-server.acme.com/denodo-restfulws instead of             |
|                                          | \https://denodo-server.acme.com:9443/denodo-restfulws.                  |
|                                          |                                                                         |
|                                          | Note that in Linux, processes that are not started by the *root* user   |
|                                          | cannot listen on ports under 1024. However, it is possible, using       |
|                                          | *iptables*, to redirect the data to port 443 to the port 9443.          |
+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.enabled          | Set to ``true``                                                         |
+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.keyStore         | Path to the *KeyStore* that contains the certificate for the Solution   |
|                                          | Manager.                                                                |
|                                          |                                                                         |
|                                          | For example,                                                            |
|                                          | ``com.denodo.security.ssl.keyStore=                                     |
|                                          | C:/DenodoSolutionManager/denodo_server_key_store.jks``                  |
|                                          |                                                                         |
|                                          | Even if the Denodo servers run on Windows, the path separator has to be |
|                                          | the forward slash (/).                                                  |
+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.keyStorePassword | Password of the *KeyStore* that contains the certificate for the        |
|                                          | Solution Maanger. The value of the password can be stored as clear text |
|                                          | or encrypted. To obtain a valid encrypted value, use the                |
|                                          | ``{<DENODO_HOME>}/bin/encrypt_password`` script.                        |
+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.                 | Set to ``true`` if the password of the *KeyStore* is encrypted.         |
| \keyStorePassword.encrypted              |                                                                         |
+------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.trustStore       | Leave these three properties commented.                                 |
|                                          |                                                                         |
| com.denodo.security.ssl.                 |                                                                         |
| \trustStorePassword                      |                                                                         |
|                                          |                                                                         | 
| com.denodo.security.ssl.                 |                                                                         |
| \trustStorePassword.encrypted            |                                                                         |
+------------------------------------------+-------------------------------------------------------------------------+
   
5. Edit the file :file:`{<SOLUTION_MANAGER_HOME>}/resources/apache-tomcat/conf/server.xml`

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


6. Edit the file :file:`{<SOLUTION_MANAGER_HOME>}/resources/apache-tomcat/webapps/solution-manager-web-tool/WEB-INF/classes/ConfigurationParameters.properties`.
   
   Change the value of the property ``com.denodo.solutionmanager.security.ssl.enabled`` to ``true``
      
#. Start the Denodo Platform servers.
   
   It is important to stop them all before any change (step #1) so the Denodo web container is stopped as 
   well.
 
#. To check that HTTPs was enabled successfully, start the Solution Manager Administration Tool and all the other modules. Then, go to 
   \https://denodo-solution-manager.acme.com:9443/solution-manager-web-tool and log in. (9443 is the default value of the 
   property ``com.denodo.tomcat.https.port``). 

.. important:: After enabling SSL/TLS on the Solution Manager, you need to change the configuration of all the Denodo servers that connect to this installation. This is to indicate they have to use SSL/TLS to connect to the Solution Manager. The section :ref:`Configuring the Connection to the License Manager` of the Installation Guide explains how to do this.

.. rubric:: Additional Information about the *TrustStore* (cacerts file)

By leaving the properties ``com.denodo.security.ssl.trustStore``, ``com.denodo.security.ssl.trustStorePassword`` and ``com.denodo.security.ssl.trustStorePassword.password`` commented on these files, these modules will use the default *TrustStore* of the installation (:file:`{<SOLUTION_MANAGER_HOME>}/jre/lib/security/cacerts`).

If the certificate is not signed by a trusted authority (i.e. one that is 
not registered in the JRE’s *TrustStore*), you have to import it into the
default *TrustStore* (:file:`{<SOLUTION_MANAGER_HOME>}/jre/lib/security/cacerts`). Note that you did that already when following the instructions of the previous section, to obtain the certificate.

It is possible to configure the Denodo servers to use a *TrustStore*
that is not the default one. However, **we do not recommend** 
doing so and that you go to the next section. The main reason to use the default *TrustStore* is that it makes the configuration of the Denodo servers easier.

In case you want to do it, uncomment the following properties in the configuration files listed above:

1. ``com.denodo.security.ssl.trustStore`` = Path to the *TrustStore*.   
      
   For example,    
   ``com.denodo.security.ssl.trustStore=<SOLUTION_MANAGER_HOME>/jre/lib/security/cacerts``

   Even if the Denodo servers run on Windows, the path separator has to be the forward slash (``/``).

2. ``com.denodo.security.ssl.trustStorePassword`` = Password of the 
   *TrustStore*. The default password of the *TrustStore* (:file:`{<SOLUTION_MANAGER_HOME>}/jre/lib/security/cacerts`) 
   is ``changeit``. The value of the password can be stored as clear text
   or encrypted. To obtain a valid encrypted value, use the ``{<DENODO_HOME>}/bin/encrypt_password`` script.
   
3. ``com.denodo.security.ssl.trustStorePassword.encrypted`` = Set it to ``true`` if the password of the *TrustStore* is encrypted.

4. Change these properties in the file :file:`{<SOLUTION_MANAGER_HOME>}/conf/solution-manager/denodo-monitor/ConfigurationParametersGeneral.template`.

5. Change these properties in the file :file:`{<SOLUTION_MANAGER_HOME>}/conf/solution-manager/SMConfigurationParameters.properties`.
