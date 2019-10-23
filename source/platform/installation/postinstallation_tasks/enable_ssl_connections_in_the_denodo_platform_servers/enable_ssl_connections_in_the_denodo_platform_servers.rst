=========================================================
Enable SSL/TLS in the Denodo Platform
=========================================================

This section explains how to secure with SSL (TLS) the connections between the
Denodo Platform servers, their administration tools and their clients.

Although in this document we will refer to SSL, you are actually enabling the encryption protocol TLS (Transport Layer Security), which is the successor of SSL.

SSL requires configuring certificate repositories. There are two types
of certificate repositories:

-  KeyStore
-  TrustStore

**KeyStore**

An application that listens to incoming SSL connections needs a public
key and a private key in order to allow clients to access the server. In
Java, these keys are stored in a repository called *KeyStore*.

**TrustStore**

During the initialization of an SSL connection, the server sends its SSL
certificate to the client. The client must then decide if it trusts this
or not. To do this, the client checks if the certificate has been signed
by a trusted certification authority (CA). The *TrustStore* is a
repository of the certificates of trusted certification authorities.

Every Java installation comes with a *TrustStore* that the JRE uses by
default (:file:`{<DENODO_HOME>}/jre/lib/security/cacerts` file). If the server’s
certificate is not signed by a trusted authority (i.e. one that is not
registered in the Java’s *TrustStore*), you have to store the
certificate of the authority, which has to be stored in the ``cacerts`` file of the Java Runtime Environment (JRE)
used to launch the Denodo Platform servers and their tools (:file:`{<DENODO_HOME>}/jre/lib/security/cacerts`).

The Java Runtime Environment (JRE) included with the Denodo Platform has a utility called
*keytool* that manages the *Certificate Repositories*.

These are the steps to enable SSL:

.. toctree::
   :maxdepth: 1
   
   obtaining_and_installing_an_ssl_certificate.rst
   enabling_ssl_in_denodo_platform_servers.rst
   enabling_https_in_the_embedded_apache_tomcat.rst
   enabling_ssl_in_denodo_platform_tools.rst
   enabling_ssl_for_external_clients.rst

SSL/TLS Versions Supported by the Denodo Platform Servers
=========================================================

When SSL (TLS) is enabled on the Denodo servers, the version of TLS used depends on the configuration on the components involved in the communication. Although for clarity purposes we refer to this as SSL, SSL is not actually used, only TLS.

.. csv-table:: Protocol used to encrypt the traffic between Denodo servers and other components
   :header: "Components Involved", "Protocol used to encrypt the traffic" 

   "Traffic between Denodo servers. For example, when Scheduler connects to the Virtual DataPort server to run a query.", "TLS 1.2"
   "Traffic between Denodo servers and the Denodo web container. For example, when a REST service receives a request that involves executing a query, this traffic is encrypted even if it occurs within the same host.", "TLS 1.2"
   "Traffic between Denodo servers and DF, Excel, JSON, XML and Custom data sources that use an HTTP route to an https service", "TLS 1.2"
   "Traffic between the administration tool and the Virtual DataPort server", "TLS 1.2"
   "Traffic between the Virtual DataPort server and JDBC client applications", "TLS 1.2"
   "Traffic between the Virtual DataPort server and ODBC client application", "TLS 1.2"
   "Traffic between the Denodo web container and its clients", "TLS 1.2"
