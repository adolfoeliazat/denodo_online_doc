===========================================
Enabling SSL/TLS in Denodo Platform Servers
===========================================

Follow these steps to secure with SSL/TLS the incoming connections with the servers of 
a Denodo Platform installation.

By doing this, the communications between the Denodo servers and its 
administration tools, and between the Denodo servers and its clients (JDBC and 
ODBC applications) will be encrypted.


1. Stop all the Denodo servers of this installation.

   Under some circumstances, the Denodo servers modify their configuration files when shutting down. Shutting them down first, prevents that you lose the changes that you are about to do.

#. Open the configuration files of the servers whose connections have to be
   secured:

+----------------------------------------+--------------------------------------------------------------------+
| Virtual DataPort server                | <DENODO_HOME>/conf/vdp/VDBConfiguration.properties                 |
+----------------------------------------+--------------------------------------------------------------------+
| Scheduler server                       | <DENODO_HOME>/conf/scheduler/ConfigurationParameters.properties    |
+----------------------------------------+--------------------------------------------------------------------+ 
| Scheduler Index server                 | <DENODO_HOME>/conf/arn-index/ConfigurationParameters.properties    |
+----------------------------------------+--------------------------------------------------------------------+
| ITPilot Browser Pool                   | <DENODO_HOME>/conf/iebrowser/IEBrowserConfiguration.properties     |
+----------------------------------------+--------------------------------------------------------------------+
| ITPilot Verification server            | <DENODO_HOME>/conf/maintenance/ConfigurationParameters.properties  |
+----------------------------------------+--------------------------------------------------------------------+

3. In all the files opened in the previous step, uncomment the following
   properties and change their values:

+--------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.enabled            | Set to ``true``                                                         |
+--------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.keyStore           | Path to the *KeyStore* that                                             |
|                                            | contains the private key for the Denodo Platform servers.               |
|                                            |                                                                         |
|                                            | E.g. ``C:/Denodo/denodo_server_key_store.jks``                          |
|                                            |                                                                         |
|                                            | Even if the Denodo servers run on Windows, the path separator           |
|                                            | has to be the forward slash (``/``).                                    |
+--------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.keyStorePassword   | Password of the *KeyStore* that contains the private key for the Denodo |
|                                            | Platform servers (this file is always password protected).              |
|                                            |                                                                         |
|                                            | This property can store this password in plain text or encrypted. We    |
|                                            | recommend encrypting it. To encrypt it, execute                         |
|                                            | ``{<DENODO_HOME>}/bin/encrypt_password``. You will have to              |
|                                            | enter the password of the keystore. Then, copy the output of this tool  |
|                                            | to this property.                                                       |
+--------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.                   | Set to ``true`` if you encrypted the password for the property          |
| \keyStorePassword.encrypted                | ``keyStorePassword``. Otherwise, set to ``false``.                      |
+--------------------------------------------+-------------------------------------------------------------------------+
| com.denodo.security.ssl.trustStore         | Leave these three properties commented.                                 |
|                                            |                                                                         |
| com.denodo.security.ssl.                   |                                                                         |
| \trustStorePassword                        |                                                                         |
|                                            |                                                                         |
| com.denodo.security.ssl.                   |                                                                         |
| \trustStorePassword.encrypted              |                                                                         |
+--------------------------------------------+-------------------------------------------------------------------------+

.. rubric:: Additional Information about the *TrustStore* (cacerts file)

By leaving the properties ``com.denodo.security.ssl.trustStore``, ``com.denodo.security.ssl.trustStorePassword`` and ``com.denodo.security.ssl.trustStorePassword.encrypted`` commented on these files, these modules will use the default *TrustStore* of the installation (:file:`{<DENODO_HOME>}/jre/lib/security/cacerts`).

It is possible to configure the Denodo servers to use a *TrustStore*
that is not the default one. However, **we do not recommend** 
doing so and that you go to the next section. The main reason to use the default *TrustStore* is that it makes the configuration of the Denodo servers easier.

In case you want to do it, uncomment the following properties in the configuration files listed above:

1. ``com.denodo.security.ssl.trustStore`` = Path to the *TrustStore*.   
      
   For example,    
   ``com.denodo.security.ssl.trustStore=<DENODO_HOME>/jre/lib/security/cacerts``

   Even if the Denodo servers run on Windows, the path separator has to be the forward slash (``/``).

2. ``com.denodo.security.ssl.trustStorePassword`` = Password of the 
   *TrustStore*. The default password of the *TrustStore* (:file:`{<DENODO_HOME>}/jre/lib/security/cacerts`) 
   is ``changeit``. The value of the password can be stored as clear text
   or encrypted. To obtain a valid encrypted value, use the ``{<DENODO_HOME>}/bin/encrypt_password`` script.
   
3. ``com.denodo.security.ssl.trustStorePassword.encrypted`` = Set it to ``true`` if the password of the *TrustStore* is encrypted.
